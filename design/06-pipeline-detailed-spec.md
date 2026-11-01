# Pipeline detailed specification

Status: design, 2026-11-01. This is the exact content the setup week installs in both track repos. Design 00 is the overview; this document is the spec. Where a value differs by track it is marked A or B.

## 1. Repository files

### 1.1 `CLAUDE.md` (template, under 60 lines)

```
# <Repo name>

Purpose: <one sentence>. Studio design: IdeasMoney/design (00 pipeline, 0X this track).

## Commands
- Install: <A: uv sync | B: pnpm install>
- Fast gate: scripts/gate-fast.sh     (format, lint, typecheck, fast tests; under 60s)
- Full gate: scripts/gate-stop.sh     (fast gate plus suite, lint rules, evidence check)
- App: <A: uv run uvicorn records.api.main:app | B: pnpm --filter web dev>
- Sim/E2E: <A: uv run records jobs --dry-run | B: pnpm sim run --hours 100 ; pnpm e2e>

## Layout
<five to ten lines naming top-level folders and what lives in each>

## Conventions
- Issue-driven: one issue, one branch claude/<n>-<slug>, one PR. Tests first.
- Types strict. No `any` (B) / pyright strict (A). Public functions documented in one line.
- Content (B) and source configs (A) are data, validated by schemas; never hand-edit generated drafts.
- Commit messages: `<type>(<scope>): <summary>` with the issue number.

## Definition of done
- Acceptance criteria in the issue are each covered by a test that failed before the change.
- gate-stop passes locally; CI green; evidence in the PR body.
- No new warnings; no snapshot or golden updates (human-only).

Hard rules live in hooks and CONSTITUTION.md, not here.
```

### 1.2 `CONSTITUTION.md` (full text, both tracks)

1. Never edit a test to make it pass. A wrong test is fixed only with an explicit note in the PR explaining why the spec required it.
2. Never update snapshots, golden files or recorded fixtures. Those commits are human-only.
3. Never delete, skip, quarantine or weaken a failing test.
4. Never commit secrets, credentials, personal data outside the approved schema, or another company's code, data or assets.
5. Never claim compliance, affiliation, endorsement or guarantees in any copy.
6. Disclose AI-generated player-facing content (B). Record the legal register entry before enabling a source (A).
7. Respect robots.txt and per-source rate limits in code, not by convention (A).
8. Stop and label `needs-human` when a task requires a judgment the spec does not cover. Do not guess at product, legal or money decisions.
9. Prefer deleting code to adding it. No speculative abstractions.
10. One issue per branch, one PR per issue, no direct pushes to `main`.

### 1.3 `features.json` schema

```json
{
  "id": "B-M0-012",
  "milestone": "M0",
  "title": "Seeded PRNG with per-system streams",
  "size": "S",
  "acceptance": ["Same seed yields identical sequences across runs", "Streams for loot, combat and gathering are independent", "Property test covers 1,000 seeds"],
  "passes": false,
  "issue": null,
  "depends_on": ["B-M0-003"]
}
```

The planner owns this file. `passes` flips to true only when the linked PR merges. `issue` holds the GitHub issue number once created.

### 1.4 Issue template (`.github/ISSUE_TEMPLATE/task.yml`)

Fields: Goal (one paragraph), Acceptance criteria (checklist, each testable), Out of scope, Test plan (which suites, which oracles), Size (S or M), Feature id (from features.json), Risks.

### 1.5 PR template (`.github/PULL_REQUEST_TEMPLATE.md`)

Sections: Issue, Acceptance criteria with checkboxes, Evidence (commands run and their output, screenshots or simulator output), Tests added, Risks and rollback, What was deliberately not done.

### 1.6 Labels

| Label | Meaning |
|---|---|
| `ready` | Specified, sized, buildable tonight |
| `in-progress` | A builder owns it |
| `pr-open` | PR exists |
| `needs-human` | A question for you is in the issue |
| `blocked` | Waiting on another issue or an account |
| `done` | Merged |
| `S`, `M`, `L` | Size; `L` must be split by the planner |
| `feature`, `bug`, `chore`, `content` (B), `data-source` (A), `migration`, `balance-change` (B), `large-change-approved` | Type and exceptions |

