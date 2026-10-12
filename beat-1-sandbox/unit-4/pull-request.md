# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/145

**Branch**

docs/47-curl-examples-per-endpoint

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

All runs on 2026-10-11 from WSL (Ubuntu), `python3 run_eval.py --skill /mnt/c/Users/jeffj/.claude/skills/pr-precheck`, model sonnet (pinned by the harness). As in units 2 and 3, the harness runs from WSL because Windows Python cannot spawn the `claude.cmd` shim; the skill directory itself is the Windows install, symlinked into WSL's `~/.claude/skills/` so the live run (step 10) and the eval runs grade the same files.

1. Smoke run, `--limit 2` (pkg-01, pkg-02): "agreement: 2/2 scored items". pkg-01 reject (gold reject, standards-wall), pkg-02 accept (gold accept, clear-accept). A partial run, so it decides nothing; it confirmed the harness preflight accepted the frame, rubric, procedure and evidence guide and that the JSON parsed.
2. Full run with `--save-run eval-run.txt`: "agreement: 18/20 scored items  (bar: 18/20: PASS)". Categories line: "clear-accept 5/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3". Two disagreements, both clear-accepts graded reject on the same row: pkg-05 and pkg-19, note "failed: Every planned failure mode exercised".
3. `--only pkg-05,pkg-19,pkg-04,pkg-07,pkg-10,pkg-14,calib-04 --include-calibration`, after the revision described under Check rationale: "agreement: 6/6 scored items", categories "clear-accept 2/2  not-tested 4/4"; calib-04 also agreed (reject). pkg-05 and pkg-19 flipped to accept; the four not-tested canaries and calib-04 stayed reject.
4. Confirming full run with `--save-run eval-run.txt`: "agreement: 20/20 scored items  (bar: 18/20: PASS)". Categories line: "clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3". This is the committed `eval-run.txt`; its header fingerprints (rubric `507079dcfd6d624b`, procedure `27c775799fd86842`, evidence-guide `07d89f7777bcfa71`, SKILL.md `387271870a27b179`) match the files in `tools/pr-precheck/`.

**Package analysis**

pkg-19 (mikefarah/yq#2819, category clear-accept). My rubric on run 2: reject. Gold: accept, note "terse but complete: one emitter-state fix mirroring the double-quote writer, byte-identical round-trip shown with diff, regression scenario added". After the revision, runs 3 and 4: accept.

Fourteen of my fifteen rows passed it on run 2. The diff touches exactly the two files the plan names (`vendor/github.com/mikefarah/yaml/v3/emitterc.go` and `pkg/yqlib/operator_style_test.go`), the description says "Per the plan, the reset is removed from the break-writing site" and nothing more, and the evidence captures the trigger on both sides: the issue's input through `yq .` on 4.53.3 with the dedented closing quote, then on the branch build with `diff <(./yq . source.yml) source.yml && echo IDENTICAL` printing `IDENTICAL`. The one row that failed was "Every planned failure mode exercised". The plan's test plan reads "re-run `yq . source.yml`, expect byte-identical round-trip of the issue's input; the two controls unchanged; `go test ./...`", and the evidence says of the controls only "Controls (double-quoted twin; single-quoted without the trailing blank line) unchanged", with no transcript. My first wording of the row demanded "a captured run in the evidence or an explicit 'not run' sentence" for every named case, so a control that is stated rather than shown failed it.

The gold set draws the line somewhere else, and reading the three packages that sit on it shows where. calib-04 (gold reject, not-tested) names two failure modes in its test plan, "re-run repro 1 ... re-run repro 2 (`-j 200000 -x echo .`)", and its evidence shows repro 1 and says nothing at all about repro 2. pkg-05 (gold accept) names a second case, "re-run with two same-name same-key bindings, expect one row plus a warning", and its evidence states "Same-key redefine prints the one-time warning" without a transcript. pkg-19 is the same shape as pkg-05. So the line is between a named case the evidence accounts for in words and a named case the evidence never mentions, not between captured and stated. The primary case still has to be captured, but that is a different question, and my rubric already asks it in "Before and after captured" (which is what holds pkg-04's "colors work now" and pkg-10's "a full day of syncs"). Once the row only asked its own question, pkg-19 passed on runs 3 and 4 and calib-04 still failed, on "no word on repro 2".

**Check rationale**

Quoted from `tools/pr-precheck/rubric.md`:

| Every planned failure mode exercised | Each failure mode, case, or command the plan's test plan names by itself ("both failure modes by name: repro 1 ... repro 2", "re-run with two same-name same-key bindings", "physical-path rendering unchanged"), read against the test evidence. A named case is accounted for when the evidence mentions it with its outcome: a captured run, a stated result ("the `-j 64` control unchanged", "same-key redefine prints the one-time warning"), or an explicit "not run" with the reason | Pass when every named case is accounted for. Fail when a named case is not mentioned in the evidence at all (calib-04's plan names repro 2, `-j 200000 -x echo`, and the evidence shows repro 1 only with no word on repro 2). Whether the primary case is captured rather than asserted is decided by Before and after captured, not here | required |

