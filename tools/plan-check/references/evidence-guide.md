# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

An eval bundle has six sections in a fixed order: `## Repo facts`,
`## Issue`, `## Thread highlights`, `## Repro evidence`,
`## Candidate plan`, `## Candidate plan comment`. The first four are
the issue side; the last two are the candidate side. In live mode the
issue side is the GitHub issue page, the student's own posted repro
comment on it, and the repo's `CONTRIBUTING.md`, `README.md`, and any
AI policy file; the candidate side is `plan.md` and the draft comment
file. The candidate plan's layout varies: some are four short
paragraphs labeled inline ("Cause:", "Change:", "Test:"), some use
`###` subheadings ("Diagnosis", "Scope", "Files", "Approach", "Test
plan"). Read for the sentence that does the job, not for the heading.

## Diagnosis and grounding

<!-- Where the plan states its cause, and where the repro evidence
pins down the behavior that cause must explain. What it means for a
diagnosis to follow from the evidence rather than contradict or
ignore it. -->

**Where it lives.** In the bundle: the plan's "Diagnosis:", "Cause:",
or "Problem statement" sentences in `## Candidate plan`, and the first
sentence of `## Candidate plan comment`, which usually restates the
cause. The behavior that cause must explain is in `## Repro evidence`:
the numbered steps, the "Control run" or "Control:" lines, and the
Expected and Actual sentences. A maintainer's own cause statement, when
there is one, is in `## Thread highlights` ("This seems to be the
culprit"). Live: the cause sentences of `plan.md`; the student's posted
repro comment for the steps and controls; maintainer comments on the
thread.

**What good looks like.** The cause explains every run the repro
shows, including the controls. In calib-01 the cause "that view's model
is not refreshed" explains step 3 (stale color after push) and step 4
(fresh color after leaving and re-entering the view). pkg-02 says it
outright: "Both controls in the repro fit: no background, no
subtraction; width 2, no overshoot." A control that shows the
mechanism already working rules the cause out, however confident the
plan sounds: pkg-01's control parses the same request items without
the `-v` flag, so the plan's "tokenizer is too strict" cannot be the
cause; pkg-07's control prints the friendly error in the same build the
plan says lost the friendly error system. A plan that dismisses the
repro's own evidence as "a red herring" without a run that shows why
is contradicting the repro, not grounding itself in it. For a
documentation issue, the cause is the missing or wrong text and the
repro's greps or quoted doc lines are what pin it.

## Scope

<!-- Where the plan bounds itself: the in-scope statement, the
not-in-scope line, the files or areas named. What one bounded change
looks like next to a drive-by rewrite. -->

**Where it lives.** In the bundle: "In scope", "Not in scope", "Out:",
"deferred", "Files", and the numbered change list in
`## Candidate plan`; the plan sentence of `## Candidate plan comment`
("I plan to ..."). The behavior the changes must be measured against
is the `## Issue` title and body, and the Actual line of
`## Repro evidence`. Live: the same parts of `plan.md` and the draft
comment, against the issue body.

**What good looks like.** One change that removes the reproduced
behavior, plus its test, with everything else absent or named as
deferred with a reason. calib-01: "Out: any change to how push status
is computed, or to other views' refresh behavior." pkg-09 defers the
maintainers' option 1 as "a larger rework the maintainers may prefer
long term" and still passes, because doing less with the deferral
stated is bounded. A rewrite announces itself with additions the issue
never asked for: pkg-06's "Upgrade the bundled containerd ... since we
are on an old patch series anyway", pkg-15's "While touching the
network stack: replace `node-fetch` with `undici`", pkg-19's "Add a
`persistClass` prop" and "Migrate the runtime-dom Transition tests".
Naming a suspicious adjacent site so the PR reviewer can look at it,
without changing it, is not an extra.

## Executability

<!-- Where the plan says what will actually be done: files or areas,
approach, order of work. What it means for a stranger to be able to
start executing without asking the author anything. -->

**Where it lives.** In the bundle: "Files", "Approach", "Changes",
"Steps", or the "Change:" paragraph in `## Candidate plan`. Live: the
same sections of `plan.md`.

**What good looks like.** A named landing site and one chosen
approach, so a reader with only the repository could open the right
file and begin. calib-01: "the push completion callback in
`pkg/gui/controllers/sync_controller.go`". pkg-14 names the component
("`zellij-server`'s client connection handling") and how it will pin
the exact function ("`zellij --debug` output"), which is enough even
without a function name. Not enough: pkg-17's "gocui? tcell? not sure
which layer is responsible", pkg-18's "a recover() safety net
somewhere" and "upstream or in the vendored copies, whichever turns out
to be easier", pkg-10's "Optimize whatever the profiling turns up".
The test is whether the first hour of work is decided, not whether
every line is.

## Test plan

<!-- Where the plan says how success will be observed, and how that
maps onto the repro evidence's steps and artifacts. What a decisive
test plan names that a vague one does not. -->

**Where it lives.** In the bundle: "Test plan:", "Test:", or
"Verification" sentences in `## Candidate plan`, read beside the
numbered steps and the artifacts (output blocks, counts, exit codes,
rendered states) in `## Repro evidence`. Live: the test plan in
`plan.md` beside the student's posted repro comment.

**What good looks like.** A repro step re-run, or an automated test
built from those steps, with the result named. calib-01: "at step 3 the
color must flip without leaving the view." pkg-05: "backdated by 25
hours: step 4 must fetch and print A and B (access log shows the second
request)." pkg-02: "re-run the repro command, expect drawn output and
exit 0." Not enough: calib-04's "Run the full test suite ... make sure
nothing regresses" (a suite is not an observable for this fix),
pkg-10's "should feel fast", pkg-17's "nothing else should feel
broken". A decisive test plan names what the repro currently fails to
show and says it must show it afterward: a string, a count, an exit
code, a status, a rendered state, a timing bound.

## Honesty

<!-- Where claims meet uncertainty: risks, unknowns, and deviations.
How to tell stated unknowns from false confidence, and where an
honest mid-build deviation gets recorded. -->

**Where it lives.** In the bundle: "Risk:", "Risks", "Unknowns", "open
question", "not yet", "to be pinned" sentences in `## Candidate plan`;
certainty words anywhere in the plan or `## Candidate plan comment`
("root cause is", "clearly", "simply", "the tell", "confirmed", "red
herring"), read against `## Repro evidence`, `## Thread highlights`,
and `## Issue`. Live: the risks section of `plan.md`, the
`## Deviations` section appended after the build, and the draft
comment.

**What good looks like.** Unverified things are labeled. pkg-13: "I
have not yet verified which layer clamps the viewport ... (named as an
unknown, not a certainty)"; pkg-20: "I have not yet measured the
per-print cost". False confidence is a settled claim with nothing under
it: pkg-07's "The TypeError is the tell: the function is simply absent"
when the control run shows the function present, pkg-16's "Width can be
recovered" when the repro shows the zeros already gone before the
cast. calib-01 has no risk section and is still honest: a terse plan
with nothing unverified owes no caveats. A deviation is honest when
`plan.md` says what changed from the posted plan and why, in the
author's words; a deviation that exists only in the diff is not
recorded.

## Comms

<!-- Where the words meet the thread and the repo: the plan comment
read against the issue's maintainer signals (thread highlights, or
the live thread) and against the repo-facts block's stated templates,
contributing asks, and contribution policy (including AI-use
disclosure requirements). What thread-aware looks like next to
boilerplate. -->

**Where it lives.** In the bundle: `## Candidate plan comment` read
against the entries in `## Thread highlights` from authors tagged
OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR (the direction), and
against the "contribution policy" and "bug reports" lines in
`## Repo facts` (the rules). Live: the draft comment against the issue
thread (each comment carries its author's association badge; other
students' plans and open questions are context), the repo's
`CONTRIBUTING.md`, any AI policy file or section, and the issue and PR
templates under `.github/`. For the Path Review repo,
`docs/CONTRIBUTING.md` and `.github/PULL_REQUEST_TEMPLATE.md` are the
policy sources, and neither states an AI-use rule.

**What good looks like.** Direction is adopted or answered. calib-01
engages the only signal present: "keeping it minimal given the
review-bandwidth note in CONTRIBUTING". pkg-09: "I would like to
implement option 2 from the discussion here". pkg-04 is the miss: the
owner located the culprit in `src/tui/light_windows.go` and posted a
test binary, and the comment plans documentation without a word about
either. Entries from authors tagged NONE (other reporters, other
students) are not direction, though a thread-aware comment on a shared
issue says how its plan relates to them. On disclosure: pkg-03 under
ripgrep's rule writes "the fix will be AI-assisted ... this comment is
in my own words" and passes; pkg-20 under ghostty's "All AI usage in
any form must be disclosed" says nothing and fails. A PR-only or silent
policy requires nothing, and every package is AI-assisted work, so
silence under a disclosure rule is the failure. On dates: commit to
the work, not a day. "Report-back once the test is in" is right; "I can
have the docs PR up this week" is the shape to avoid.