### 1.7 `CODEOWNERS`

`* @sebob2491` so every PR requests your review.

## 2. Hooks

### 2.1 `.claude/settings.json`

```json
{
  "hooks": {
    "PreToolUse": [
      { "matcher": "Edit|Write|MultiEdit", "hooks": [{ "type": "command", "command": "scripts/guard-paths.sh" }] }
    ],
    "PostToolUse": [
      { "matcher": "Edit|Write|MultiEdit", "hooks": [{ "type": "command", "command": "scripts/gate-fast.sh --quiet" }] }
    ],
    "Stop": [
      { "hooks": [{ "type": "command", "command": "scripts/gate-stop.sh" }] }
    ]
  }
}
```

### 2.2 `scripts/guard-paths.sh`

Reads the hook input JSON from stdin and extracts the target path. Exits 2 with a one-line reason on stderr when:

- the path matches a protected glob: `index.html` at the root (B), `**/__snapshots__/**`, `**/golden/**`, `**/fixtures/recorded/**`, `migrations/**` (unless `ALLOW_MIGRATIONS=1` is set by a builder working an issue labeled `migration`), `.env*`, `SPEC.md`, `CONSTITUTION.md`, `legal/register.yaml` (A);
- the file `.claude/phase` contains `green` and the path is a test file (`tests/**`, `**/*.test.ts`, `**/*.spec.ts`, `**/test_*.py`). In the `red` phase test edits are allowed and implementation edits under `src/` or `packages/*/src` are blocked, which enforces tests-first.

The builder prompt writes `.claude/phase`. The file is gitignored.

### 2.3 `scripts/gate-fast.sh`

A: `uv run ruff format --check . && uv run ruff check . && uv run pyright && uv run pytest -q -m fast -x`.
B: `pnpm format:check && pnpm lint && pnpm typecheck && pnpm test:fast`.
Target under 60 seconds. With `--quiet` prints only failures, so the agent's context stays small.

### 2.4 `scripts/gate-stop.sh`

Runs gate-fast, then the full unit and property suite, then the content linter and forbidden-terms lint (B) or schema and legal-register checks (A). Then two checks: no test file deleted relative to `origin/main` (`git diff --diff-filter=D --name-only origin/main...HEAD` matching test paths fails the gate), and when `.claude/phase` is `green`, `.claude/evidence.md` exists and contains a "Commands run" section. Exit 2 keeps the agent working with the failures on stderr. Claude Code stops honoring the block after a fixed number of consecutive blocks, at which point the builder prompt tells the agent to label the issue `blocked` and write what failed.

## 3. CI and branch protection

### 3.1 `ci.yml` jobs (both tracks, ordered, with time budgets)

| Job | A | B | Budget |
|---|---|---|---|
| lint-type | ruff, pyright strict | eslint, prettier check, tsc | 5 min |
| unit-property | pytest with coverage, hypothesis | vitest, fast-check | 10 min |
| content-rules | legal-register schema check, source config validation | content lint, forbidden-terms lint | 3 min |
| mutation-changed | mutmut on files changed vs main; reported, floor 50% (70% in normalize/ and billing/) | stryker incremental on changed files; floor 50% (70% in core) | 15 min |
| e2e | schemathesis against the running API; playwright plus axe on docs and pages | playwright smoke, parity suite, axe | 15 min |
| security | semgrep p/default, osv-scanner, gitleaks | same | 10 min |
| budgets | API p95 on a fixed load script (later) | bundle size, tick time, memory | 5 min |
| diff-size | fails over 600 changed lines unless `large-change-approved` | same | 1 min |

`nightly.yml`: full mutation run, dependency audit, docs drift check, long simulation (B) or full re-crawl of fixtures (A). `release.yml`: on tag, build, deploy, smoke test.

### 3.2 Branch protection on `main`

Require a pull request; require status checks lint-type, unit-property, content-rules, e2e, security, diff-size; require one approval from CODEOWNERS; dismiss stale approvals on new commits; no force pushes; no deletions; squash merge only; linear history.

