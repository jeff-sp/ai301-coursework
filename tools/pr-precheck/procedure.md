# Procedure: how this tool grades a PR package

An eval bundle has five sections in a fixed order: `## Repo facts`,
`## Issue`, `## Thread highlights`, `## Plan context` (the accepted
plan with the repro evidence it built on), and `## Candidate PR`,
which holds `### Title`, `### Description`, `### Commits`, `### Diff`,
and `### Test evidence`. The first four sections are the issue side
and the plan; `## Candidate PR` is the candidate side. Live, the issue
side is the GitHub issue page, `.github/PULL_REQUEST_TEMPLATE.md`, and
`docs/CONTRIBUTING.md` (plus any AI policy they point to); the plan is
`plan.md` with its `## Deviations`; the candidate side is
`git diff main...HEAD` (with `--stat` and `git log main..HEAD
--oneline`), `pr_draft.md`, and `test_evidence.md`. Every step below
names the bundle location first and the live location in parentheses.

## Read order

The plan is read before the diff, and the diff before the description,
so that the diff is tested against the plan rather than through the
description's account of it, and the description's claims are tested
against hunks already listed. The test evidence is read after the plan
so the test plan's named cases are in hand. Nothing is graded until
step 8 is done.

1. Read `## Repo facts` (live: `.github/PULL_REQUEST_TEMPLATE.md`,
   `docs/CONTRIBUTING.md`, any AI policy file). Copy the `pull
   requests` line verbatim and split it into a list of asks: each
   template section, each checklist item, the issue-reference form
   ("closes #xxxx", "fixes #"), and each named check or suite command.
   Mark the list "no template" if it says so. Copy the `contribution
   policy` line verbatim and mark it one of: disclosure-required
   (AI use must be disclosed or stated), no-disclosure-ask (own-words
   rule only, PR-or-code-only rule, or no AI statement).
2. Read `## Issue` (live: issue title and body). Write one line: the
   behavior reported and the input or action that produces it.
3. Read `## Thread highlights` (live: every comment with its author
   badge). Note any maintainer (OWNER, MEMBER, COLLABORATOR,
   CONTRIBUTOR) direction that the plan says it follows; it is context
   for reading the plan's boundary, not a check of its own.
