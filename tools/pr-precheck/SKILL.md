---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

You are grading one PR package to answer one question. Do not answer
from gut feel and do not grade more than one package per run: answer
by executing the student-authored procedure in `procedure.md`, which
applies the checks in `rubric.md` to evidence gathered where
`references/evidence-guide.md` says it lives.

## The question

Is this pull request ready to submit? A PR package is a candidate
pull request (its title, description, commit list, diff, and test
evidence) read against two things: the plan it claims to implement,
including any deviation notes the author recorded, and the issue that
plan belongs to, with the repo's stated asks (PR template, contributing
instructions, AI-use policy). Every check compares a part of the PR
with a part of the plan, the issue, or the repo's asks. You never grade
the plan's quality, the issue's merit, or the code's style on its own;
those were other weeks' questions.

## Inputs and modes

Run in exactly one of two modes. Decide the mode from the invocation:
a package bundle (a markdown file or inlined text with `## Repo facts`,
`## Issue`, `## Thread highlights`, `## Plan context`, and
`## Candidate PR` sections) is eval mode; anything naming the student's
own files, branch, or issue URL is live mode.

**Live mode.** The student's own submission, checked before it goes
out. Read these inputs, each from the place named:

- The plan: `plan.md` in the top folder of the working copy, including
  its `## Deviations` section if present. Deviation notes are part of
  the plan.
- The diff: everything the branch changes relative to the repo's
  default branch. Produce it by running `git diff main...HEAD` (three
  dots) from the working copy, on the student's branch. Also run
  `git diff main...HEAD --stat` for the file list and
  `git log main..HEAD --oneline` for the commit list. Uncommitted
  changes are not part of the diff; say so if `git status --short`
  shows tracked files modified.
- The title and description: `pr_draft.md` next to `plan.md`. Line 1 is
  the PR title; everything under it is the description as it will be
  pasted into GitHub.
- The test evidence: `test_evidence.md` next to `plan.md`.
- The issue: the URL given in the invocation. Read the issue body and
  every comment with its author's association badge (via
  `gh issue view --comments`, the GitHub API, or the web).
- The repo's asks: `.github/PULL_REQUEST_TEMPLATE.md` and
  `docs/CONTRIBUTING.md` (or `CONTRIBUTING.md`) in the working copy,
  plus any AI policy file or section they point to.

A student on the house chain has no plan or repro of their own: read
the house plan where the invocation says it lives and the house repro
pack in place of the student's posted repro comment. The same checks
grade the same things there.

**Eval mode.** The bundle is the whole world. Every fact comes from the
bundle text: `## Plan context` is the plan (with the repro evidence it
built on), `## Candidate PR` holds the title, description, commits,
diff, and test evidence, and `## Repo facts` holds the template asks
and the contribution policy. Fetch nothing and read nothing else. Eval
mode always grades a complete package: every check in the rubric, the
full verdict rule, no shortcuts for a package that looks obviously
ready or obviously not.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before any other
file. It names the one repository a pull request may target this unit
and the house rules of that room. Apply it like this:

- Take the repo from the `Repo:` line. If that line still carries a
  bracketed placeholder (any text in `<...>` or `[...]` instead of an
  `owner/name`), stop without grading and tell the student to fill the
  `Repo:` line in `scope.md` with their section's Path Review repo.
  Never guess a scope and never grade with an unfilled one.
- Refuse to grade a package whose issue URL or branch targets any other
  repository, however tempting. Say which repo the scope allows.
- Read the house rules as context for how evidence is read in that
  environment (one PR per issue, template always used, classmates'
  PRs do not block). Report a house rule the package breaks in the
  summary. The scope's branch example (`fix/<issue-number>-<slug>`) is
  one instance of the repo's `<type>/<issue-number>-<slug>` convention;
  read the type prefix against the type list in `docs/CONTRIBUTING.md`
  and report, do not reject, a prefix outside it. The house rules do
  not add checks and do not change the verdict; the rubric does that.

In eval mode, ignore `scope.md` entirely. The bundle's `## Repo facts`
is the only statement of where the PR lives.

## The voice seam (live mode only)

In live mode, after the rubric has been applied, read `voice-guide.md`
in this skill directory: the student's own rules for how they write
upstream, with wrong/right pairs. Hold the outgoing text, the PR title
and the PR description from `pr_draft.md`, against every rule and
every item in "Things I never post". For each rule the draft breaks,
write one line in the readable summary: the rule's name, the rule
quoted, and the draft sentence that breaks it. The voice guide never
changes the verdict on its own; it only changes the verdict if a check
in `rubric.md` names it as evidence, and this rubric does not. In eval
mode, ignore `voice-guide.md` entirely: voice is personal and carries
no gold labels.

## Component reads

The three components fit together like this, and you read all three
before grading:

- `rubric.md` is the brain. It defines a table of checks (name,
  evidence, pass condition, weight) and a verdict rule that says how
  grades combine into `accept` or `reject`, including what `unclear`
  does. Grade every row in the table, in table order. `required` rows
  gate the verdict; `preferred` rows never change it.
- `references/evidence-guide.md` is the map. For each evidence family
  it says where that evidence lives in a bundle and in a live working
  copy, and what good looks like. When a check names a family, look
  where the guide points, not wherever seems handy.
- `procedure.md` is the hands. Execute it as written: its read order,
  its gathering moves, its check-execution rules, its verdict assembly.
  Do not reorder steps, skip steps, or merge steps because the package
  looks simple.

When the procedure is silent on something you need to decide (where a
piece of evidence is, how to treat an absence it does not mention),
note the gap in the readable summary as a procedure gap, decide it by
the rubric's own Fail clause for that row, and move on. Never
improvise a new step without saying so. If `rubric.md` has no filled-in
checks or no verdict rule, or `procedure.md` has no filled-in steps,
stop and say that this tool cannot grade without a rubric and a
procedure; do not invent checks at runtime. The instruction comments
inside a file are not content.

## Verdict and output

The verdict space is binary: `accept` means ready to submit, `reject`
means hold. There is no third value, no score, and no "accept with
reservations"; reservations go in the evidence lines. Before the JSON,
you may print a readable summary: one line per check (name, grade, the
deciding fact), then in live mode the voice-guide report and any
procedure gaps. Then end your reply with this fenced JSON block, valid,
with the `item` set to the PR URL (live) or the bundle id (eval), one
entry per rubric row in table order, and nothing after the closing
fence:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first. Never grade a check without naming the fact or quote
  that decided it, in the `evidence` field. "Looks fine" and "seems
  complete" are not evidence; a quoted hunk, a quoted sentence, or
  "absent: <section>" is.
- Grade the thing, not the polish. A terse description over a diff that
  matches its plan can be ready; a confident, fully templated
  description over a diff with a stray hunk is not. Read the diff
  against the plan, the evidence against the test plan, the description
  against the diff, and the description against the repo's asks. Never
  grade length, headings, formatting, or commit count.
- The rubric decides, not you. If a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary; the fix belongs in `rubric.md`, not in this run.
- The procedure decides how, not you. Follow `procedure.md` as written
  and report its gaps instead of papering over them.
- Honest shortfall is not drift. A diff that does less than the plan
  because a deviation note says so, and a description that restates the
  deferral, is graded on what it says, not on what it skipped. Drift is
  what the notes do not say.
- Unclear defaults to fail. Treat `unclear` exactly as the rubric's
  verdict rule directs. If the rule says nothing, an `unclear` grade on
  a required row is a failing one: a PR you cannot verify from the
  package is a PR that is not ready to submit.
