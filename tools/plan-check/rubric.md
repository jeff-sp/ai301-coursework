# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

Each row tests one thing. Rows 1 and 2 read the plan's cause and
change against the repro evidence; row 3 reads every committed change
against the issue; rows 4 and 5 read the approach and the test plan
the way a stranger would; row 6 reads confidence words against what
the package establishes; rows 7 to 9 read the plan comment against the
thread and the repo's policy. Rows 1, 3, 4 and 5 grew out of my
group's worksheet rows (diagnosis, scope, candidate plan, test); the
rest were added because the worksheet had no row for symptom-versus-
cause, for honesty, or for the thread and the repo's conventions.
Every row judges the thing itself, never the write-up's length or
headings: a four-paragraph plan with no section titles can pass every
row.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Cause follows the repro | The plan's stated cause (its "Diagnosis", "Cause", or "Problem" sentences in `## Candidate plan`) read against every numbered step, artifact, control run, and the Expected and Actual lines in `## Repro evidence` | Pass when the plan says what causes the reported behavior and that cause fits every run the repro shows: the failing steps fail for the reason given, and each control run behaves the way the cause predicts. Fail when any step or control run shows the cause's mechanism already working or already absent (the same input parses once the blamed flag is removed; an instance method prints the friendly error the plan says was stripped from the build; the top-level call already yields `[null]`; the zeros are already gone before the cast the plan blames), or shows the failure where the cause says it cannot occur. Fail also when the plan calls part of the repro evidence a red herring without showing why. Unclear when the plan names no cause and only restates the symptom ("it is slow", "something changed", "should never crash") | required |
| Change acts on the cause | The change the plan commits to, read against the mechanism its own cause and `## Repro evidence` identify, and against any culprit a maintainer located in `## Thread highlights` | Pass when the change alters that mechanism at the point the evidence pins (a clamp at the underflowing subtraction, a separator before the file path, the refresh the callback misses). For a documentation issue the mechanism is the missing or wrong text, and a change to that text passes. Fail when the change leaves the mechanism in place: it documents a workaround for behavior the evidence or a maintainer has located in code, wraps the failure in a recover, catch, or retry, or rewrites the output after the damage is done (re-padding digits the parser already dropped). Unclear when no concrete change is described | required |
| One bounded change | Every change the plan commits to (numbered changes, "In scope", "Files", "Approach", and the plan sentence of `## Candidate plan comment`) read against the behavior `## Issue` reports and `## Repro evidence` reproduces | Pass when each committed change is needed to remove the reproduced behavior, or is a test, doc line, or cross-reference for that change. Doing less than the issue asks passes when the deferral is stated ("Not in scope: option 1", "I cannot test the Windows variant, so I am leaving it out"); naming adjacent suspicious sites for the PR without changing them passes. Fail when any committed change is work the issue did not ask for: a dependency migration or version bump, a new option, prop, or setting, a module restructure, moving other constructs onto a new mechanism, a CI matrix, a retry framework, or anything introduced with "while in there", "while touching", "since we are on an old series anyway", or "might as well" | required |
| Stranger could start | The plan's "Files", "Approach", "Changes", or "Steps" in `## Candidate plan`, read as someone who has only the repository and this package | Pass when the plan names at least one concrete landing site (a file path, function, handler, route, or named component such as "the client connection handling in `zellij-server`") and commits to one approach, so the reader could open the repository and begin without asking which layer or which approach. A function left to be pinned passes when the component and approach are fixed and the plan says how it will pin it. Fail when the site is "somewhere" or a question ("gocui? tcell? not sure which"), or when the central decision is deferred to build time ("whichever turns out to be easier", "optimize whatever the profiling turns up", "poke around"). Unclear when the plan names no files or areas at all | required |
| Test names the observable | The plan's test plan ("Test plan", "Test", or "Verification" sentences in `## Candidate plan`) read against the steps and artifacts in `## Repro evidence` | Pass when the test plan names a re-run of a repro step or command, or an automated test built from those steps, together with the specific result that must appear afterward: an output string, exit code, rendered state, count, timing bound, or assertion the repro currently fails to show. An automated test that follows the repro steps is the preferred form, and a manual re-run with the result named also passes. Fail when the test plan names only a suite ("run the full test suite", "make sure nothing regresses") or a feeling ("should feel fast", "nothing else should feel broken", "undo works") with no repro-tied observable. Unclear when there is no test plan | required |
| Unknowns stated as unknowns | Certainty words in `## Candidate plan` and `## Candidate plan comment` ("root cause is", "clearly", "simply", "the tell", "confirmed", "red herring") read against what `## Repro evidence`, `## Thread highlights`, and `## Issue` establish | Pass when each claim stated as settled is backed by a repro artifact, a maintainer statement, or the issue's own analysis, and anything unverified is labeled as such ("open question", "not yet measured", "to be pinned during the build"). A terse plan with nothing unverified owes no caveats. Fail when a claim the package does not establish is stated as settled, or when a dependency the approach rests on is asserted without being checked | preferred |
| Comment follows thread direction | Entries in `## Thread highlights` from authors tagged OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR that give direction (locate the culprit, propose or reject an approach, post a patch or test binary, ask for testing or information, or call the behavior intended), read against `## Candidate plan comment` and the plan's scope | Pass when no such direction exists ("(no comments)", or only entries from authors tagged NONE), or when the comment adopts the direction, or names it and gives a reason for departing from it. Fail when a direction exists and the comment neither follows nor mentions it, or when the plan does what a maintainer rejected, or documents around a culprit a maintainer located in code. Entries from authors tagged NONE are context, not direction | required |
| AI disclosure per repo policy | The "contribution policy" line in `## Repo facts` read against `## Candidate plan comment` | Pass when the policy does not require disclosing AI assistance in issue comments (no stated policy, a policy about code or pull requests only, one that states no disclosure ask for comments, or one asking only that comments be in the contributor's own words). Also pass when the policy requires disclosure and the comment names the AI tool used, or that AI was used, and the extent of its help. Fail when the policy requires disclosing AI use in comments or "all AI usage in any form" and the comment is silent. Every package is treated as AI-assisted work, so silence under a disclosure rule is a fail | required |
| Comment promises investigation, not a date | The commitment sentences of `## Candidate plan comment` | Pass when the comment commits only to steps within the author's control (implement the change, run the test plan, report back, open a PR when the tests pass) and names no calendar deadline. Fail when it gives a deadline or window ("this week", "by Friday", "within 2 days") or guarantees the outcome | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
Ready (accept) if every required check passes; otherwise hold
(reject). The two preferred checks (Unknowns stated as unknowns,
Comment promises investigation, not a date) never change the verdict;
they appear in the output as feedback for the author. Unclear (`?`)
counts as fail on every required row: a cause, change, landing site,
or test that cannot be located in the package is one a stranger cannot
build from. Live mode applies the same rows and the same rule; the
only differences are where the evidence is read (the live locations in
procedure.md and the evidence guide) and that the repro evidence is
what the drafts quote from the student's posted repro comment. When
the drafts quote no repro evidence at all, Cause follows the repro and
Test names the observable are graded unclear, which holds the package.
A deviation recorded in `plan.md` after the build is graded as part of
the plan, under One bounded change and Unknowns stated as unknowns.