4. Read `## Plan context` (live: `plan.md`, every section, then
   `## Deviations`). Extract, each as a quote:
   - the repro evidence's trigger (the step, input, or configuration
     that produces the reported behavior) and each control run (what
     works without the trigger);
   - every committed change (numbered changes, "Scope: one bounded
     change" sentence, named deliverables);
   - the "Files" list;
   - every "Not in scope" exclusion;
   - every deferral or known limit ("deliberately deferred", "NOT
     covered", a `## Deviations` entry that leaves something out);
   - every deviation that adds or alters something;
   - the test plan, split into named cases (each "re-run X, expect Y",
     each control, each suite command).
5. Read `### Diff` (live: `git diff main...HEAD`, after confirming
   `git status --short` shows no modified tracked files; if it does,
   note "uncommitted changes present" as a summary line). List every
   file and, under it, every hunk with a three-word label of what it
   does. Tag each hunk: `planned` (a committed change or its test or
   changelog entry), `deviation` (named by a deviation note), `extra`
   (neither), `churn` (only reformats, re-indents, reorders imports,
   or re-prints identical lines), and separately mark each added line
   that is a debugging leftover (debug print, commented-out code,
   unused or dead function or import, TODO or "keeping for reference"
   note). Write "absent: diff" if there is none.
6. Read `### Commits` (live: `git log main..HEAD --oneline`). Copy each
   subject line.
7. Read `### Test evidence` (live: `test_evidence.md`). List every
   captured run: the command or action, the side (before, after,
   both), and the result shown. Separately list every bare assertion
   with no command ("tested locally", "all tests pass", "verified
   working"). List every check or suite named with its result. Write
   "absent: test evidence" if the section says "(none provided)" or
   does not exist.
8. Read `### Title` and `### Description` last (live: line 1 of
   `pr_draft.md`, then the rest). Copy the title. From the description
   extract, each as a quote: every sentence stating what the change
   does, does not do, or contains; every section heading or label it
   carries; the issue reference, in its exact form; any disclosure of
   AI use; any sentence restating a deferral.

## Evidence gathering

Each move below produces the note one rubric row is graded from.
Record quotes, not summaries. `references/evidence-guide.md` says what
good looks like for each family.

9. Boundary (for Diff adds nothing outside the plan): take the step-5
   hunk list. For every hunk tagged `extra`, write the file name and
   the hunk's own words (the option, flag, setting, rename, or rewrite
   it introduces) and whether any "Not in scope" exclusion from step 4
   names it. Write "all hunks planned or deviation" if there are none.
10. Delivery (for Diff delivers every planned change): for each
    committed change from step 4, write "delivered: <hunk>",
    "deferred: <plan's own words>", or "missing". The plan's deferral
    sentence must come from the plan, not from the description.
11. Claims (for Description claims match the diff): for each claim
    sentence from step 8, write "matches: <hunk>" or "contradicted:
    <hunk or absence>". A "nothing beyond the plan" claim is
    contradicted by any `extra` hunk; a "<deliverable> added" claim is
    contradicted by the absence of a hunk for it.
12. Deferral restated (for Deferred work is restated in the
    description): for each deferral from step 4, quote the description
    sentence that restates it, or write "not restated". Write "no
    deferral in plan" if step 4 found none.
13. Capture (for Before and after captured): from step 7, write the
    captured runs that show the reported behavior or its fix, with
    their side and result. Write "assertions only: <quote>" when step 7
    found only bare assertions. Write "absent" when the section is
    absent.
14. Trigger (for Evidence exercises the trigger path): take the trigger
    and controls from step 4. For each captured run from step 7, mark
    it "trigger", "control", or "unchanged path" by matching its input
    against the trigger's words (the two-file module versus the
    single-file case; the single-header POST versus a GET with a custom
    header; the sealed object versus the extensible one).
15. Cases (for Every planned failure mode exercised): for each named
    case in the test plan from step 4, write "run: <captured run>",
    "stated: <evidence sentence giving its outcome>", "declared not
    run: <reason quoted>", or "not mentioned".
16. Checks (for Repo checks run with outcome shown): for each named
    check from step 1 (and any suite the plan's test plan names), write
    "shown: <command or name> -> <result>" or "not shown". Write "no
    check named" if the repo names none.
17. Leftovers (for No debugging leftovers): copy every added line
    marked as a leftover in step 5, or write "none".
18. Churn (for No unrelated hunks or churn): copy every hunk tagged
    `churn` or `extra` in step 5 with its file, or write "none".
19. Template (for Template sections filled): for each ask in the
    step-1 list that is a section or checklist item, write "filled:
    <description label or sentence>" or "missing". For a checklist
    item naming an artifact (changelog or whatsnew entry, test), add
    "in diff" or "not in diff" from the step-5 file list. Write "no
    template" if step 1 said so.
20. Reference (for Issue referenced as the repo asks): write the asked
    form from step 1 and the description's reference from step 8, or
    "none in description", or "no reference ask".
21. Policy (for AI disclosure per repo policy): write the policy mark
    from step 1 and quote any disclosure sentence from step 8, or
    "none".
22. Commits and title (for the two preferred rows): copy the step-6
    subjects and the step-8 title.
23. When a section is empty or its live equivalent does not exist,
    write "absent: <section>" for that family. Absence is itself the
    evidence for the rows that read it; never fill the gap from the
    description's account of what the diff or the evidence contains.

## Check execution

Grade each check pass (P), fail (F), or unclear (?), in rubric order,
from the notes of steps 9 to 23 only. Re-read the package only to
settle an ambiguous note, and then only the quoted passage. Each
grade's evidence line is one quote from the notes.

24. Diff adds nothing outside the plan: fail on the first `extra` hunk
    in step 9; unclear on "absent: diff"; otherwise pass.
25. Diff delivers every planned change: fail on the first "missing" in
    step 10; unclear when step 4 extracted no committed change;
    otherwise pass.
26. Description claims match the diff: fail on the first
    "contradicted" in step 11; otherwise pass.
27. Deferred work is restated in the description: fail on the first
    "not restated" in step 12; pass on "no deferral in plan";
    otherwise pass.
28. Before and after captured: fail on "assertions only"; unclear on
    "absent"; otherwise pass. The before side may be the repro
    evidence's own result quoted in the evidence ("before: exit 101").
29. Evidence exercises the trigger path: fail when step 14 marked no
    run "trigger"; unclear when there are no captured runs; otherwise
    pass.
30. Every planned failure mode exercised: fail on the first "not
    mentioned" in step 15; "run", "stated", and "declared not run" all
    pass; otherwise pass.
31. Repo checks run with outcome shown: fail on the first "not shown"
    in step 16; pass on "no check named"; otherwise pass.
32. No debugging leftovers: fail when step 17 lists any line; otherwise
    pass.
33. No unrelated hunks or churn: fail when step 18 lists any hunk;
    otherwise pass.
34. Template sections filled: fail on the first "missing" or "not in
    diff" in step 19; pass on "no template"; otherwise pass.
35. Issue referenced as the repo asks: fail on "none in description"
    when an ask exists; pass on "no reference ask"; otherwise pass.
36. AI disclosure per repo policy: fail only when the policy mark is
    disclosure-required and step 21 says "none"; otherwise pass.
37. Commit subjects describe the change, Title names the change:
    grade from step 22 by the rubric's wording. Preferred: record the
    grade, it does not enter the verdict.
38. Absent evidence: a required row whose family is "absent" and whose
    pass condition needs that evidence (the diff for rows 1 and 2, the
    test evidence for rows 5 and 6) is graded unclear. A row whose
    pass condition is satisfied by absence (no template, no reference
    ask, no policy, no deferral, no named check) is graded pass. Two
    executors with the same notes must reach the same grade; if a note
    could support either grade, the rubric's Fail clause wins, because
    the burden is on the PR to show it.

## Verdict assembly

39. Apply the verdict rule from `rubric.md`: count the required rows.
    If every required row is pass, the verdict is accept (ready). If
    any required row is fail or unclear, the verdict is reject (hold).
    Preferred rows do not enter the count.
40. The deciding check is the first required row in rubric order that
    failed or was unclear. On an accept, the deciding check is the row
    whose evidence most directly ties the diff to the plan (Diff adds
    nothing outside the plan). Its `evidence` field quotes the exact
    package phrase that decided it, in one line: the extra hunk's
    words for boundary, the missing deliverable for delivery, the
    contradicted sentence for claims, the bare assertion for capture,
    the control run's words for trigger, the unrun case for cases, the
    leftover line for leftovers, the churn hunk for churn, the missing
    section for template, the policy line for disclosure.
41. Every other check's `evidence` field carries one quoted phrase from
    its note, or "absent: <section>".
42. Live mode only: hold the title and description against
    `voice-guide.md` and list each rule they break in the readable
    summary, quoting the rule; this never changes the verdict. Report
    any scope house rule the package breaks, and the branch's type
    prefix read against `docs/CONTRIBUTING.md`'s type list, as summary
    lines. Also list any step above the package made impossible to
    follow, as a procedure gap.
43. Write the readable summary (one line per check), then the fenced
    JSON block exactly as SKILL.md specifies, one entry per rubric row
    in table order, with nothing after it.
