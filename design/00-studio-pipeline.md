# The studio pipeline: how both tracks get built with about an hour of your time a week

Status: design, 2026-11-01. Applies to Track A (public-records data product) and Track B (original-IP idle game). The evidence behind every choice here is in `WHAT-TO-BUILD.md` Section 5 and `research/02-build-and-test-methods.md`.

## 1. Principles

1. **Verification first.** The first two weeks of each track build oracles, not features: the test harness, the simulator or the data-quality checks the agents will have to satisfy for the following months.
2. **The implementing agent never owns its oracle.** Tests are written by a different context and protected by hooks. Snapshot and golden-file updates are human-only.
3. **Small, sized issues.** One issue per session, one PR per issue. Anything larger than about three hours of agent work goes back to the planner to be split.
4. **Fresh context reviews.** A reviewer that never saw the implementation conversation, with a different prompt from the builder.
5. **Humans merge.** You approve from your phone. Branch protection makes this mechanical, not optional.
6. **Everything runs on a schedule.** Nothing waits for you except decisions.
7. **Fix the generating process, not the output.** When an agent produces a bad pattern, the fix is a hook, a lint rule, a skill or a spec line, not a hand patch.

## 2. Repositories

Recommended: three repositories.

| Repo | Role | Contents |
|---|---|---|
| `IdeasMoney` (this one) | Studio control plane | `research/`, `design/`, `DECISIONS.md`, `digests/`, portfolio-level `ROADMAP.md` |
| Track A repo | Data platform, API, site | Python services, source adapters, schemas, docs site |
| Track B repo | Game monorepo | TypeScript packages: core, content, ui, sim, tools; web and desktop apps |

Why separate: different secrets and deploy targets, each scheduled task runs against one repo clone, CI stays fast, and the game repo will carry large fixtures and art. A monorepo under `tracks/` here would work but mixes secrets and slows CI. This is open question Q-001 in `DECISIONS.md`; the recommendation is separate repos.

## 3. Files every track repo carries

| File | Purpose | Who edits |
|---|---|---|
| `SPEC.md` | Vision, users, non-goals, pricing hypothesis, legal constraints | You and me once; revised on decisions |
| `CONSTITUTION.md` | Non-negotiables (below) | Fixed; changes need your sign-off |
| `ROADMAP.md` and `features.json` | Milestones; a JSON list of features with `id`, `title`, `acceptance`, `passes: false`. JSON because models overwrite it less than Markdown | Planner routine |
| `CLAUDE.md` | Under 60 lines: commands, conventions, definition of done, where things live | Me; kept short, hard rules go to hooks |
| `DECISIONS.md` | Numbered log plus open questions for you | Planner writes, you answer |
| `.claude/settings.json` | Hooks (Section 4) | Me |
| `.claude/skills/` | Playbooks: add-feature, add-source (A), add-content (B), rebalance (B), release, triage | Me, then the agents propose edits via PR |
| `.github/workflows/ci.yml` | Gates (Section 7) | Me |
| `.github/ISSUE_TEMPLATE/task.yml` | Goal, acceptance criteria, out of scope, test plan, size | Planner fills |
| `docs/adr/` | Architecture decision records, one page each | Builder writes when it makes a structural choice |

**Constitution, both tracks.** Never edit a test to make it pass. Never update a snapshot, golden file or recorded fixture; those commits are human-only. Never delete, skip or quarantine a failing test. Never commit secrets, another game's data or assets, or scraped personal data outside the approved schema. Never claim compliance, affiliation, or guarantees in copy. Disclose AI-generated player-facing content. Respect robots.txt and per-source rate limits. Stop and label `needs-human` when a task requires a judgment the spec does not cover. Prefer deleting code to adding it.

## 4. Hooks: deterministic gates inside the agent loop

Preferences live in `CLAUDE.md`. Rules that must hold every time live here, because a hook cannot be talked out of anything.

```json
{
  "hooks": {
    "PreToolUse": [
      { "matcher": "Edit|Write|MultiEdit",
        "hooks": [{ "type": "command", "command": "scripts/guard-paths.sh" }] }
    ],
    "PostToolUse": [
      { "matcher": "Edit|Write|MultiEdit",
        "hooks": [{ "type": "command", "command": "scripts/gate-fast.sh" }] }
    ],
    "Stop": [
      { "hooks": [{ "type": "command", "command": "scripts/gate-stop.sh" }] }
    ]
  }
}
```

- `guard-paths.sh` exits 2 (block, with the reason fed back) when the target path matches a protected glob: `tests/**` while the session is in implement phase (test-first enforcement, or adopt TDD Guard / Probity for this), `**/__snapshots__/**`, `**/golden/**`, `**/fixtures/recorded/**`, `migrations/**` unless the issue carries the `migration` label, `.env*`, `SPEC.md`, `CONSTITUTION.md`.
- `gate-fast.sh` runs format, lint, strict typecheck and the fast unit suite (target under 60 seconds) and returns failures to the agent after every edit.
- `gate-stop.sh` runs the fast gate plus an evidence check: the PR body or session notes must contain the commands run and their output. Exit 2 keeps the agent working. Claude Code caps consecutive Stop blocks, so a stuck session ends with a `blocked` label rather than looping forever.

## 5. Issue flow

Labels: `ready`, `in-progress`, `pr-open`, `needs-human`, `blocked`, `done`; type labels `feature`, `bug`, `data-source`, `content`, `chore`, `migration`; size labels `S` (under one hour), `M` (under three hours), `L` (must be split).

