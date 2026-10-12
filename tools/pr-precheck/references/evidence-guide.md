# Evidence guide: where evidence lives in a PR package

An eval bundle has five sections in a fixed order: `## Repo facts`,
`## Issue`, `## Thread highlights`, `## Plan context`, and
`## Candidate PR`. `## Plan context` carries two paragraphs: "Repro
evidence (from the accepted report)" and "Plan:", and the plan
paragraph runs through the same stations every time in prose: the
diagnosis, "Scope, one bounded change:", "Not in scope:", "Files:",
"Test plan:", sometimes "Known limit" or "Deliberately deferred".
`## Candidate PR` carries `### Title`, `### Description`,
`### Commits`, `### Diff` (one unified diff, `--- a/` and `+++ b/`
per file), and `### Test evidence`. Live, the plan is `plan.md` in the
top folder of the working copy (sections "Diagnosis", "Scope",
"Files", "Approach", "Test plan", "Risks and unknowns", and
`## Deviations` written after the build); the diff is
`git diff main...HEAD` on the branch; the title is line 1 of
`pr_draft.md` and the description is the rest; the evidence is
`test_evidence.md`; the repo's asks are
`.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`. Read for
the sentence that does the job, not for the heading.

## Plan fidelity (harness category: silent-drift)

**Where it lives.** The plan's boundary is in `## Plan context`: the
"Scope, one bounded change:" sentence, the "Files:" list, and the
"Not in scope:" exclusions, plus any "Known limit" or "Deliberately
deferred" sentence (pkg-13: "`%` ... is NOT covered by this change";
pkg-16: "the start-time loading path ... keeps its current tolerant
behavior"). What actually changed is the `### Diff`: every `+++ b/`
file header and every `@@` hunk under it. The description's fidelity
claims are the sentences in `### Description` that say what the PR
does or does not contain ("Implements the posted plan exactly", "No
functional changes outside the system menu fix", "the reactivity-core
documentation now documents the limitation", "CHANGELOG entry
added"). Live: `plan.md`'s "Scope", "Files", and "Approach" sections
are the boundary, `## Deviations` re-ties any mismatch, `git diff
main...HEAD --stat` lists the files, and `pr_draft.md`'s Summary and
Changes sections carry the claims.

**What good looks like.** Every hunk edits a file the plan names and
does work a committed change calls for, and every committed change has
a hunk. calib-01: plan "Files: `themes.gitconfig`", diff one deleted
line in `themes.gitconfig`. pkg-02: plan names `rich/console.py`,
`tests/test_console.py`, `CHANGELOG.md`, and the diff touches exactly
those three. Drift in the more-than-plan direction is a hunk the plan
never asked for: pkg-03's `+ pub resolve_symlinks: bool` in
`src/configs/directory.rs` when the plan said "Not in scope: ... any
config surface change", pkg-06's "member/event renaming pass" and
`experimental.menuFollowsWindowTheme` setting, pkg-09's
`--time-format` flag, calib-03's second hunk rewriting
`print/property.js`, a file the plan never names. Drift in the
less-than-plan direction is a planned deliverable with no hunk and no
deferral note: pkg-17's plan names "(2) update the reactivity-core
documentation page" and the diff has no docs file. The description's
claim makes drift worse, not better: pkg-03 "Implements the posted
plan exactly", pkg-06 "No functional changes outside the system menu
fix", pkg-09 "exactly as planned", calib-03 "with no changes beyond
it", pkg-17 "documents the limitation" all sit over diffs that
contradict them. An honest deviation re-ties the mismatch: pkg-13's
plan says `%VAR%` is not covered and its description says "percent-sign
environment expansion (`%VAR%`) is NOT handled here"; pkg-16's plan
defers the start path and its description says "Deliberately out of
scope (recorded in the plan ...)". A deferral that exists only in the
description, or only in the diff, is not recorded. Live, the same
test: each file in `--stat` against `plan.md`'s "Files", each hunk
against "Approach" plus `## Deviations`.

## Test evidence (harness category: not-tested)

**Where it lives.** The proof is in `### Test evidence`, read against
two things from `## Plan context`: the "Test plan:" sentence (which
names the cases: "re-run the repro ... expect ...", the control, the
suite command) and the "Repro evidence" paragraph (which names the
trigger: the input or configuration that produces the reported
behavior, and the control that does not). The repo's own checks are
named in the `pull requests` line of `## Repo facts` ("tests added and
passed", "pass cargo test", "yarn test") and in the test plan. Live:
`test_evidence.md` read against `plan.md`'s "Test plan" and the
student's posted repro comment; the repo's checks are the PR
template's Testing checklist (`make test-unit`, `make
test-integration`, `make lint`, `make typecheck`) and
`docs/CONTRIBUTING.md`'s "CI must be green" section.

**What good looks like.** A command shown with its result on both
sides, where the command is the trigger. calib-01: the `configparser`
one-liner with "before: DuplicateOptionError ... after: parsed OK".
pkg-05: the issue's three-line script with the one-row table on
0.115.0 and the two-row table on the branch. pkg-16: `minikube image
load ./notanimage.tar` with "before: OK, exit 0" and "after: X Failed
to load image ... exit 1", then the valid-tarball control. pkg-19:
`diff <(./yq . source.yml) source.yml && echo IDENTICAL`. Decisive
evidence names the observable: a string, a status, an exit code, a
count, a rendered state. Not decisive: a bare assertion with no
command (pkg-04 "Tested locally on Windows 11 with less-Windows:
colors work now in paging mode. `cargo test` passes"; pkg-10 "a full
day of WebDAV syncs ... with zero ETIMEDOUT errors"); a run of the
control instead of the trigger (pkg-07 runs `./onefile/`, the path the
plan says "applies cleanly", and never the two-file repro; pkg-14 runs
a GET with one custom header checking casing, while the trigger is the
single-header POST whose `Content-Type` goes missing); a test plan
case the evidence never mentions (calib-04's plan names "both failure
modes by name: re-run repro 1 ... re-run repro 2 (`-j 200000 -x echo
.`)" and the evidence shows repro 1 only, with no word on repro 2). A
secondary case stated with its outcome counts as accounted for even
without a transcript: pkg-19's "Controls (double-quoted twin;
single-quoted without the trailing blank line) unchanged", pkg-05's
"Same-key redefine prints the one-time warning". The primary case
must be captured, not stated; that is the before-and-after test
above. The repo's
checks count when shown with a result: "`cargo test -p nu-protocol`
passes (312 tests); fmt and clippy clean" (pkg-05), "`go test ./...`
passes; `go generate ./...` produces no cheatsheet diff" (pkg-08). An
honestly reported failing check with its reason ("`make
test-integration` stops with 'no tests ran'") is a shown result. Live:
the before side is the repro comment's output or a re-run on `main`,
the after side is a run on the committed branch (its SHA quoted), and
the four `make` targets appear with their output pasted.

## Diff quality (harness category: unreviewable)

**Where it lives.** The change itself is `### Diff`, hunk by hunk, and
`### Commits`, one subject per line. Live: `git diff main...HEAD` and
`git log main..HEAD --oneline`.

**What good looks like.** Every hunk performs part of the planned
change and every added line is meant to ship. calib-01: one deleted
line. pkg-11: one function in one file plus its tests. pkg-19: the
emitter change plus one regression test. Debris is anything a reviewer
would have to ask the author to remove: a debug print (pkg-15's
`eprintln!("DEBUG accept err = {:?}", err)`, pkg-12's `//
eprintln!("DBG buffer_size = {}", buffer_size)`, pkg-18's `//
eprintln!("help colors: tty=...")`), commented-out code kept "for
reference" (pkg-12's first attempt, pkg-15's `// log::debug!` and `//
std::process::Command::new("lsof")` lines), a dead function (pkg-12's
`fn _unused_buffer_probe`, pkg-15's `#[allow(dead_code)] fn
count_open_fds`), a leftover note (pkg-12's `// TODO: ring buffer?` on
an import), and churn: a hunk that only re-indents a block (pkg-12's
`while reader.read_line` loop, deleted and re-added identical), only
reorders imports (pkg-15's `use std::sync` split, pkg-18's six-line
import restructure), only re-prints identical lines (pkg-18's two
`println!` blocks), or only reformats a struct literal (pkg-15's
`ClientAttributes { size, style }`), or an edit the plan does not need
(calib-03's `print/property.js` rewrite, pkg-06's renaming pass). A
formatting change on a line the fix itself edits is part of the fix.
Commit subjects are feedback: "wip", "fix", "fmt + cleanup" (pkg-12),
"misc cleanups while debugging" (pkg-15), "fixed dead keys
(hopefully)" (calib-02) tell a reviewer the author did not read their
own diff; they do not by themselves hold the PR. Live: read every hunk
of `git diff main...HEAD` the same way; for a Markdown-only change the
tells are stray blank-line churn, edits to sections the plan left
alone, and text left from an earlier draft.

## Standards and comms (harness category: standards-wall)

**Where it lives.** The repo's asks are the two lines of `## Repo
facts`: `pull requests` (the template's sections and checklist items,
the issue-reference form, the named checks, or "no PR template") and
`contribution policy` (the AI rule, or "no stated AI policy"). The
PR's answer is `### Description`: its section labels ("Summary of the
Pull Request:", "Description:", "User-facing changes:", "PR
Checklist:", "Type of changes:", "Requirements:"), its issue reference
("closes #20507", "fixes #23471", "(#2819)"), and its disclosure
sentence. Maintainer direction in `## Thread highlights` is context
for reading the plan's boundary (pkg-06's plan follows DHowett's "no
private APIs"); it is not a separate check this week. Live:
`.github/PULL_REQUEST_TEMPLATE.md` (Summary, Issue with `Closes #`,
Changes, Testing checklist, Screenshots / Demo, Notes for Reviewers),
`docs/CONTRIBUTING.md` ("Fill out the PR template completely", "CI
must be green", branch and commit conventions), and `pr_draft.md`.
Path Review's CONTRIBUTING and template state no AI-use rule; the
course asks for the disclosure under Notes for Reviewers, so in live
mode its absence is reported from the voice guide, not failed by the
policy row.

**What good looks like.** Each ask is answered with content specific
to this change. pkg-05 under nushell's template: "Description: since
0.115 ..." and "User-facing changes: same-name keybindings ... no
longer dropped". pkg-08 under lazygit's checklist: "Requirements:
cheatsheets regenerated (no binding changes, no diff); code formatted;
integration test added; new error text internationalised; no
UserConfig changes". pkg-02 under rich's template: "Type of changes:
Bug fix" and "CHANGELOG.md entry added", with `CHANGELOG.md` in the
diff. pkg-16 under minikube's hint: "fixes #23471" as the first line.
The wall is a required ask visibly ignored: pkg-01 under pandas'
"required checklist: closes #xxxx ..., tests added and passed, all
code checks passed, ... an entry in the latest
doc/source/whatsnew/vX.X.X.rst" writes a free-form description with no
`closes #66657`, no checklist, and no whatsnew file in the diff.
calib-02 under lazygit's checklist has no checklist at all. On
disclosure: pkg-11 under ripgrep's "comments to maintainers must be
written by humans in their own words" writes "this change was
AI-assisted with me reviewing and understanding every line, and this
text is in my own words" and passes; pkg-20 under ghostty's "All AI
usage in any form must be disclosed, stating the tool used and the
extent of the assistance" says nothing about AI and fails. A repo with
no template and no AI rule (pkg-19, pkg-14) asks nothing here, and
silence passes. Every package is AI-assisted work, so silence under a
disclosure rule is the failure, however good the fix. Whether the
description's content is true of the diff is plan fidelity, above.