## 4. Routines

All run in `auto` permission mode as fresh cloud sessions on a fresh clone of the repo, with a turn cap. Times are America/Phoenix.

### 4.1 Builder (A at 01:00, B at 02:30, Monday to Friday nights; cap 40 turns)

```
You are the nightly builder for <repo>. Work only in this repo.
1. Read CLAUDE.md, CONSTITUTION.md, SPEC.md and features.json. Run `git fetch origin main`; everything starts from origin/main.
2. Pick the highest-priority open issue labeled `ready` sized S or M (priority label first, then lowest number). If none, run scripts/gate-stop.sh on main, report its result as a comment on the latest planner digest issue, and stop.
3. Comment on the issue "Builder starting" and relabel `ready` to `in-progress`.
4. Create branch claude/<issue-number>-<slug>.
5. Write "red" to .claude/phase. Write failing tests for every acceptance criterion. Run them and confirm each fails for the right reason. Commit `test(<scope>): failing tests for #<n>`.
6. Write "green" to .claude/phase. Implement until tests pass without editing test files. Commit in small steps.
7. Run scripts/gate-stop.sh. Run the relevant app, simulator or job and record commands and output in .claude/evidence.md; save screenshots or simulator output under evidence/<n>/.
8. Open one PR to main titled "#<n>: <title>" using the PR template, with the acceptance checklist, the evidence, risks and what you did not do. Relabel to `pr-open`.
9. Subscribe to the PR. Fix CI failures and address review comments until CI is green or the turn cap is near. On a judgment call the spec does not cover, label `needs-human`, write the precise question on the issue, and stop.
Rules: never edit tests after the red phase except to fix a test that is wrong per spec, stated in the PR; never touch protected paths; never push to main; never open a second PR in one run; never install a dependency not already in the lockfile unless the issue says so.
```

### 4.2 Reviewer (trigger: pull request opened or synchronized by a builder; cap 15 turns)

```
You are an independent reviewer for <repo>. You have not seen the conversation that produced this PR and must not assume its reasoning.
1. Read the PR, the linked issue, CONSTITUTION.md and SPEC.md.
2. Run /code-review and /security-review on the diff.
3. Check: every acceptance criterion has a test that would fail without the change; no protected path touched; no snapshot updates; evidence present and plausible; no new dependency without justification; (A) legal register entry for any new source; (B) forbidden-terms lint and content lint pass.
4. Post one review. Flag only P1 (must fix before merge) and P2 (should fix). Do not post style nits. If a P1 exists, request changes and label the issue `needs-human` only if the fix needs a product or legal judgment. Otherwise the builder addresses it.
Bias toward merging when criteria are met.
```

### 4.3 Planner (Sundays 18:00; cap 30 turns; runs on IdeasMoney with both track repos readable)

```
You are the weekly planner for the studio.
1. For each track repo: list PRs merged this week, open PRs, issues labeled needs-human and blocked, CI pass rate, mutation scores from the nightly job, watchdog issues. Read digests/ for last week.
2. Update features.json in each repo: set passes true for merged features; add issues for the next milestone's unbuilt features; split anything sized L; keep 20 to 30 issues labeled ready per repo.
3. Write digests/YYYY-WW.md in IdeasMoney with sections: Shipped, Metrics vs targets (design 05 Section 6), Usage this week, Needs you (every needs-human item as a yes/no or multiple-choice question with your recommended default), Risks changed, Next week.
4. Append the questions to DECISIONS.md as Q-items with defaults and a needed-by date. Read any answers written since last week and record them as D-items; update issues accordingly.
5. Email the digest to the owner (Gmail connector) with subject "Studio digest YYYY-WW".
Rules: never change SPEC.md or CONSTITUTION.md; propose changes as questions. Never approve or merge PRs.
```

### 4.4 Maintenance (Tuesday and Thursday nights 04:00; cap 25 turns)

Surviving mutants from the nightly report become issues (one per module, sized S). Dependency bumps: one PR per ecosystem with OSV scan output in the body. Docs drift: compare CLAUDE.md commands and README against the tree; PR fixes. Flaky tests: report in an issue, never skip. (A) refresh recorded fixtures only on request. (B) run the long simulation and attach the report to the balance issue.

### 4.5 Watchdog A (every 6 hours; cap 10 turns)

Check freshness per source against SLA class; crawl error rates; anomaly flags; API 5xx rate; billing webhook failures; uptime. File or update one issue per problem with the evidence. Pause publication for a failing source by the documented command. Escalate `needs-human` only for legal or billing problems.

### 4.6 Watchdog B (daily 05:00; cap 10 turns)

Run the simulation suite against bands; run content lint on main; collect crash reports and itch comments; file issues; attach the balance report to the weekly balance issue.

### 4.7 Release (on tag; cap 20 turns)

Generate changelog from merged PRs since the last tag; build; deploy to the target for the tag's channel; run the smoke suite; post the release notes; for Steam, produce the depot build and the disclosure text check.

## 5. Protocols

- **DECISIONS.md:** D-items are decisions with date and decider; Q-items are questions with a recommended default and a needed-by date. You answer by editing the row or replying to the digest email; the planner records the answer as a D-item. An unanswered question past its date proceeds on the default if the row says so, otherwise the work stays `blocked`.
- **needs-human:** the issue gets one comment with a yes/no or multiple-choice question, the default, and what each answer changes. The digest collects these.
- **ADR:** `docs/adr/NNNN-title.md` with Context, Decision, Consequences, written by the builder when it makes a structural choice; the reviewer checks one exists for structural PRs.
- **Commit messages:** `<type>(<scope>): <summary> (#<issue>)` plus the attribution trailers the environment requires.
- **Versioning:** semantic versions; tags `vX.Y.Z` trigger releases; Track B web alpha uses `0.x`.

## 6. Model routing and budgets

| Work | Model tier | Cap |
|---|---|---|
| Planner, digests | Mid | 30 turns |
| Builder, S chores | Mid | 40 turns |
| Builder, M features and anything in core or normalize or billing | Top | 40 turns |
| Reviewer | Top | 15 turns |
| Maintenance, watchdogs | Small or mid | 10 to 25 turns |
| Design and incident sessions | Top, interactive | None |

The digest carries a usage line. If a week's routines exceed the share you are comfortable with, the planner proposes which routine to thin.

## 7. Secrets and access

| Secret | Where | Who can read |
|---|---|---|
| GitHub Actions deploy tokens | Repo Actions secrets | CI only |
| Fly or Render API token (A) | Fly or Render dashboard and Actions secret | CI deploy job only |
| Database URL (A) | Fly secrets; Actions secret for migrations | API runtime; CI migration job |
| Stripe keys (A) | Stripe dashboard; Actions secret for the API deploy | API runtime |
| Cloudflare tokens | Cloudflare; Actions secret | CI pages deploy |
| Steamworks credentials (B) | Your machine and a dedicated Actions secret for depot uploads | Release job only |
| Sentry DSN | Actions secret | Runtime |

Routines never hold production write secrets. They open PRs; CI deploys. Rotation on a 90-day schedule is a maintenance-routine issue. Gitleaks runs in the fast gate and CI.

## 8. Kill switches and rollback

Disable a routine (one toggle) to stop all autonomous work for a track. Revert a PR with a one-click revert PR. Deploys roll back by redeploying the previous tag. Track A can pause a single source's publication without touching code. Track B web releases are static files, so rollback is republishing the previous build; Steam builds are set live per branch in Steamworks.

## 9. Pipeline observability

The planner records weekly: PRs opened and merged, median time to green, revert count, mutation scores, CI minutes, needs-human count, usage share. These live in `metrics/weekly.csv` in IdeasMoney and feed the monthly review in design 05.

## 10. Owner onboarding checklist

GitHub: both track repos private, your handle in CODEOWNERS, branch protection on, this environment granted access. Stripe (A) before M3. Cloudflare account; Fly or Render; Neon or Supabase; Apify developer account; Sentry (A, from M1). itch.io and a domain (B, from M2); Steamworks (B, from M3). Each is a `blocked` issue until done, listed in the digest when its milestone approaches.
