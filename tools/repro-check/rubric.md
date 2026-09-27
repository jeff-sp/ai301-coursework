# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

Each row tests one thing. Rows 1 and 2 read the environment record,
rows 3 to 6 read the repro report's proof, rows 7 and 8 read the claim
comment, and row 9 reads both comments against the repo's policy. In a
live claim-only run, rows 1 to 6 are not yet applicable and rows 7 to
9 decide.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Environment recorded | The repro report's environment record (the "Environment:" line or equivalent sentences naming versions and platform) | Pass when the report names both the version of the software under test and the operating system or platform it ran on. Hardware specs, "my machine", or the OS alone do not satisfy this. Fail when either the version or the platform is absent | required |
| Target version honored | The version in the report's environment record, read against the version the issue names or that maintainers confirmed in the thread highlights | Pass when the tested version is the issue's version or newer, or when a differing version, OS, or shell is named as a difference in the report itself. Fail when the report tests a version older than the one the issue or thread confirms the bug on without saying so. Unclear when no version is recorded | required |
| Steps re-runnable by a stranger | The steps in the repro report: commands run, input files or data, configuration, starting state, read against the trigger the issue describes | Pass when every input needed to reach the trigger is given in the report or is the issue's own input used verbatim, so a reader with only public material could run the same thing. Fail when a step depends on material the reader cannot obtain (a private repository, an unshared config or hook) or when the steps leave out an input the issue names as part of the trigger | required |
| Artifact shown | Output excerpts, error text, log lines, rendered output, or screenshots quoted or described in the repro report | Pass when the report contains at least one artifact produced by running its own steps, with the specific content shown (the actual lines, or a screenshot described down to what it displays). Fail when the report only tells the reader what happened without showing any of it | required |
| Artifact shows the issue's behavior | The artifact read against the behavior the issue describes (error text, exit code, output shape, what is missing), and the command or input that produced the artifact read against the issue's trigger | Pass when the artifact exhibits the behavior the issue reports and was produced by the issue's trigger (same syntax, same operator, same code path). Also pass when the report states it could not reproduce and the artifact shows the issue's stated steps, input, and configuration being run with the outcome that was observed; a difference in OS, shell, or hardware is an environment difference, not a missing trigger, and belongs to Target version honored. Fail when the artifact shows a different behavior (a graceful validation error where the issue reports a crash, a compile error where the issue reports a wrong result, output that only shows the program running) or when the input differs from the issue's trigger in a way that changes which code path runs | required |
| Outcome stated as observed | The report's concluding statements (Actual, Result, "confirmed", "root cause") read against the artifacts and steps in the same report | Pass when every claim of reproduction, confirmation, generality, or cause is backed by an artifact in the report that shows exactly that, and when a failed attempt is stated as a failed attempt. Fail when the report claims more than its artifacts show: confirmation over a non-matching artifact, a root cause with no trace or test shown, or generalization to versions or builds the report did not run | required |
| Claim names this issue's specifics | The candidate claim comment read against the issue's title, body, and thread highlights | Pass when the comment names at least one concrete detail of this issue (the behavior, a version, a file, a function, a flag, or a pointer from the thread) and states a concrete next step the author will take. Fail when the comment could be posted unchanged on a different issue: me-too, assign-me, praise for the project, or "looks like a good one for me" | required |
| Claim promises only investigation | The statements in the candidate claim comment about what the author will do | Pass when the author commits only to steps within their control (reproduce, read named code, test a patch, report back) and names no delivery date. A plan to attempt a fix is fine. Fail when the comment guarantees a fix, gives a deadline, or asks for the issue to be reserved or held for them | required |
| AI disclosure per repo policy | The "contribution policy" line in the repo-facts block, read against both the candidate claim comment and the candidate repro report | Pass when the policy does not require disclosing AI assistance in issue comments (no stated policy, a policy about code or pull requests only, or one that states no disclosure ask for comments). Also pass when the policy requires disclosure and either comment names the AI tool used and the extent of its help. Fail when the policy requires disclosing AI use in comments or "all AI usage in any form" and neither comment discloses. Every package is treated as AI-assisted work, so silence under a disclosure rule is a fail | required |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
Accept if every required check passes. There are no preferred checks; if one is added later it never changes the verdict. Unclear counts as fail: proof that cannot be located is proof that is not ready to post. In a live claim-only run, checks marked not yet applicable are left out, and the verdict is decided by the remaining checks (Claim names this issue's specifics, Claim promises only investigation, AI disclosure per repo policy).
