# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer alive | "last 5 default-branch commits" in the repo-facts block (commit dates and authors); the "maintainer first-response sample" as supporting evidence | At least one default-branch commit by a human, or a bot merging a human's PR, dated within 90 days of the capture date | required |
| Repo in use | "archived:" flag, "latest release", and "last 5 default-branch commits" in the repo-facts block | Not archived, AND either a release within 12 months of the capture date or a default-branch commit within 90 days of it | required |
| Scope fits newcomer | Issue title, body, labels, and the comment thread; "linked PRs" state list in the repo-facts block | Pass when the issue asks for one outcome and says where it lands: a named file or page, a command, or a described wrong behavior with the expected behavior. A proposed-changes list that spells out the edits needed for that one outcome is one deliverable even when it touches several files, and passes. Fail only when one of these holds: the issue calls itself a tracking, umbrella, meta, or mega issue, or explicitly says its items are to be split into separate PRs or issues, or already has three or more linked PRs working on different parts of it; the thread shows the design still being debated with no maintainer decision; a maintainer says the fix touches core internals; the issue is a usage question with no change requested; or two or more closed, unmerged PRs are linked to it. A feature request (new behavior, as opposed to a bug fix or a docs or cleanup task) also needs maintainer endorsement: the opener is an OWNER, MEMBER, or COLLABORATOR, or the issue carries a triage label, or a maintainer comment supports the direction. An unendorsed feature request from an outside author or a bot is a product decision, not a bounded task: fail. Fail as well when the issue leaves an input the work depends on undecided ("TBD", "to be determined", asset or design not chosen). A terse body is not a scope failure: when the opener is an OWNER, MEMBER, or COLLABORATOR, or the issue carries a bug or good-first-issue label, a one-line statement of what is wrong or missing counts as a described wrong behavior with the expected behavior implied, and words like "several" or "etc." in such a report describe one class of fix, not an umbrella. | required |
| Unclaimed | "this issue: assignees" and "linked PRs" in the repo-facts block; claim comments in the thread ("I'll take this", "working on this", "can I work on this") | No assignee; no open linked PR; no claim comment within 90 days of the capture date unless a maintainer later said the issue is free. A claim older than 90 days with no PR behind it is stale and does not count | required |
| AI policy allows contribution | "contribution policy" line in the repo-facts block | Pass unless the policy states an outright ban on AI-generated code or documentation. Conditions such as disclosure, personal understanding, testing, or human review are terms to follow and pass. No stated policy passes | required |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->
accept if every required check passes; preferred checks never change the verdict, they rank accepted issues; unclear counts as fail.