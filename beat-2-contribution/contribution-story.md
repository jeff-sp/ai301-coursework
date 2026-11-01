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
**Pull Request:** https://github.com/KaotoIO/kaoto/pull/4306

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

**Unit 7: installed the ship-gate on my Kaoto clone, wrote a manifest of four checks as `corepack yarn` commands, and took one real branch through four stops to two passes and an open PR**

The manifest names the four checks AGENTS.md and `build-lint-test.yml` ask for, each written as `corepack yarn ...` because `yarn` is not on PATH in a fresh terminal. The gate ran them on `fix/issue-3529-node-context-menu`. The unit-test check stopped it twice with 10 s timeouts in catalog-loading tests my change does not touch, which pass alone and passed 2 of 4 full default runs. Instead of moving the suite out, the unit-test check now carries `--testTimeout=30000 --hookTimeout=60000` and says why, and it has passed three times since. The third run passed all four checks and then could not spawn pr-precheck, the `claude.cmd` lookup bug Python has on Windows, so `check_gate.py` now resolves the CLI with `shutil.which` like my voice guard does. pr-precheck passed all 15 checks on the first call, but its voice report named five lines in `pr_draft.md` (a cause stated as fact, a deferral phrased as a promise, a test block with no pass count, a missing base commit, a duplicate `Fixes` line). Fixing them cost one more full run and bought a clean pass from the voice guard when `gh pr create` opened [KaotoIO/kaoto#4306](https://github.com/KaotoIO/kaoto/pull/4306).

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

The issue came from the Issue Selection site. I read all 31 pages (3035 issues), kept the 918 in TypeScript or JavaScript repos, narrowed to the 91 that matched my testing and CI keywords, and graded 7 with my scout: 4 accepted, 3 rejected, all three on the Unclaimed check because the site hides assignees, open PRs and claim comments. KaotoIO/kaoto #3529 was one of my two picks. On 2026-10-18 the scout accepted it and ranked it first on fit. On 2026-10-24, with the unit 6 scope patch in place, it accepted it again with a difficulty note of ambitious, for two reasons: the only comment on the thread (2026-08-05) says the original failure signature stopped appearing after PR #3522, and the failure that remains shows up under CI load. The patched read-out now ranks my other pick, openedx #2723, first because it is easier to start. I chose Kaoto anyway. The open edge is a real question I can ask a maintainer, and I asked it in my claim; a 34-file pattern fix would teach me less.

It fits me because I do TypeScript test automation for a living and a flaky Cypress spec in a React app is the kind of bug I chase at work, except that here the fix has to land in the app and not in the test. I hope to learn how React provider state and unmount-remount races turn into end-to-end flakes, and how to work a thread where the issue's own hypothesis may be stale.

An honest read of the scout: it held up on facts. Every pass cites a date, a count or a quote I could check, and the Unclaimed check did the real filtering on the site. It fell short in two places. Before the patch, the scope check was strict enough to reject #3528 on a "may already be fixed" comment and would have made #3529 a borderline case; the difficulty note fixed that. And the scout cannot read code: the issue's "confirmed smell" in the three providers was already fixed 40 minutes before the issue was filed, and only reading the clone showed that. The re-grade also flagged that "about 34 test files" sits close to the new check's "improve X everywhere" fail line and suggested a file-count rule; I have not added one.

Claim comment: https://github.com/KaotoIO/kaoto/issues/3529#issuecomment-6404572840

**Toolkit**

- Tool: issue-scout in direct mode on my two site picks, run twice: 2026-10-18 with the unit 5 rubric, and 2026-10-24 after replacing my `Scope fits newcomer` check with the staff `scope-feasible` check and adding the difficulty note section.
- What it produced: accept on both issues both times. The second run added difficulty notes (Kaoto #3529 ambitious, naming the changed failure signature; openedx #2723 moderate) and flipped the ranking to put openedx first.
- Difficulties: my old scope check turned open edges into rejections, which is why the "no longer reproduces" clause I added on 2026-10-18 rejected #3528 outright. The difficulty note lives in prose outside the JSON, so nothing enforces it. On Windows the unit 1 regression harness crashed before grading because Python could not find the CLI installed as `claude.cmd`.
- What changed: the scope check is now `scope-feasible` and the old fail conditions (TBD inputs, unendorsed feature requests, closed PRs behind the issue, "may already be fixed") feed the difficulty note instead. I patched my copy of the harness to resolve the CLI path with `shutil.which`. Regression against the unit 1 eval set: 18/20 (bar 18/20, pass), down from 20/20. Both misses are scope bundles the patch now accepts on purpose, a years-long design debate and an unendorsed feature wish with an undecided asset; under the patched rubric each would carry an ambitious note naming that edge, and I recorded both as intended disagreements rather than tightening the floor back to where it rejected real work.

---

## Understanding the Issue

**Problem Description**

The Cypress spec `packages/ui-tests/cypress/e2e/designer/basicNodeActions/stepPaste.cy.ts` is flaky on CI. Each of its seven tests right-clicks a canvas node, clicks a context-menu item (copy, then one of the three paste-as items), and checks the pasted node. Under CPU load on the CI shards, the click on the menu item sometimes fails. The issue was filed as a copy of #3523 (an "untrue ready signal" in the three loading providers), but that provider change (PR #3522) was already merged 42 minutes before the issue was opened, and my 21 local runs at `0a0a0df3` passed. What is left is the failure tplevko reported on 2026-08-05: the menu item is found, then detached while Cypress is clicking it.

**Expected Behavior**

After a right-click on a node, the context menu renders its items once and keeps them in the DOM until the user picks one or closes the menu. A click on `[data-testid="context-menu-item-paste-as-*"]` (or `-copy`) lands on the same element Cypress located, and the spec passes on every run, loaded or idle, without the fixed `cy.wait(200)` and second right-click that `performNodeAction` in `design.ts` carries today.

**Current Behavior**

On the CI shards (Chrome and Electron, `ubuntu-latest`, several specs per shard), the spec fails intermittently with:

```
CypressError: Timed out retrying after 8050ms: `cy.click()` failed because the page updated while this command was executing. Cypress tried to locate elements based on this query:

> <li.pf-v6-c-menu__list-item>
```

That is a menu `<li>` that existed when Cypress queried it and was gone by the time it clicked. The older signature, `Expected to find element: [data-testid="context-menu-item-paste-as-*"], but never found it`, stopped appearing after PR #3522. Locally, on Windows 11 and WSL Ubuntu, idle and under load, 147 of 147 tests passed, so the race is narrower than the issue's recipe suggests.

**Affected Components**

- The paste menu items: `packages/ui/src/components/Visualization/Custom/ContextMenu/ItemPasteStep.tsx` and the hook behind them, `packages/ui/src/components/Visualization/Custom/hooks/paste-step.hook.tsx` (`usePasteStep`), which start with `isCompatible = false`, read the clipboard asynchronously (`ClipboardService.paste()` in `packages/ui/src/services/visualization/clipboard.service.ts`), and only then render the item.
- The menu builder, `packages/ui/src/components/Visualization/Custom/ContextMenu/NodeContextMenu.tsx` (`addCopyPasteItems`), and the node that opens it, `packages/ui/src/components/Visualization/Custom/Node/CustomNode.tsx`, through PatternFly topology's `withContextMenu` and `ContextMenu` (`@patternfly/react-topology` 6.6.0), whose `ContextMenu` closes and reopens itself on the next animation frame whenever its children change.
- The test side: `performNodeAction` in `packages/ui-tests/cypress/support/next-commands/design.ts`, whose fixed 200 ms wait and second right-click were added in January (commits 4c9088db and 9d206e9b) as a workaround.
- Not involved any more: `runtime.provider.tsx`, `schemas.provider.tsx` and `catalog.provider.tsx`, which already derive `loadingStatus` from the current request (PR #3522).

**Toolkit**

- Tool: Claude Code with the repo memory file from unit 6 (`CLAUDE.local.md`), used for one scoped read of the context-menu components, the paste hook, PatternFly's `withContextMenu` and `ContextMenu` sources under `node_modules`, and the Cypress command, plus `gh issue view` for the thread and `gh pr view 3522` for what the earlier fix touched.
- What it produced: the four answers above, and the finding that the menu item is built in two steps (copy item at once, paste items after an async clipboard read) on top of a PatternFly menu that remounts itself when its children change.
- Difficulties: the issue body points at the three providers, and those were already fixed; the useful pointer was the maintainer's August comment, not the issue text. PatternFly's behaviour is only visible in `node_modules`, which my memory file did not cover.
- What changed: I added the PatternFly `withContextMenu` and `ContextMenu` paths to `CLAUDE.local.md` so the next read starts there, and I treat the issue body's hypothesis as history rather than as the plan.

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
- Difficulties: the issue's load recipe never broke the spec here; on Windows the busy loops slowed the tests 2x to 4x, on Linux pinned to two cores 5x, and all 147 tests still passed. One Windows load run was effectively idle because the loops' 400 s timeout expired during Cypress's slow startup; I kept it and said so. The repo's failed e2e logs from September are no longer downloadable, so CI could not tell me when this spec last failed. The rubric's fifth check accepts an evidenced cannot-reproduce, which is why the honest result could go in as is.
- What changed: the sandbox scope was replaced with the wild scope (a stranger's claim blocks, never piggyback, repo rules outrank mine). Nothing in the rubric. The recipe grew a 2-core pinned run and then a Linux phase because the first five load runs did not look like a small CI runner, and the claim now leads with the counts and asks the maintainer which failure to chase, instead of promising to run the spec.

---

## Solution Approach

**Analysis**

The issue body's cause is gone. `runtime.provider.tsx`, `schemas.provider.tsx` and `catalog.provider.tsx` already engage `Loading` synchronously (PR #3522, merged 42 minutes before #3529 was filed), and my 21 runs at `0a0a0df3` passed. The failure that remains, per tplevko's 2026-08-05 comment, is `cy.click()` failing "because the page updated while this command was executing" on a `li.pf-v6-c-menu__list-item`: Cypress had the menu item, and it left the DOM under the click. So I traced what can remove a context-menu item after it has rendered.

The node menu is PatternFly topology's. `CustomNode.tsx` and `CustomGroup.tsx` wrap the node in `withContextMenu(NodeContextMenuFn)` from `@patternfly/react-topology` 6.6.0. That wrapper (`behavior/withContextMenu.js`) keeps one `reference` state per node; a right-click sets it, an effect calls `NodeContextMenuFn(element)` and hands the items to `<ContextMenu>` through `Promise.resolve(...).then(setElementActions)`, and until then the menu shows a spinner. `ContextMenu` (`components/contextmenu/ContextMenu.js`) has this effect:

```js
useEffect(() => {
  if (isOpen) {
    setOpen(false);
    requestAnimationFrame(() => setOpen(true));
  }
}, [children]);
```

Every change of `children` closes the Popper (which unmounts every `<li>`) and reopens it on the next animation frame. On the first right-click that is harmless: the spinner-to-items swap happens before the frame, so the items are inserted once. My DOM trace of the spec (a `MutationObserver` on every `li.pf-v6-c-menu__list-item`, three idle runs on Windows) shows exactly that: the items appear 12 to 56 ms after the `contextmenu` event, the paste item 3 to 20 ms later, and nothing is removed until the click.

The problem is the second right-click while a menu is open. `performNodeAction` in `design.ts` does one when the item it wants is not in the DOM 200 ms after the first right-click, and a user does one whenever they right-click again. The `mousedown` closes the Popper (`setReference(null)`), the `contextmenu` sets a new reference, React batches both, so the same `ContextMenu` instance stays mounted with the previous menu's items as children while the new actions resolve. The previous items are still in the DOM and still wired to the previous node, and when the new array arrives the effect above tears the whole menu down and brings it back a frame later. Under load with every core busy, the trace shows the sequence on the first test ("Paste steps across CamelRoute", right-click on `Choice-route-2`): items at +54 ms without the paste item (the clipboard read had not finished), the helper's retry right-click at +277 ms, the old items removed 11 ms after it, and the new menu, now with `context-menu-item-paste-as-special-child`, 540 ms after it. That 11 ms is a window in which Cypress can find an `<li>` that is about to be detached. I expect the window to be wider on a loaded CI container, where React's effect flush comes later, and I expect this to be what produces the "page updated while this command was executing" error; I have not seen the failure itself, here or in a CI log. The paste items make the retry likely: `usePasteStep` (`paste-step.hook.tsx`) starts with `isCompatible = false`, awaits `navigator.permissions.query` and `ClipboardService.paste()`, and `ItemPasteStep.tsx` returns `null` until then, so under load the paste item is often the one missing at the 200 ms check.

Two things I did not trace to the end: how long the window gets on the `cypress/browsers` container (I measured 11 ms on a Windows machine under 32 busy loops), and whether the catalog tiles provider (`catalog-tiles.provider.tsx` fills a ref with no state change, so `checkCompatibility` returns `false` until the tiles are in) adds to the paste item's delay on CI.

What CI shows today: I scanned every failed Cypress job whose log GitHub still serves, on `KaotoIO/kaoto`: the 16 nightly runs since 2026-10-12 and the 243 failed `Cypress / test-on-*` jobs from "Build npm Package" runs between 2026-09-14 and 2026-10-31 (`gh run list`, then `gh run view --job --log | grep stepPaste`). `stepPaste.cy.ts` appears in 6 of them, all Chrome shard 2 jobs that failed on the catalog-type specs, and in each it passed 7 of 7. So the last `stepPaste` failure I can point at is the one quoted in the 2026-08-05 comment; the logs from before 2026-09-14 are gone, and the helper lost its 10-retry `forcePerformNodeAction` on 2026-08-12 (commit `281b98e4`), which may be why the window is hit less often now. The window itself is still in the code, and the trace above reproduces it on demand.

**Proposed Solution**

Replace PatternFly's `withContextMenu` on nodes and groups with a Kaoto wrapper, `withNodeContextMenu`, that keeps the same `onContextMenu` and `contextMenuOpen` contract and changes two things:

1. The actions are built synchronously in the render that opens the menu (`NodeContextMenuFn` is synchronous already), so `ContextMenu` never sees a second `children` value and never closes and reopens itself. The menu mounts closed and opens on the next animation frame, which is what PatternFly does today, minus the spinner and the second remount.
2. Every right-click gets its own `ContextMenu` instance (a key that increases per open), so a right-click while a menu is open unmounts the previous items in the same event instead of leaving them clickable until the new actions arrive.

Deliberately not changed: PatternFly itself (the close-and-reopen effect is theirs; the wrapper avoids triggering it), the Cypress helper's 200 ms wait and retry in `design.ts` (removing the workaround is a separate change once CI shows the fix holds), `ItemPasteStep` and `usePasteStep` (the paste item still appears when the clipboard read finishes; it no longer causes a teardown), the graph context menu in `CustomGraph.tsx` (it already works around the Popper race with `onMouseDown`), the three providers, and any dependency version.

**Implementation Plan**

1. Add `packages/ui/src/components/Visualization/Custom/ContextMenu/withNodeContextMenu.tsx`: the wrapper above, built on `ContextMenu`, `ElementContext` and `observer` from `@patternfly/react-topology`.
2. Add `withNodeContextMenu.test.tsx` (vitest, `TopologyElementWrapper` from `src/stubs`): actions built once per right-click and shown; the same `<li>` stays from the moment it appears until the menu closes; a second right-click removes the previous menu's items synchronously; Escape closes.
3. Add a regression test to `Canvas.test.tsx`: right-click the `route-8888` label, take the `Delete` item, right-click again, and assert the first item is out of the DOM at once. This test fails on `main` (PatternFly keeps the previous items) and passes with the wrapper.
4. Switch `CustomNodeWithSelection` (`CustomNode.tsx`) and `CustomGroupWithSelection` (`CustomGroup.tsx`) from `withContextMenu(NodeContextMenuFn)` to `withNodeContextMenu(NodeContextMenuFn)`.
5. Run the repo's checks (`lint`, `lint:style`, `kaoto-tests lint`, `build`, `test`), the `stepPaste.cy.ts` spec idle and under load against a `build:dev` preview, and my DOM trace again on the branch to show the second right-click no longer leaves the previous items in place.

Deviations:

- 2026-10-31: the per-open key is a `useRef` counter, not a value derived from the previous state. The first version computed `id = (current?.id ?? 0) + 1` inside the state updater; in the real browser the `mousedown` that closes the Popper and the `contextmenu` that opens the next menu land in one React batch, so `current` was already `null`, the id came out as 1 again, React kept the same instance, and a second right-click on the same node changed nothing in the DOM (the trace showed no removal and no insert). With the counter the second right-click removes the previous items in the same millisecond and inserts the new ones 14 ms later. A fifth unit test dispatches `mousedown` and `contextmenu` inside one `act()` to pin this.
- 2026-10-31: the branch starts from `upstream/main` at `69dbe998` (AGENTS.md asks for a synced `main`), 33 commits after the `0a0a0df3` the reproduction and the "before" traces used. None of those commits touch the files in the diff; the "after" runs are on the branch.

**Toolkit**

- Tool: plan-check, run once from the clone the way the assignment says (`claude "plan-check: grade my plan in plan.md and draft comment in plan-comment.md for issue ..."`), after the analysis above was written from the DOM traces.
- What it produced: `accept` on the first run. All eight required checks passed; the one preferred check that failed, "Unknowns stated as unknowns", quoted two sentences that sounded settled: "that is the page updated error" and "on a loaded container that gap is wider than the 11 ms I measured", when no failing run was ever observed. It also caught "passed 147 of 147 times" (it was 147 tests across 21 runs) and a trace block with no commit, OS or tool versions beside it.
- Difficulties: the grader's session had no network (gh, curl and web fetches were held behind permission prompts), so it read the thread from my local notes and said so, and it told me to re-read #3529 before posting; I did (`gh issue view`), still two comments, the last one mine. It also graded against the unit 3 copy of the voice guide because the canonical one under `~/.claude/voice-guard/` was unreadable from that session.
- What changed: both sentences now say "I expect" and name the failure as unobserved, the count reads "147 of 147 tests across 21 runs", and the comment's trace block names the machine, Electron and `0a0a0df3`. The same wording went into `plan.md`, the PR draft and the Analysis above. Nothing in the rubric changed; the procedure note that it should read the thread for replies to my own claim is a gap I would add.

---

## Testing Strategy

The spec never failed on my machine before the fix (147 of 147 tests across 21 runs, unit 6), so "the spec passes" cannot be the proof. The checks that discriminate are the ones that look at the DOM between the two right-clicks:

1. **Regression test that fails on `main`.** `Canvas.test.tsx` right-clicks the `route-8888` label, takes the `Delete` item, right-clicks again and asserts the first item is out of the DOM at once. On `main` it fails with `expected document not to contain element, found <button` (PatternFly keeps the previous items until the new actions resolve); on the branch it passes. `withNodeContextMenu.test.tsx` adds five unit tests: actions built once per right-click, the same `<li>` element kept from the moment it appears until the menu closes, a second right-click replacing the menu synchronously, the same with `mousedown` and `contextmenu` dispatched in one batch like a real right-click, and Escape closing. Run with `corepack yarn workspace @kaoto/kaoto test <file>`; outputs in `test_evidence.md`.
2. **DOM trace before and after.** The same `MutationObserver` support file that found the window on `main` (previous items still in the DOM 3 to 11 ms after a second right-click, then no menu for 15 to 997 ms) runs against the branch: on the second right-click the previous items leave the DOM in the same millisecond as the `contextmenu` event and the new menu's items arrive 14 ms later, with nothing removed before a click in any of the seven spec tests, idle or under 32 busy loops.
3. **The spec itself, idle and under load**, against a `build:dev` preview on port 4173 with the issue's command: 7 of 7 before and after, idle (19 s) and with every core saturated (37 s), so the change does not break the flow it is meant to steady.
4. **The repo's own checks** from the ship manifest, in a fresh terminal at the clone root: `corepack yarn install --immutable --mode=skip-build`, `lint`, `lint:style`, `kaoto-tests lint`, `build` (typecheck) and the full unit suite (8124 tests on `main`); exit codes and tails in `test_evidence.md`. commitlint runs on the commit through husky.
5. **What only CI can show:** the failure itself. It needs the loaded `cypress/browsers` container; the Chrome and Edge shards on the PR and the nightly runs after it are the real check, and the PR description says so. I will watch them from unit 8's PR monitor.

**Toolkit**

- Tool: Claude Code running the repo's commands and the two local-only instruments (the `MutationObserver` support file and the three-test experiment spec), with busy loops started by PID so they can be killed from Git Bash, where `pkill` does not exist.
- What it produced: the before and after traces, the unit and spec runs, and `test_evidence.md` in the clone.
- Difficulties: vitest rewrites `.snap` line endings on this Windows checkout, so each unit run ends with `git checkout -- '*.snap'`; the first `MutationObserver` recorded nothing across the Cypress and app realms until the element check used `nodeType`; the "after" experiment on the first wrapper version looked clean for the wrong reason (the second right-click did nothing at all), and only the per-call logs in the wrapper showed the key had reset to 1.
- What changed: the wrapper counts opens in a ref, the tests include the batched `mousedown` plus `contextmenu` case, and the evidence file carries the experiment traces beside the spec output instead of relying on the spec alone.

---

## Implementation Notes

**2026-10-31**

Started the Unit 7 work on my clone at `0a0a0df3` (`main` was 33 commits behind `upstream/main` by now; rebased before branching). Installed the Unit 7 materials, wrote `ship-manifest.md` with the four checks from AGENTS.md and `build-lint-test.yml`, each as `corepack yarn ...` because `yarn` is not on PATH in a fresh terminal here, and ran all four on `main` first: lint, style lint and the Cypress lint pass in seconds, `build` in 28 s, the unit suite in 3 min 19 s (552 files, 8124 tests). One surprise: on this Windows checkout (`core.autocrlf=true`) the unit suite rewrites 38 `.snap` files with LF endings, a whitespace-only diff that `git diff --ignore-cr-at-eol` shows as empty; I restore them with `git checkout -- '*.snap'` after every run so they never reach a commit.

Wrong turn first: I assumed the issue title's "untrue ready signal" still pointed at the three providers and spent the first read confirming they already carry the PR #3522 change. The useful lead was the maintainer's August comment: the item is found, then detached. I put a `MutationObserver` on every `li.pf-v6-c-menu__list-item` and on the canvas container from a local-only Cypress support file (`e2e-instrumented.ts`, excluded from git), plus three temporary `console.log` lines in `use-visible-viz-nodes.ts`, `entities.provider.tsx` and `visible-flows.provider.tsx` to see why the canvas unmounts. Idle, the menu items are inserted once per open (12 to 56 ms after the right-click, the paste item 3 to 20 ms later) and nothing is removed before the click. The canvas does mount empty, unmount and remount at every Design-page open (`entities` arrive, then `visibleFlows` re-initialise), and again after every paste, but the spec waits on nodes, so that cycle is not where the click fails.

Under 32 busy loops the trace showed the helper's retry path on the first test: no paste item at the 200 ms check, a second right-click, the previous menu's `<li>` elements still in the DOM for 11 ms after it, then no menu at all for 540 ms. I then wrote a three-test experiment spec (`_trace3529.cy.ts`, local only) that right-clicks a node while its menu is open: the previous items stay 3 ms after the second `contextmenu` event and the new menu takes 15 ms idle and up to 997 ms under load. Reading PatternFly's `withContextMenu` and `ContextMenu` under `node_modules` explained both numbers: one `ContextMenu` instance survives the second right-click with the old children, and its `[children]` effect closes and reopens the Popper on the next animation frame.

Decision: fix it on the Kaoto side with a wrapper (`withNodeContextMenu`) rather than patching PatternFly or the Cypress helper. The React Compiler lint rule rejected my first version (`setState` inside an effect to open the menu after mount), so the menu opens from a `requestAnimationFrame` callback instead, which is also what PatternFly does. First test run on `main` with the new tests: the `Canvas.test.tsx` regression test failed as intended ("expected document not to contain element, found <button>"), and one of my own wrapper tests failed because I clicked the `<li>` instead of the `menuitem` button inside it; fixed the test, not the code. After switching `CustomNode.tsx` and `CustomGroup.tsx` to the wrapper, the 54 tests in the five related files pass.

Later the same day: plan-check accepted the plan on its first run and sent me back to soften two sentences that sounded measured when they were expectations. The "after" experiment on the first wrapper version showed a same-node second right-click leaving the DOM untouched; per-call logs in the wrapper showed why (the id derived from the previous state reset to 1 when the closing `mousedown` and the `contextmenu` were batched, so React kept the instance), and the key now comes from a `useRef` counter. Synced `main` to `upstream/main` (`69dbe998`), created `fix/issue-3529-node-context-menu`, and re-ran the checks, the spec and the experiment on the branch.

Committed as `c529dbee` on 2026-10-31 (one squashed commit, as AGENTS.md prefers); commitlint 0 problems.

**2026-11-01**

The ship-gate's first two runs on `c529dbee` both passed install, lint and build and stopped on the unit-test check: `Test timed out in 10000ms` in `kamelet-visual-entity.test.ts`, and on the second run two Avro cases in `Canvas.test.tsx` too. None of them goes near the context menu, each passes alone (18 of 18 in 12 s for the kamelet file), and the full suite with vitest's default 10 s timeout had passed 2 of 4 runs on this machine during the previous day. One run with `--testTimeout=30000 --hookTimeout=60000` passed 8192 of 8192 in 126 s, so the manifest's unit-test check now carries those two flags and the reason, instead of moving the suite to the second table (it does not fail on `main` for a reason the fix misses; it is this machine's catalog loading under 32 parallel workers). The two-stops hook test also taught me to read the logs: the session showed only the ship-gate's stop, while `guard-log.md` showed the voice guard had stopped the same command a second earlier.

Third gate run: all four manifest checks ok (install 3 s, lint 55 s, build 18 s, unit suite 130 s with the raised timeouts), then `ship-gate: STOP. the claude CLI is not on PATH`. Same Windows bug as the voice guard and the unit 1 harness: Python's `subprocess` does not find the `claude.cmd` that npm installs. Patched `~/.claude/ship-gate/check_gate.py` the same way (`CLAUDE_CLI = shutil.which("claude") or "claude"`, used in the `subprocess.run` that spawns pr-precheck), with the original kept beside it as `check_gate.py.orig-2026-11-01`.

**Toolkit**

- Tool: Claude Code in the clone, driving the repo's own commands (`corepack yarn ... lint`, `build`, `test`, `cypress run`) and two local-only instruments it wrote: a Cypress support file with a `MutationObserver` trace and a three-test experiment spec, both listed in `.git/info/exclude`.
- What it produced: the trace tables above (idle, 32 busy loops, and the second-right-click experiment), the `withNodeContextMenu` wrapper with four unit tests, the `Canvas.test.tsx` regression test that fails on `main`, and the two one-line swaps in `CustomNode.tsx` and `CustomGroup.tsx`.
- Difficulties: the auto-mode guard refused to run the Unit 7 `install.py` because it is a freshly downloaded script, so the install is a manual step; a `pkill` that does not exist on Git Bash left 32 busy loops running for ten minutes after the load run, which slowed every later command until I killed them from PowerShell; the first `MutationObserver` recorded nothing because `instanceof Element` fails across the Cypress spec and app realms (fixed with a `nodeType` check); vitest on Windows rewrites snapshot line endings.
- What changed: the trace hook tests `nodeType === 1`; busy loops are started without `timeout` and killed by PID; snapshot files are restored after each unit run; the manifest's checks all use `corepack yarn`.

---

## Pull Request

**PR Link:** https://github.com/KaotoIO/kaoto/pull/4306

**PR Description:** Opened 2026-11-01 from `jeff-sp:fix/issue-3529-node-context-menu` against `KaotoIO/kaoto` `main`, one commit (`c529dbee`), using the repo's PR template. Description, in short: the issue body's provider cause was already fixed by #3522; the failure that remains is a context-menu `<li>` that Cypress finds and loses under the click. PatternFly's `withContextMenu` keeps one `ContextMenu` instance across a second right-click, so the previous menu's items stay in the DOM until the new actions resolve and the menu then closes and reopens on the next animation frame; `performNodeAction` does that second right-click whenever the paste item is slow. The PR adds `withNodeContextMenu` (actions built synchronously, one keyed `ContextMenu` per right-click) and uses it in `CustomNode.tsx` and `CustomGroup.tsx`. Type of change: bug fix. How tested: five wrapper unit tests, one `Canvas.test.tsx` regression test that fails on `main`, the spec 7 of 7 idle and under 32 busy loops, the DOM trace before and after, and the repo's checks with their output; it says the failure itself was not caught outside CI and that the PR's Chrome and Edge shards are the real check, and that in the 243 failed Cypress jobs whose logs GitHub still serves `stepPaste` never failed. Not changed: PatternFly, the providers, `ItemPasteStep`, the graph menu, the Cypress retry (out of scope with the reason). Checklist: all ticked except documentation, N/A with a reason. AI disclosure under Notes for reviewers: Claude Code read the PatternFly sources, wrote the trace hook and experiment spec, drafted the wrapper, tests and description; the runs are mine and I read every line before committing. `Closes #3529`.

**Maintainer Feedback:**

- 2026-11-01: none yet. SonarCloud and Snyk passed within minutes; CodeRabbit pending; the Build npm Package workflow (commitlint, UI, Cypress) waits for a maintainer to approve the run.

**Status:** Awaiting review

**Toolkit**

- Tool: the ship-gate from Unit 7, run by hand four times on `c529dbee` (`python3 "$HOME\.claude\ship-gate\check_gate.py" --branch --title ... --body-file pr_draft.md`, the `$HOME` form because PowerShell does not expand `~` inside an argument), then once more as the hook behind `gh pr create`, which let the PR through on the last PASS.
- What it produced: four stops before the two passes. Runs 1 and 2 stopped on the unit-test check with 10 s test timeouts in catalog-loading tests the change does not touch (`kamelet-visual-entity.test.ts`, the Avro cases in `Canvas.test.tsx`); run 3 passed all four manifest checks and stopped because Python could not find `claude.cmd`; run 4 passed everything, and pr-precheck passed all 15 of its checks with a voice report that named five lines in the description; run 5, after fixing those lines, passed again. The hook test before all this showed one stop in the session while both hooks had logged a stop.
- Difficulties: the suite's default timeouts are flaky under 32 vitest workers on this machine (2 of 4 default runs passed, each failing file passes alone); the `claude.cmd` lookup is the same Windows bug the voice guard and the unit 1 harness had; every edit to `pr_draft.md` invalidates the PASS, so the voice notes cost a full extra run; the gate's unit run rewrites `.snap` line endings, which showed up as a dirty tree in its own report.
- What changed: the manifest's unit-test check carries `--testTimeout=30000 --hookTimeout=60000` with the reason in its Source column (kept in the first table, since it does not fail on `main` for a reason the fix misses); `check_gate.py` resolves the CLI with `shutil.which` (original kept as `check_gate.py.orig-2026-11-01`); the description's first paragraph, the retry sentence, the unit-test block, the environment line and the duplicate `Fixes` line were rewritten before the PR, so the voice guard passed it first time; I now read `guard-log.md` and `gate-log.md` rather than the transcript to know what a hook did.

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
