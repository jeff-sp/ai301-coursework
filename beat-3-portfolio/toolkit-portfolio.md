# Toolkit Portfolio

**Student:** Jeff San Pedro

My toolkit is a set of Claude Code skills and hooks that check my work before it reaches an open source maintainer: one picks the issue, the others stop a claim, a plan, a pull request or a reply that is not ready. I used it to fix [KaotoIO/kaoto#3529](https://github.com/KaotoIO/kaoto/issues/3529). The fix was merged on 2026-11-11 as [PR #4306](https://github.com/KaotoIO/kaoto/pull/4306).

## Featured Tools

I feature issue-scout, ship-gate and voice-guard because all three changed during the real Kaoto work, not only in the sandbox. Each one stopped me at least once on something I would have posted, and each one needed a fix of its own before it worked on my Windows machine.

**issue-scout**

**What it is, and the judgment hole I wrote:** A skill that searches GitHub for first-contribution issues, ranks them with free filters, and grades the top few against a rubric. The parts I wrote are `search-strategy.md` (four TypeScript queries and five ranking filters, each tied to my fit profile) and `rubric.md` (the checks an issue must pass, such as Maintainer alive, Unclaimed and AI policy).

**How it evolved, dated:**
- 2026-10-18: the first scan graded 5 issues and 3 came from one org, so I changed the cap from 2 per repository to 2 per owner.
- 2026-10-18: all 5 graded issues were filed that day in repos under 30 days old, so I added a preferred check, "Repo has adoption".
- 2026-10-18: one repo's hooks reject AI co-author trailers, so the AI-policy check now names trailer rules.
- 2026-10-24: I replaced "Scope fits newcomer" with `scope-feasible` plus a difficulty note. Things I used to reject on now raise the note instead. The regression run went from 20/20 to 18/20, and I accepted both misses as the change doing what it says.

**Evidence:** The cap, before and after, from `scan-output.md`:

```
F4 cap 2 per repository: 74 dropped, 174 left across 145 repos.
```

```
Filter 4 changed from "2 per repository" to "2 per owner". Same 388 results: F4 now drops 80 (168 left, 139 repos).
```

The read-out for the issue I chose, after the scope change (2026-10-24):

```
2. KaotoIO/kaoto#3529, flaky `stepPaste.cy.ts` (accept, ambitious)
- scope-feasible: pass. The issue names one flaky spec and three loading providers as suspects.
- Unclaimed: pass. There is no assignee, no linked PR (my search for PRs mentioning the issue found none), and no claim comment.
- Difficulty: ambitious, for two reasons:
  - The latest comment (`tplevko`, 2026-08-05) says the original failure no longer appears, likely fixed by PR #3522, and no maintainer has replied.
```

**ship-gate**

**What it is, and the judgment hole I wrote:** A hook that stops `gh pr create` until the repo's own checks pass on my branch and my pr-precheck skill accepts the PR text. The part I wrote is `ship-manifest.md` in my Kaoto clone: four must-pass checks taken from the repo's AGENTS.md and CI workflow, and two checks I excluded with a dated reason (Cypress e2e and commitlint).

**How it evolved, dated:**
- 2026-10-31 and 2026-11-01: two runs stopped on the unit-test check with 10 second timeouts in tests my change does not touch. Each failing file passed alone, and the full suite passed 2 of 4 runs with the defaults. I kept the check and added `--testTimeout=30000 --hookTimeout=60000` to it, with the reason written in the manifest.
- 2026-11-01: the third run passed all four checks and then stopped with "the claude CLI is not on PATH". On Windows, npm installs the CLI as `claude.cmd`, which Python does not find. I patched `check_gate.py` to resolve it with `shutil.which`.
- 2026-11-01: the next run passed. pr-precheck passed all 15 checks, and its voice report named five lines in my PR description. I fixed them, the gate passed again, and the hook let the PR through.

**Evidence:** From `gate-log.md`, the stop and the pass:

```
## 2026-10-31 22:03 [manual] KaotoIO/kaoto @ fix/issue-3529-node-context-menu (base main)
- verdict: BLOCK (block)
  - Unit tests (vitest, full `@kaoto/kaoto` suite): `corepack yarn workspace @kaoto/kaoto test` -> FAILED (exit 1, 128s)
    FAIL  src/models/visualization/flows/kamelet-visual-entity.test.ts
    Error: Test timed out in 10000ms.
```

```
## 2026-11-01 12:24 [manual] KaotoIO/kaoto @ fix/issue-3529-node-context-menu (base main)
- verdict: PASS
  - Unit tests (vitest, full `@kaoto/kaoto` suite): `corepack yarn workspace @kaoto/kaoto test --testTimeout=30000 --hookTimeout=60000` -> ok (172s)
  - Repo checks run with outcome shown [pass]: lint/lint:style/kaoto-tests lint exit=0, build exit=0, test "Tests 8192 passed | 25 skipped", commitlint exit 0

## 2026-11-01 12:28 [hook] KaotoIO/kaoto @ fix/issue-3529-node-context-menu (base main)
- verdict: PASS (honored the manual pass at 2026-11-01 12:24)
```

**voice-guard**

**What it is, and the judgment hole I wrote:** A hook that reads anything I am about to post with `gh` (an issue comment, a PR, a PR comment) and stops it if it breaks my voice guide. The part I wrote is the guide: rules such as "Promise the investigation, not the fix", "Show it, then say it" and "Say what I did not do", each with a wrong and a right example, plus a never-post list.

**How it evolved, dated:**
- 2026-10-24: I had three copies of the guide in three skill folders. I folded them into one file and left a one-line pointer in each folder, because `ln -s` copies instead of linking on Git Bash.
- 2026-10-24: the first manual check failed with "the claude CLI is not on PATH", the same Windows bug, and got the same `shutil.which` patch.
- 2026-10-24: my never-post list banned any date, which collided with saying when I would report back. I narrowed the rule to fixed dates and allowed a report-back window.
- 2026-11-08: the guard stopped my 52-word reply to a maintainer on some runs and passed it on others. I ran each candidate several times. The version that says only what I changed passed 4 of 4. I learned that the judge is a model call and one pass is not proof.

**Evidence:** From `guard-log.md`. The first two lines are my claim draft, the next two are the real post on the Kaoto issue:

```
- 2026-10-24 20:58 [manual] stop: rule broken: "An environment I did not record: no report goes up without the commit hash, the OS, and the tool versions I ran." | target: claim-draft.md
- 2026-10-24 20:59 [manual] pass | target: claim-draft.md
- 2026-10-25 15:56 [hook] stop: rule broken: "A statement about behavior comes after the output that shows it. If I have no output to paste, I write what I did instead of what I concluded." | target: gh issue comment https://github.com/KaotoIO/kaoto/issues/3529 --body-file
- 2026-10-25 15:57 [hook] pass | target: gh issue comment https://github.com/KaotoIO/kaoto/issues/3529 --body-file
```

## Inventory

- issue-select: my unit 1 rubric for grading a single issue. It scored 19/20, then 20/20 after I moved the liveness check from maintainer comments to commit dates, and it became the base of issue-scout's rubric.
- repro-check: grades a claim comment and a reproduction report before I post. 19/20, then 20/20 after one row stopped treating an OS difference as a missing trigger. On the Kaoto claim it rejected my first package on "Outcome stated as observed".
- plan-check: grades a plan and its plan comment. 20/20 on its first full run and unchanged since. It accepted my Kaoto plan and flagged two sentences that sounded measured when they were expectations.
- pr-precheck: grades a PR against its plan and issue. 18/20, then 20/20 after I reworded one evidence row. It now runs inside ship-gate and passed all 15 checks on PR #4306.
- pr-monitor: reads the state of my open PR and drafts the next move from a playbook I wrote. On 2026-11-05 it reported that a maintainer's question had been waiting three days.
- Repo memory file (`CLAUDE.local.md` in my Kaoto clone): 53 lines of facts about the repo so each session starts with them. I added the PatternFly `node_modules` paths after the first read missed them.

## The Arc

**Beat 1**
Date: 2026-09-20
Friction: My first issue rubric failed a healthy repo because it judged liveness by "a maintainer commented within 30 days". The eval run scored 19/20.
Change: I moved liveness to commit dates: at least one default-branch commit by a human within 90 days.
Result: The next eval run scored 20/20.

**Beat 2**
Date: 2026-10-11
Friction: pr-precheck scored 18/20 with two false rejects, because one row only accepted "a captured run in the evidence or an explicit 'not run' sentence".
Change: I reworded the row so a named case counts when the evidence mentions it with its outcome.
Result: The next run scored 20/20, and three weeks later the same rubric passed my real Kaoto PR on all 15 checks.

**Beat 3**
Date: 2026-10-18
Friction: issue-scout's first scan gave 3 of its 5 graded slots to one org, because the cap was per repository.
Change: I changed the cap to 2 per owner.
Result: Re-ranking the same 388 results dropped both tdk-cli-core issues from the top 5. Kaoto #3529 still did not come from the scan. It came from grading the course site's picks with the same rubric.

**Beat 4**
Date: 2026-10-24
Friction: My scope check rejected issues for reasons that were really difficulty, not scope, and it had rejected Kaoto #3528 on a rule I had added the week before.
Change: I replaced it with `scope-feasible` plus a difficulty note.
Result: #3529 came back "accept, ambitious" and ranked second. I chose it anyway, and the regression run fell from 20/20 to 18/20.

**Beat 5**
Date: 2026-11-01
Friction: ship-gate stopped my branch twice on unit-test timeouts in tests my change does not touch.
Change: I added `--testTimeout=30000 --hookTimeout=60000` to the manifest's unit-test check, with the reason, instead of removing the check.
Result: The gate passed at 12:24 and the hook let `gh pr create` through at 12:28. PR #4306 was open that day.

**Beat 6**
Date: 2026-11-08
Friction: voice-guard stopped my reply to a maintainer on "Show it, then say it", then passed and stopped the same text on different runs.
Change: I ran each candidate several times and kept the one that says only what I changed and leaves the behavior to the trace in the PR description.
Result: The 52-word reply passed the hook and was posted on 2026-11-08. A maintainer approved and merged the PR on 2026-11-11.

## Limitations and What's Next

The toolkit checks what I write and what I can run on my own machine. It could not make the Kaoto test fail (21 runs, 147 of 147 tests passed), and it cannot re-run CI on a repo where I only have pull access, so the fix rests on a DOM trace and unit tests, and only later nightly runs can show the flake is gone. Every tool that calls the Claude CLI also needed the same Windows patch (`shutil.which`) before it ran at all, which I fixed four times in four places instead of once.

Next I would check Kaoto's nightly runs for `stepPaste` now that the fix is merged, move issue-scout's per-owner cap to run after the fit ordering, and put the CLI lookup in one shared helper that all four scripts import.

**Tool limits.**

issue-scout: its own search never surfaced the issue I ended up fixing, and the per-owner cap runs before the fit ordering, so a good-fit issue can lose its slot to a worse one from the same owner.
ship-gate: it only runs checks that fit in one terminal command, so the Cypress e2e spec, the test my fix was about, is excluded from the gate.
voice-guard: its judge is a model call, so the same text can stop on one run and pass on the next. A single pass is not proof.
