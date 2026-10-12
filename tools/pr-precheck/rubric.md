# Rubric: is this pull request ready to submit?

Each row decides exactly one thing, and the Evidence column carries
the definitions that decision needs (what the plan's boundary is made
of, what the trigger path is, what counts as a required ask), so no
pass condition needs an exception clause or a pointer to another row.
Rows 1 to 4 read the diff and the description against the plan
(silent drift); rows 5 to 8 read the test evidence against the plan's
test plan, the repro, and the repo's checks (not tested); rows 9 and
10 read the diff for things a reviewer would have to strip out
(unreviewable); rows 11 to 13 read the description against the repo's
stated asks (standards wall). Rows 14 and 15 are feedback only. Every
row judges the artifact itself, never the write-up's length, headings,
or commit count: a two-sentence description over a one-line diff
(calib-01) passes every row.

The honest-outcome rule runs through rows 1 to 4: a PR that does less
than its plan is ready when the plan's deviation note records the
deferral and the description restates it (pkg-13 defers `%VAR%`
expansion, pkg-16 defers the start-time path; both accept). Drift is
what the notes do not say: a hunk the plan never mentions (calib-03's
`print/property.js` rewrite), a feature the plan scoped out (pkg-03's
`resolve_symlinks` option), or a description claiming a deliverable
the diff does not contain (pkg-17's "documentation now documents the
limitation" over a diff with no docs file).

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diff adds nothing outside the plan | Every file and hunk in the diff (`### Diff` in the bundle; live, `git diff main...HEAD`), read against the plan's boundary. The boundary is the union of the plan's "Scope", "In scope", "Files", "Changes", or "Approach" statements, its "Not in scope" statements read as exclusions, and every deviation note the plan records (`## Deviations` live). A hunk is inside the boundary when it edits a file the plan names (or a test or changelog file for that change) and does work one of the plan's committed changes calls for | Pass when every hunk is inside the boundary. Fail when any hunk edits a file the plan does not name and no deviation note names it, adds a new option, flag, setting, prop, or config field the plan did not commit to, rewrites or renames code the plan did not touch, or does work the plan's "Not in scope" excludes. Unclear when the bundle has no diff | required |
| Diff delivers every planned change | Each change the plan commits to (its numbered changes, "Scope: one bounded change" sentence, "Files" list, and named deliverables such as "a dev warning AND a docs update"), read against the diff and the plan's deviation notes. A planned change is delivered when a hunk performs it; it is deferred when a deviation note or a "deliberately deferred" sentence in the plan names it as left out | Pass when every planned change is delivered or deferred by the plan's own note. Fail when a planned change is neither in the diff nor named as deferred by the plan. Unclear when the plan names no changes | required |
| Description claims match the diff | Every sentence of the description (`### Description`; live, `pr_draft.md` below line 1) that states what the change does, does not do, or contains ("implements the plan exactly", "no functional changes outside X", "docs now document Y", "CHANGELOG entry added", "tests added"), read against the list of hunks in the diff | Pass when no such claim is contradicted by the diff: each named deliverable has a hunk and each "nothing else" claim is true of the hunk list. Fail when a claimed deliverable has no hunk, or a "nothing beyond the plan" claim sits over a hunk outside the plan | required |
| Deferred work is restated in the description | Each deferral the plan records (a deviation note, "deliberately deferred", "known limit ... NOT covered", "Not handled here"), read against the description | Pass when every deferral the plan records is restated in the description, or when the plan records no deferral. Fail when the plan records a deferral the description does not mention | required |
| Before and after captured | The test-evidence section (`### Test evidence`; live, `test_evidence.md`), read against the plan's test plan and the repro evidence in `## Plan context`. A capture is a command or action shown with its result on the pre-fix tree and on the branch: output text, HTTP status, exit code, count, rendered state, or assertion result | Pass when the evidence shows the behavior on both sides with the observable named, or shows the after side with the before side quoted from the repro evidence. Fail when the evidence only asserts an outcome ("tested locally, colors work now", "verified working for a full day", "all tests pass") with no captured command and result. Unclear when there is no test-evidence section | required |
| Evidence exercises the trigger path | The commands or actions the test evidence runs, read against the trigger path. The trigger path is the repro step, input, or configuration that produces the behavior the issue reports, as `## Plan context` describes it (two files, one custom header, a sealed object, a symlinked cwd); the control run (one file, two headers, the extensible object) and any path the plan says is unchanged are not the trigger path | Pass when at least one captured run in the evidence is the trigger path. Fail when every captured run is a control run or an unchanged path. Unclear when there are no captured runs | required |
| Every planned failure mode exercised | Each failure mode, case, or command the plan's test plan names by itself ("both failure modes by name: repro 1 ... repro 2", "re-run with two same-name same-key bindings", "physical-path rendering unchanged"), read against the test evidence. A named case is accounted for when the evidence mentions it with its outcome: a captured run, a stated result ("the `-j 64` control unchanged", "same-key redefine prints the one-time warning"), or an explicit "not run" with the reason | Pass when every named case is accounted for. Fail when a named case is not mentioned in the evidence at all (calib-04's plan names repro 2, `-j 200000 -x echo`, and the evidence shows repro 1 only with no word on repro 2). Whether the primary case is captured rather than asserted is decided by Before and after captured, not here | required |
| Repo checks run with outcome shown | The check or suite commands the repo names (the `pull requests` and `contribution policy` lines in `## Repo facts`; live, the PR template's checklist and `docs/CONTRIBUTING.md`'s check commands), read against the test evidence. A check is shown run when its command or name appears with its result (pass count, "passes", "no tests ran", or a failure with its output) | Pass when each named check is shown run with its result, including an honestly reported failure with its reason, or when the repo names no check. Fail when a named check is never mentioned, or is mentioned without a result | required |
| No debugging leftovers | Every added line in the diff, read for leftovers: `print`, `eprintln!`, `console.log`, `DBG`, or `DEBUG` statements that are not part of the planned change; commented-out code (a `//` or `#` line containing code, not prose); functions, variables, or imports that nothing in the diff uses, including anything behind `#[allow(dead_code)]` or an `_unused` prefix; and `TODO`, `FIXME`, or "keeping for reference" notes added by this diff | Pass when the diff adds none of these. Fail when it adds any one of them | required |
| No unrelated hunks or churn | Every hunk in the diff, read for work a reviewer must strip out: a hunk that only reformats, re-indents, reorders imports, or re-prints identical lines; a hunk that renames or restructures code the planned change does not need; or a hunk in a file neither the plan nor the issue needs. A formatting change on a line the planned change also edits is part of that change, not churn | Pass when every hunk performs part of the planned change. Fail when any hunk is pure formatting, pure re-printing, a rename pass, or an edit the planned change does not need | required |
| Template sections filled | Each section and checklist item the repo's PR template requires (the `pull requests` line in `## Repo facts`; live, every heading and checkbox in `.github/PULL_REQUEST_TEMPLATE.md`), read against the description. A section is filled when it appears with content specific to this change; a checklist item that names an artifact (a changelog or whatsnew entry, a test) is honored when the description addresses it and the diff contains the artifact or the description says why it is absent | Pass when every required section and checklist item is filled, or when the repo has no PR template. Fail when a required section or checklist item is missing, empty, or left as the template's placeholder, or when a checklist artifact is claimed but absent from the diff | required |
| Issue referenced as the repo asks | The issue-reference form the repo asks for ("closes #xxxx", "fixes #<issue number>", "reference the issue") in the `pull requests` line of `## Repo facts` (live, the PR template's `Closes #` line), read against the description | Pass when the description carries the reference in the asked form, or when the repo states no reference ask. Fail when the repo asks for a reference and the description has none in that form | required |
| AI disclosure per repo policy | The `contribution policy` line in `## Repo facts` (live, `docs/CONTRIBUTING.md` and any AI policy file), read against the description. A policy requires disclosure when it says AI use must be disclosed or stated; a policy about human-written comments only, or with no AI statement, does not | Pass when the policy does not require disclosure, or when it does and the description names that AI was used and the extent (the tool, or what it helped with). Fail when the policy requires disclosure and the description is silent. Every package is treated as AI-assisted work | required |
| Commit subjects describe the change | Each subject line in the commit list (`### Commits`; live, `git log main..HEAD --oneline`) | Pass when every subject says what changed. Fail when any subject is a placeholder ("wip", "fix", "fmt + cleanup", "misc cleanups while debugging", "(hopefully)") | preferred |
| Title names the change | The PR title (`### Title`; live, line 1 of `pr_draft.md`) | Pass when a reader can tell from the title alone what behavior or text changed. Fail when the title names only a feeling or the bug's existence ("fixed the dead keys bug!!") | preferred |

## Verdict rule

Ready (accept) if every required check passes; otherwise hold
(reject). The two preferred checks (Commit subjects describe the
change, Title names the change) never change the verdict; they appear
in the output as feedback for the author. Unclear counts as fail on
every required row: a diff, a planned change, a test-evidence section,
or a captured run that cannot be located in the package is one a
reviewer cannot verify, and a PR that cannot be verified from its
package is not ready to submit. Live mode applies the same rows and
the same rule; the only differences are where the evidence is read
(the live locations named in each Evidence cell and in
`references/evidence-guide.md`). A deviation recorded in `plan.md`
under `## Deviations` is part of the plan for rows 1, 2, and 4, and a
shortfall it records, restated in the description, holds nothing.