The builder picks the highest-priority `ready` issue sized S or M, moves it to `in-progress`, and works in a branch named `claude/<issue>-<slug>`. A PR moves it to `pr-open`. Merge moves it to `done`. Anything the builder cannot resolve from the spec becomes `needs-human` with a specific question, and shows up in your digest.

## 6. Scheduled cloud tasks

All times America/Phoenix. Each run is a fresh cloud session on a fresh clone.

| Routine | Schedule | What it does | Output |
|---|---|---|---|
| Builder A, Builder B | Nightly, 01:00 and 02:30 | Pick one `ready` issue. `/goal "<acceptance criteria> or stop after 40 turns"`. Red: write failing tests. Green: implement. Self-check: run the app or simulator, E2E, mutation run on changed files. Open a PR with commands, output and screenshots. Subscribe to the PR and fix CI until green. | One PR per track per night |
| Reviewer | On PR opened | Fresh session; `/code-review` and `/security-review` with a prompt that differs from the builder's; flag only serious findings; label `needs-human` if severity is high | Review comments |
| Planner | Sundays, 18:00 | Read merged PRs, failed runs, metrics. Update `features.json`. Propose and size next week's issues. Write questions to `DECISIONS.md`. Write `digests/YYYY-WW.md` and email it to you. | The digest you read |
| Maintenance | Nightly, 04:00 | Surviving mutants to issues; dependency bumps with vulnerability scan; docs drift PRs; flaky-test reports (never auto-skip) | Small PRs and issues |
| Watchdog A | Every 6 hours | Freshness and coverage SLAs, crawl errors, API error rate, billing webhooks | Issues |
| Watchdog B | Daily | Nightly balance simulation against target bands; content lint; crash reports; itch and store comments | Issues |
| Release | On tag | Changelog, build, deploy, smoke test, store checklist | A release |

Builder prompt skeleton:

```
You are the nightly builder for <repo>. Read CLAUDE.md, CONSTITUTION.md, SPEC.md
and features.json. Pick the highest-priority issue labeled ready, sized S or M.
Comment on it that you are starting. Create branch claude/<id>-<slug>.
Write failing tests for the acceptance criteria first and show them failing.
Implement until green without touching tests. Run the full gate. Run the
app/simulator and capture evidence. Open a PR titled "<id>: <title>" whose
body lists: acceptance criteria with checkboxes, commands run with output,
screenshots, risks. Label the issue pr-open. If anything requires a judgment
the spec does not cover, stop, label needs-human, and write the question.
```

## 7. CI gates

Common to both tracks: format and lint; strict typecheck; unit and property-based tests; mutation score on changed files at or above 50% (70% in `core/` for Track B and in `normalize/` and `billing/` for Track A); Semgrep with the default ruleset plus repo rules; OSV-Scanner on dependencies; Gitleaks; Playwright end-to-end with axe accessibility checks on any web UI; size and performance budgets; a diff-size gate that fails PRs over about 600 changed lines unless labeled `large-change-approved`.

Branch protection on `main`: CI green, reviewer posted, one human approval, squash merge, no direct pushes.

Track-specific oracles are in each track's design document.

## 8. AI review

Two reviewers with different briefs, each in a context that never saw the implementation: one for correctness and security, one for spec conformance and simplicity. Both are told to flag only serious issues and to bias toward merging, which is the fix OpenAI found for reviewer-author thrash. You read the review summary, not the diff, unless something is flagged.

## 9. Your weekly hour

Read the Sunday digest. Answer three to eight questions in `DECISIONS.md` (or by replying to the email; the planner ingests replies). Merge PRs from GitHub mobile. Spend fifteen minutes with the latest game build or the data dashboard and say what is wrong. Everything labeled `needs-human` is in the digest, so nothing else needs your attention.

## 10. Budget and throttles

I cannot see your plan's limits, so the first week measures actual spend and sets caps. Controls: `--max-turns` and `/goal` turn caps per session, one builder session per track per night, a session time cap, model routing (a small model for grooming and triage, a mid model for routine issues, the top model for design and review), and a weekly spend line in the digest. The kill switch is disabling a routine. Published reference points: the Ralph loop at roughly $10 per agent-hour, Gas Town at roughly $100 per hour for 20 to 30 agents, Managed Agents at about $0.08 per session-hour plus tokens.

## 11. Expected failure modes and their countermeasures

| Failure | Countermeasure |
|---|---|
| Agent declares victory early | Browser or simulator oracle; evidence check in the Stop hook |
| Duplicate implementations | `features.json` as the single feature registry; planner dedupes; a code-search step in the add-feature skill |
| Tests gamed to pass | Protected test paths; mutation floors; verification scripts hashed in CI |
| Reviewer and builder thrash | Severity threshold; bias to merge |
| Secrets in logs, missing authorization checks | Gitleaks and Semgrep; invariant tests (one tenant never reads another's data) |
| PRs too large to review | Size labels; diff-size gate |
| Source portals change (A) | Freshness SLAs; anomaly detection; auto-filed issues |
| Balance drifts (B) | Nightly simulation regression against bands |
| Agent installs a bad dependency | OSV-Scanner; dependency bumps only via the maintenance routine |

## 12. Setup sequence

After your decisions in `DECISIONS.md`: I create each repo's skeleton (files in Section 3, hooks, CI, labels, issue template, skills), write the first twenty sized issues per track from the roadmap, run the gate on an empty project so it is green, and create the routines. You create the two repositories and grant this environment access, or approve the monorepo alternative, and open the accounts listed in each track's design.
