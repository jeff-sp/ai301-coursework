# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

An eval bundle has six sections in a fixed order: `## Repo facts`,
`## Issue`, `## Thread highlights`, `## Repro evidence`,
`## Candidate plan`, `## Candidate plan comment`. The first four are
the issue side; the last two are the candidate side. Live, the issue
side is the GitHub issue page (title, body, every comment with its
author's association badge), the student's own posted repro comment on
that issue, and the repo's `CONTRIBUTING.md`, `README.md`, and any AI
policy file; the candidate side is `plan.md` and the draft comment
file. Every step below names the bundle location first and the live
location in parentheses.

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides the order (issue first? repro evidence first?) and says why
the order matters for the checks that come later. -->

The issue side is read before the candidate side, and the repro
evidence is read before the plan, so that the plan's cause is tested
against the evidence instead of the evidence being read through the
plan. Nothing is graded until step 6 is done.

1. Read `## Issue` (live: the issue title and body). Write one line:
   the behavior reported (error text, exit code, output shape, what is
   missing) and what the reporter asks for. Every later comparison is
   made against this line.
2. Read `## Thread highlights` (live: every comment on the issue, in
   date order, noting the association badge on each). List every entry
   from an author tagged OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR,
   and mark each one "direction" or "context". Direction means the
   entry locates the culprit, proposes or rejects an approach, posts a
   patch or test binary, asks for testing or information, or calls the
   behavior intended. Entries from authors tagged NONE are context
   only, but note any open question they raise. If the section says
   "(no comments)", write "no direction".
3. Read `## Repro evidence` (live: the student's posted repro comment
   on the issue; on a house issue, the repro pack as quoted in the
   drafts). List each numbered step with its outcome. List every
   control run separately, with the one fact it shows (what works
   without the trigger, what the control rules out). Copy the Expected
   and Actual lines. Mark which steps are the trigger. If the drafts
   quote no repro evidence in live mode, write "repro evidence absent".
4. Read `## Repo facts` (live: `CONTRIBUTING.md`, any `AI_POLICY.md`
   or AI section, `README.md`, and the issue and PR templates under
   `.github/`). Copy the "contribution policy" line verbatim and mark
   it one of: comments-disclosure-required, PR-or-code-only,
   own-words-only, silent. Note any contributing ask that bears on a
   plan (review bandwidth, test requirements, branch naming).
5. Read `## Candidate plan` (live: `plan.md`). Extract, each as a
   quote: the cause sentences; every committed change (numbered items,
   "In scope", "Files", "Approach"); every deferral ("Not in scope",
   "deferred", "leaving out"); every landing site named (file path,
   function, handler, route, component); the test plan sentences; the
   risk or unknown sentences; any deviation note.
6. Read `## Candidate plan comment` (live: the draft comment file)
   last. Extract: what it says it will do; every reference to a thread
   entry, maintainer, or other plan on the thread; any AI disclosure;
   any date or deadline; the sentence that states what is out of
   scope, if any.

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which page or
thread location per your evidence guide) to pull the fact from, and
what to record. A complete procedure leaves no check whose evidence an
executor would have to hunt for. -->

Each move below produces the note that one rubric row is graded from.
Record quotes, not summaries. `references/evidence-guide.md` says what
good looks like for each family.

7. Cause (for Cause follows the repro): pair the plan's cause from
   step 5 with each step and each control run from step 3. For each
   pair write "consistent" or "rules out", followed by the step's or
   control's own words. Write "no cause stated" if step 5 found none.
8. Change versus mechanism (for Change acts on the cause): write the
   one sentence the plan gives as its change, and the mechanism the
   repro evidence or a maintainer direction pins. Mark the change
   "code at the mechanism", "text at the mechanism" (docs issues), or
   "workaround: docs / recover / retry / reformat".
9. Scope (for One bounded change): for each committed change from
   step 5 write "needed for the reproduced behavior", "test or doc for
   it", or "extra: <quote the words that introduce it>". Write each
   deferral on its own line; deferrals are never extras.
10. Executability (for Stranger could start): copy every landing site
    and the approach sentence; copy any hedge ("not sure which",
    "whichever is easier", "whatever turns up", "somewhere"). Note
    whether a site left to be pinned comes with the method of pinning
    it.
