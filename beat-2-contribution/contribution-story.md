# Contribution: Flaky e2e: stepPaste.cy.ts, copy/paste node action lost under load (KaotoIO/kaoto #3529)

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Jeff San Pedro
**GitHub Username:** jeff-sp
**Issue:** https://github.com/KaotoIO/kaoto/issues/3529
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: [one line on the tool work]**

 I built a scout using four GitHub issue searches and five ranking filters (TypeScript good-first-issues, Playwright, e2e tests, and GitHub Actions in TypeScript repos), then ran a scan on five issues. The results favored one org and surfaced mostly brand-new issues from new repos with little adoption. I changed the cap to per-owner, added "Repo has adoption" to the rubric, and updated the AI-policy check to flag repos with hooks that reject AI co-author trailers.

**Unit 6: installed the voice guard as a PreToolUse hook on one canonical voice guide with a new Upstream section, and wrote a 53-line repo memory file for my Kaoto clone**

I folded my three voice guide copies into one file at `~/.claude/voice-guard/voice-guide.md` (the pr-precheck copy was a superset of the other two), added four Upstream rules for claims, questions and pings, and wired `check_voice.py` as a user-global hook; the wiring test was stopped as expected and the stop is in `guard-log.md`. Two things broke on Windows: Git Bash `ln -s` copies instead of linking, so the three skill folders now hold a one-line pointer to the canonical guide, and Python's subprocess could not launch the CLI that npm installs as `claude.cmd`, so the guard's manual check on my claim draft failed with "the claude CLI is not on PATH" until I patched the script to resolve the path with `shutil.which` (the unit 1 eval harness had the same bug and got the same fix). My never-post list also banned any date, which collided with the assignment's "say when you'll report back"; I narrowed the rule to fix dates and allowed a report-back window for my own investigation, and the guard then passed the draft. It also earned its keep once for real: when I rewrote the claim to lead with my run counts, the guard stopped it for naming runs without the tool versions (my own never-post rule), and passed the corrected draft. On the real post it fired as a hook: the first `gh issue comment` was stopped on "Show it, then say it", the retry a minute later passed, and both lines are in `guard-log.md` along with the manual checks. The memory file came from one scoped read of the three providers, the Cypress commands and the spec, and that read found something the issue did not say: the "confirmed smell" in the providers was already fixed by PR #3522, merged 40 minutes before the issue was filed.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

The issue came from the Issue Selection site. I read all 31 pages (3035 issues), kept the 918 in TypeScript or JavaScript repos, narrowed to the 91 that matched my testing and CI keywords, and graded 7 with my scout: 4 accepted, 3 rejected, all three on the Unclaimed check because the site hides assignees, open PRs and claim comments. KaotoIO/kaoto #3529 was one of my two picks. On 2026-10-18 the scout accepted it and ranked it first on fit. On 2026-10-24, with the unit 6 scope patch in place, it accepted it again with a difficulty note of ambitious, for two reasons: the only comment on the thread (2026-08-05) says the original failure signature stopped appearing after PR #3522, and the failure that remains shows up under CI load. The patched read-out now ranks my other pick, openedx #2723, first because it is easier to start. I chose Kaoto anyway. The open edge is a real question I can ask a maintainer, and I asked it in my claim; a 34-file pattern fix would teach me less.

It fits me because I do TypeScript test automation for a living and a flaky Cypress spec in a React app is the kind of bug I chase at work, except that here the fix has to land in the app and not in the test. I hope to learn how React provider state and unmount-remount races turn into end-to-end flakes, and how to work a thread where the issue's own hypothesis may be stale.

An honest read of the scout: it held up on facts. Every pass cites a date, a count or a quote I could check, and the Unclaimed check did the real filtering on the site. It fell short in two places. Before the patch, the scope check was strict enough to reject #3528 on a "may already be fixed" comment and would have made #3529 a borderline case; the difficulty note fixed that. And the scout cannot read code: the issue's "confirmed smell" in the three providers was already fixed 40 minutes before the issue was filed, and only reading the clone showed that. The re-grade also flagged that "about 34 test files" sits close to the new row's "improve X everywhere" fail line and suggested a file-count rule; I have not added one.

Claim comment: https://github.com/KaotoIO/kaoto/issues/3529#issuecomment-6404572840