Two decisions shaped this row, one before the first run and one after it.

Before the first run, I split what had been one "not tested" check into three rows that each decide one thing: "Before and after captured" (is the primary observable shown on both sides, or only asserted), "Evidence exercises the trigger path" (is the run the repro's trigger, or its control, as in pkg-07's single-file case and pkg-14's GET with a custom header), and this row (is every case the test plan names accounted for). The unit 1 and unit 2 feedback was that a compound row is a function with too many responsibilities, and that a fix "usually reads more cleanly as a tighter definition in the evidence column ... rather than as an exception clause in the pass condition; definitions compose, exceptions accumulate". The split is what made the run-2 disagreement cheap to diagnose: the `note` column named one row, and that row asks one question, so the fix touched one cell.

After run 2, the fix went into the Evidence column as a definition, "A named case is accounted for when the evidence mentions it with its outcome", with the three forms that count listed, and the pass condition shrank to "every named case is accounted for" versus "not mentioned in the evidence at all". I rejected two other fixes. Deleting the row would have let calib-04 through, since its primary run is captured and decisive and only the second named failure mode is missing; "Before and after captured" cannot hold it. Keeping the row but weakening it to preferred would have had the same effect on the verdict. Moving the capture-versus-assertion question into this row (so a stated control fails here) is what run 2 did, and it false-rejected two of the seven clear-accepts. The last sentence of the pass condition names the row that does decide capture, so a reader can see the boundary, but it adds no condition of its own; the row still decides one thing.

**Trade-offs**

The revision loosened a required row, so before the confirming run I re-ran the two affected packages with canaries from every category the change could touch: `--only pkg-05,pkg-19,pkg-04,pkg-07,pkg-10,pkg-14,calib-04 --include-calibration`. All four not-tested packages stayed reject (pkg-04 and pkg-10 on "Before and after captured", pkg-07 and pkg-14 on "Evidence exercises the trigger path"), calib-04 stayed reject on this row, and pkg-05 and pkg-19 flipped to accept. The standards-wall pair (pkg-01, pkg-20) reads the description against the template and the AI policy and never reads the test plan, so it was not in the canary list; it held at 2/2 on the confirming run, as did silent-drift and unreviewable. The fingerprints in `eval-run.txt` match the files in `tools/pr-precheck/`, so the canary run graded the same rubric the full run did.

What the row now gives up: a PR whose evidence says "control unchanged" without ever having run the control passes this row. I accept that because the eval set accepts it (pkg-05, pkg-19, and the cost of the alternative was two false rejects among seven clear-accepts), and because the primary observable still has to be captured, so the lie would have to be about a secondary case. On my own PR this is a live gap: my `test_evidence.md` shows all nine curl examples with their statuses, so nothing there is merely stated, but a future PR of mine could coast on a stated control and this row would not notice.

One mismatch between my tool and my practice, named rather than hidden: `scope.md` (a staff file I do not edit) gives the house branch shape as `fix/<issue-number>-<slug>`, and my branch is `docs/47-curl-examples-per-endpoint`. Path Review's `docs/CONTRIBUTING.md` lists `docs` among the allowed types and gives `docs/11-hybrid-scoring-formula` as an example, and the assignment accepts `docs/` too, so I kept the branch and made the skill report, not reject, a type prefix read against CONTRIBUTING's list (SKILL.md, scope seam). The live run on my draft reported the branch as matching and graded the package accept on all fifteen rows.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