11. Test (for Test names the observable): copy the test plan; next to
    it write the repro step or command it re-runs and the result it
    names (string, exit code, state, count, bound, assertion). Write
    "no repro step" or "no result" where either is missing, and "no
    test plan" if none exists.
12. Honesty (for Unknowns stated as unknowns): copy each certainty
    phrase from the plan and comment and write what in the package
    backs it (a step, a control, a maintainer entry, the issue body)
    or "unbacked". Copy each labeled unknown.
13. Thread (for Comment follows thread direction): for each
    "direction" entry from step 2 write "adopted", "named and departed
    with reason", or "unmentioned", quoting the comment's words or
    noting their absence. If step 2 wrote "no direction", write that.
14. Policy (for AI disclosure per repo policy): write the policy mark
    from step 4 and quote any disclosure sentence in the comment, or
    "none".
15. Date (for Comment promises investigation, not a date): quote any
    deadline, window, or guarantee in the comment, or "none".
16. When a section is empty or its live equivalent does not exist,
    write "absent: <section>" for that family. Absence is itself the
    evidence for the rows that read it; never fill the gap from the
    plan's own claims about what the evidence shows.

## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

Grade each check pass (P), fail (F), or unclear (?), in rubric order,
from the notes of steps 7 to 16 only. Re-read the package only to
settle an ambiguous note, and then only the quoted passage. Each
grade's evidence line is one quote from the notes.

17. Cause follows the repro: fail on the first pair marked "rules
    out"; unclear when step 7 says "no cause stated" or "repro
    evidence absent"; otherwise pass.
18. Change acts on the cause: fail when step 8 marks the change a
    workaround; unclear when no change was extracted; otherwise pass.
19. One bounded change: fail on the first "extra"; deferrals and sites
    named for the PR without being changed are not extras; otherwise
    pass.
20. Stranger could start: fail when no landing site is named or a
    hedge stands where the approach decision should be; a site left to
    be pinned passes only when step 10 found the method of pinning it;
    unclear when the plan names no files or areas at all; otherwise
    pass.
21. Test names the observable: fail when step 11 wrote "no repro step"
    or "no result"; unclear on "no test plan" or "repro evidence
    absent"; otherwise pass.
22. Unknowns stated as unknowns: fail on any "unbacked" certainty
    phrase; otherwise pass. Preferred: record the grade, it does not
    enter the verdict.
23. Comment follows thread direction: pass when step 13 says "no
    direction"; fail on any "unmentioned" direction or when the plan
    does what a maintainer rejected; otherwise pass.
24. AI disclosure per repo policy: fail only when the policy mark is
    comments-disclosure-required and step 14 says "none"; otherwise
    pass.
25. Comment promises investigation, not a date: fail on any quoted
    deadline or guarantee; otherwise pass. Preferred: record only.
26. Absent evidence: a required row whose family is "absent" and whose
    pass condition needs that evidence (cause, change, test) is graded
    unclear. A row whose pass condition is satisfied by absence (no
    maintainer direction, no policy) is graded pass. Two executors with
    the same notes must reach the same grade; if a note could support
    either grade, the rubric's Fail clause wins, because the burden is
    on the plan to show it.

## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check. A
complete procedure produces the same verdict from the same grades,
every time. -->

27. Apply the verdict rule from `rubric.md`: count the required rows.
    If every required row is pass, the verdict is accept (ready). If
    any required row is fail or unclear, the verdict is reject (hold).
    Preferred rows do not enter the count.
28. The deciding check is the first required row in rubric order that
    failed or was unclear. On an accept, the deciding check is the row
    whose evidence most directly ties the plan to the repro (Cause
    follows the repro). Its `evidence` field quotes the exact package
    phrase that decided it, in one line: the control run's words for
    cause, the extra change's words for scope, the hedge for
    executability, the test sentence for test, the maintainer entry
    for thread direction, the policy line for disclosure.
29. Every other check's `evidence` field carries one quoted phrase
    from its note, or "absent: <section>".
30. Live mode only: hold the draft comment against `voice-guide.md`
    and list each rule it breaks in the readable summary, quoting the
    rule; this never changes the verdict. Also list any step above the
    package made impossible to follow, as a procedure gap.
31. Write the readable summary (one line per check), then the fenced
    JSON block exactly as SKILL.md specifies, with nothing after it.