**Toolkit**

- Tool: issue-scout in direct mode on my two site picks, run twice: 2026-10-18 with the unit 5 rubric, and 2026-10-24 after replacing my `Scope fits newcomer` check with the staff `scope-feasible` row and adding the difficulty note section.
- What it produced: accept on both issues both times. The second run added difficulty notes (Kaoto #3529 ambitious, naming the changed failure signature; openedx #2723 moderate) and flipped the ranking to put openedx first.
- Difficulties: my old scope check turned open edges into rejections, which is why the "no longer reproduces" clause I added on 2026-10-18 rejected #3528 outright. The difficulty note lives in prose outside the JSON, so nothing enforces it. On Windows the unit 1 regression harness crashed before grading because Python could not find the CLI installed as `claude.cmd`.
- What changed: the scope row is now `scope-feasible` and the old fail conditions (TBD inputs, unendorsed feature requests, closed PRs behind the issue, "may already be fixed") feed the difficulty note instead. I patched my copy of the harness to resolve the CLI path with `shutil.which`. Regression against the unit 1 eval set: 18/20 (bar 18/20, pass), down from 20/20. Both misses are scope bundles the patch now accepts on purpose, a years-long design debate and an unendorsed feature wish with an undecided asset; under the patched rubric each would carry an ambitious note naming that edge, and I recorded both as intended disagreements rather than tightening the floor back to where it rejected real work.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

Windows 11 Home build 26300, Intel i9-14900F (32 logical cores), 32 GB RAM. Node v24.12.0, Yarn 4.18.1 via corepack, Cypress 16.0.0 with the Electron browser it bundles, Git Bash from git 2.52.0.windows.1. Kaoto commit `0a0a0df3` (`main`, 2026-10-23), a clone of KaotoIO/kaoto. Linux side: WSL2 Ubuntu 26.04 LTS (kernel 6.6.87, 32 logical cores and 15 GB visible to the VM), Node v22.22.1, Yarn 4.18.1 via corepack, Cypress 16.0.0 with Electron and Xvfb, a second clone at the same commit built the same way. The three providers at this commit already carry the PR #3522 change (`loadingStatus` derived from `loadState.request === request`), so the "set Loading inside .then" smell the issue body describes is not in this code.

Setup challenges: Yarn 4 is not on PATH on either side, so every command runs through `corepack yarn`. The repo's `.yarnrc.yml` sets `enableScripts: false`, so `yarn install` skips Cypress's binary postinstall; on Windows the 16.0.0 binary was already cached, on Linux `yarn cypress install` fetched it. Java is not needed for the UI build or the e2e run. On WSL, Cypress needs system libraries the fresh Ubuntu 26.04 image lacks (`libnss3`, `libasound2t64`, `libxss1`, `libnotify`, `xvfb`); the first `apt-get install` failed on a stale package index with 404s and had to be re-run after `apt-get update`. No other setup problems came up.

**Steps to Reproduce**

1. `corepack yarn install` (yarn's `enableScripts: false` skips Cypress's postinstall; the 16.0.0 binary was already in `~/AppData/Local/Cypress/Cache`, and `yarn cypress verify` passed).
2. `yarn workspace @kaoto/kaoto run build:dev && yarn workspace @kaoto/kaoto run build:lib`, then `yarn workspace @kaoto/kaoto run preview --host` (serves http://localhost:4173).
3. Idle runs, five times, from `packages/ui-tests`:
   `yarn cypress run --browser electron --spec cypress/e2e/designer/basicNodeActions/stepPaste.cy.ts --config baseUrl=http://localhost:4173 --env grepTags=-weekly`
4. Load runs, five times: before each run, start one busy loop per logical core (`for c in $(seq 1 $(nproc)); do timeout 400 bash -c 'while :; do :; done' & done`, 32 loops), wait 5 s, run the same command, then kill the loops.
5. One Windows run with the loops up and the Cypress process tree pinned to 2 logical cores (PowerShell `Start-Process`, then `ProcessorAffinity = 3`).
6. Linux, same steps 1 to 3 inside WSL Ubuntu (clone, `corepack yarn install`, `yarn cypress install` for the binary, `build:dev`, `build:lib`, `preview --host`), five idle runs, then five runs with 32 busy loops up and the Cypress command under `taskset -c 0,1`.
7. Observed result: 147 of 147 tests passed across all 21 runs on both platforms. Tests ran 2x slower under load on Windows, 4x pinned on Windows and 5x pinned on Linux, so the load reached the renderer, but neither failure signature from the thread appeared.

**Reproduction Evidence**

- **Commit showing reproduction:** none (no change to the repo was needed; the runs are against upstream `main` at `0a0a0df3`)
- **Screenshots/logs:** full Cypress output for all 21 runs is saved locally in my unit 6 materials folder (`support/repro-idle-1..5.txt`, `repro-load-1..5.txt`, `repro-load2core-1.txt`, `repro-wsl-idle-1..5.txt`, `repro-wsl-load2core-1..5.txt`), with the failed-step logs I read from the repo's nightly runs under `support/ci/`. Excerpts:

**Results (21 runs, 147 tests, 0 failures):**

| Mode | Runs | Tests passing | Spec duration reported by Cypress | Wall time per run |
|---|---|---|---|---|
| Windows, idle | 5 | 35 of 35 | 13 s | 23 to 35 s |
| Windows, 32 busy loops, Cypress unpinned | 5 | 35 of 35 | 24 to 29 s (run 4: 13 s, see note) | 225 to 411 s |
| Windows, 32 busy loops, Cypress tree pinned to 2 cores (`ProcessorAffinity = 3`) | 1 | 7 of 7 | 50 s | 735 s |
| Linux (WSL Ubuntu), idle | 5 | 35 of 35 | 13 s | 20 to 21 s |
| Linux (WSL Ubuntu), 32 busy loops, Cypress under `taskset -c 0,1` | 5 | 35 of 35 | 62 to 67 s | 87 to 93 s |

Note on load run 4: the busy loops had a 400 s `timeout` and Cypress took longer than that to start under load, so by the time the spec ran the loops had exited; its per-test timings match the idle runs. The other four Windows load runs show 2x slower tests, the Windows pinned run 4x, and the five Linux pinned runs 5x, so the load did reach the Electron renderer every time.

Idle run 1, Cypress result block:

```
  │ Tests:        7                                                                                │
  │ Passing:      7                                                                                │
  │ Failing:      0                                                                                │
  │ Duration:     13 seconds                                                                       │
  │ Spec Ran:     stepPaste.cy.ts                                                                  │
```

Per-test timings, idle run 1 versus load run 1 versus the 2-core pinned run:

```
idle 1:
    √ Design - Paste steps across CamelRoute (3257ms)
    √ Design - Paste steps in CamelRoute (2408ms)
    √ Design - Paste steps in Pipe/KB (1568ms)
    √ Design - Copy/paste steps across Kamelets-routes (2299ms)
    √ Design - Paste intercept in RouteConfiguration (1466ms)
    √ Design - Paste steps in CamelRoute by adding something to the clipboard (1122ms)
    √ Design - Paste step from external YAML source (no Kaoto marker) (1023ms)

load 1 (32 busy loops):
    √ Design - Paste steps across CamelRoute (6011ms)
    √ Design - Paste steps in CamelRoute (3719ms)
    √ Design - Paste steps in Pipe/KB (3009ms)
    √ Design - Copy/paste steps across Kamelets-routes (4988ms)
    √ Design - Paste intercept in RouteConfiguration (2847ms)
    √ Design - Paste steps in CamelRoute by adding something to the clipboard (2603ms)
    √ Design - Paste step from external YAML source (no Kaoto marker) (2724ms)

Windows load, pinned to 2 cores:
    √ Design - Paste steps across CamelRoute (10651ms)
    √ Design - Paste steps in CamelRoute (6704ms)
    √ Design - Paste steps in Pipe/KB (6325ms)
    √ Design - Copy/paste steps across Kamelets-routes (10192ms)
    √ Design - Paste intercept in RouteConfiguration (5774ms)
    √ Design - Paste steps in CamelRoute by adding something to the clipboard (5017ms)
    √ Design - Paste step from external YAML source (no Kaoto marker) (4292ms)

Linux idle 1:
    ✓ Design - Paste steps across CamelRoute (3189ms)
    ✓ Design - Paste steps in CamelRoute (2349ms)
    ✓ Design - Paste steps in Pipe/KB (1544ms)
    ✓ Design - Copy/paste steps across Kamelets-routes (2296ms)
    ✓ Design - Paste intercept in RouteConfiguration (1562ms)
    ✓ Design - Paste steps in CamelRoute by adding something to the clipboard (1100ms)
    ✓ Design - Paste step from external YAML source (no Kaoto marker) (1083ms)

Linux load, taskset -c 0,1, run 1:
    ✓ Design - Paste steps across CamelRoute (11755ms)
    ✓ Design - Paste steps in CamelRoute (9315ms)
    ✓ Design - Paste steps in Pipe/KB (6819ms)
    ✓ Design - Copy/paste steps across Kamelets-routes (10390ms)
    ✓ Design - Paste intercept in RouteConfiguration (8587ms)
    ✓ Design - Paste steps in CamelRoute by adding something to the clipboard (6514ms)
    ✓ Design - Paste step from external YAML source (no Kaoto marker) (6531ms)
```

Neither failure signature from the thread appeared in any run: not `Expected to find element: [data-testid="context-menu-item-paste-as-*"]` and not `cy.click() failed because the page updated while this command was executing`. Full Cypress output for every run is on my machine (`support/repro-idle-1..5.txt`, `repro-load-1..5.txt`, `repro-load2core-1.txt`, `repro-wsl-idle-1..5.txt`, `repro-wsl-load2core-1..5.txt`, local files, not attached here); Cypress also recorded videos, not part of this report.

**Expected (per the issue):** idle passes, saturated CPU fails intermittently with the context-menu item never found, or (per the 2026-08-05 comment) `cy.click()` failing because the page updated mid-command.

**Actual:** 147 of 147 tests passed across idle, saturated, and saturated-plus-pinned runs, on Windows 11 and on Linux (WSL Ubuntu), both with Electron.
- **My findings:** I cannot reproduce #3529 on this machine at commit `0a0a0df3`: 147 of 147 tests passed, idle, saturated, and saturated with Cypress pinned to 2 cores, on Windows and on Linux. Two observations go with that, one from the code and one that is only a guess.

From the code: the "set Loading inside the fetch's .then" window the issue body describes is not in the three providers at this commit. Each one derives `loadingStatus` from whether the current request matches the one being loaded, which is the PR #3522 change (merged 2026-07-21 08:27 UTC, 42 minutes before this issue was opened). `packages/ui/src/providers/runtime.provider.tsx` at `0a0a0df3`:

```
 52    const [loadState, setLoadState] = useState({ request, status: LoadingStatus.Loading });
 53    // A new request hides interactive children immediately, before its effect starts fetching.
 54    const loadingStatus = loadState.request === request ? loadState.status : LoadingStatus.Loading;
```

`packages/ui/src/providers/schemas.provider.tsx`:

```
 24    const [loadState, setLoadState] = useState({ request, status: LoadingStatus.Loading });
 25    // A new request hides interactive children immediately, before its effect starts fetching.
 26    const loadingStatus = loadState.request === request ? loadState.status : LoadingStatus.Loading;
```

`packages/ui/src/dynamic-catalog/catalog.provider.tsx`:

```
 33    const [loadState, setLoadState] = useState({ request, status: LoadingStatus.Loading });
 34    // A new request hides interactive children immediately, before its effect starts fetching.
 35    const loadingStatus = loadState.request === request ? loadState.status : LoadingStatus.Loading;
```

I have not traced whether anything else can still present the canvas as ready mid-reload; this only shows that the specific window the issue names is closed.

The guess: the CI shards run on `ubuntu-latest` inside a `cypress/browsers` container (`.github/workflows/e2e-tests.yml`) with several specs per shard, and this is a 32-core desktop. The Linux runs pinned to 2 cores under full load were my attempt to narrow that gap, and they passed too, so whatever separates my runs from the CI failures is not Windows versus Linux and is probably not CPU starvation of this one spec on its own. I have not tested the real matrix (16 specs per shard, three browsers, four shards in parallel).

**What I did not try:** Chrome or Chromium (Electron only), the real CI matrix with several specs running at once, and the `weekly`-tagged tests (`grepTags=-weekly` excludes them, as the issue's command does). The next thing I would try is the repo's own e2e workflow on my fork, which runs the real shards on `ubuntu-latest`.

**CI reading (Actions tab, 2026-10-24):** since 2026-09-09 the Cypress e2e job runs inside the "Build npm Package" workflow and in the nightly full matrix. The last 14 nightly runs (2026-10-12 to 2026-10-24) were 9 successes and 5 failures, and none of the failures is this spec: 2026-10-13 Firefox shard 2 (`stepPaste.cy.ts` is skipped on Firefox and shows `0 passing` in that log), 2026-10-20 Firefox shard 3 plus a VS Code macOS job, 2026-10-21 all twelve e2e shards with "Socket closed before the connection was established" after 2 minutes (an infrastructure outage, no spec ran), 2026-10-22 and 2026-10-24 VS Code jobs only. No Chrome or Edge shard failed on a spec in that window. The list I read (`gh run list --workflow nightly.yml --limit 14`, date, conclusion, run id):

```
2026-10-24 | failure | 40071978011   (VS Code macOS job)
2026-10-23 | success | 39914956300
2026-10-22 | failure | 39738397710   (VS Code ubuntu job)
2026-10-21 | success | 39566763152
2026-10-21 | failure | 39559658427   (all 12 e2e shards: "Socket closed before the connection was established")
2026-10-20 | failure | 39384980239   (Firefox shard 3, VS Code macOS job)
2026-10-19 | success | 39215406840
2026-10-18 | success | 39105949122
2026-10-17 | success | 39010501360
2026-10-16 | success | 38863022689
2026-10-15 | success | 38695805753
2026-10-14 | success | 38526341814
2026-10-13 | failure | 38373012947   (Firefox shard 2)
2026-10-12 | success | 38224883676
```
 The e2e workflow's last failures as a standalone PR check are from 2026-09-09 and their logs are no longer downloadable, so I cannot say from CI when `stepPaste.cy.ts` last failed; that is a question for the thread. The failed-step logs I did read are local files too (`support/ci/`).

**Toolkit**

- Tool: repro-check in live mode under the new wild scope, run five times through the CLI the way the assignment says (`claude "repro-check: grade ..."`): once on the claim alone, then four times on the claim plus this report as the report and the claim changed; transcripts saved beside the drafts (`support/claim-live-run.txt`, `support/package-live-run*.txt`). The runs themselves came from two small bash runners (one for Windows, one for WSL) that start the busy loops, run the spec, kill the loops, and write one output file per run.
- What it produced: accept on the claim-only draft (three claim checks pass, six report checks marked not yet applicable). The first full package was rejected on Outcome stated as observed: my findings asserted the providers were fixed without quoting the code, and asserted a CI runner profile nothing supported. I quoted the three provider lines and marked the hardware point as an untested guess; the second run accepted. After the Linux phase was added the third run accepted again, with voice notes to show the CI list I read instead of asserting it, which I did. The fourth full run graded the claim after I rewrote it in my own words (the drafted version read like a model wrote it: one inventory sentence, a staged "one question before I go further", flourishes like "whichever way it goes"); it accepted, and caught one thing I had glossed: the Windows load run whose loops expired had been counted as saturated in the claim, so the claim now says so. One more thing the guard taught me: it is a Sonnet judgment, not a regex, and it stopped my rewritten claim on "Show it, then say it" a minute after passing the same sentences, so I put one line of Cypress output ahead of the counts rather than argue with it.
- Difficulties: the issue's load recipe never broke the spec here; on Windows the busy loops slowed the tests 2x to 4x, on Linux pinned to two cores 5x, and all 147 tests still passed. One Windows load run was effectively idle because the loops' 400 s timeout expired during Cypress's slow startup; I kept it and said so. The repo's failed e2e logs from September are no longer downloadable, so CI could not tell me when this spec last failed. The rubric's row 5 accepts an evidenced cannot-reproduce, which is why the honest result could go in as is.
- What changed: the sandbox scope was replaced with the wild scope (a stranger's claim blocks, never piggyback, repo rules outrank mine). Nothing in the rubric. The recipe grew a 2-core pinned run and then a Linux phase because the first five load runs did not look like a small CI runner, and the claim now leads with the counts and asks the maintainer which failure to chase, instead of promising to run the spec.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
