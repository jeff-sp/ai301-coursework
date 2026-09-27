# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

An eval bundle has five sections in a fixed order: `## Repo facts`,
`## Issue`, `## Thread highlights`, `## Candidate claim comment`, and
`## Candidate repro report`. The first three are the issue side; the
last two are the candidate side. In live mode the issue side is the
GitHub issue page and the repo's docs, and the candidate side is the
student's draft file(s). Read the issue side first so every candidate
claim is read against something.

## Environment

<!-- Where the environment record lives, and what a sufficient one
looks like against the issue's stated target. -->

**Where it lives.** In the bundle: the `## Candidate repro report`
section, usually a line starting "Environment:" but sometimes a
sentence or a table near the top. The target to compare against is in
the `## Issue` section (the reporter's version and OS, often in a
"Version:" or "Environment:" line or the template's fields) and in
`## Thread highlights` when a maintainer confirms the bug on a
specific version or branch. The `latest release` line in
`## Repo facts` says what "current" means at capture time. Live: the
draft repro comment for the record; the issue body, its template
fields, and maintainer comments for the target; the repo's Releases
page or tags for current.

**What good looks like.** The record names the version of the software
under test and the OS or platform it ran on, in words a reader could
match to their own machine ("ripgrep 15.2.0 (cargo install), Arch
Linux (x86_64)"). When the version, OS, or shell differs from what the
issue names, the report says so in the same breath ("the issue was
filed against 13.0.0; behavior is unchanged on 15.2.0"). A record that
lists hardware but not the program's version, or says only "Windows
11", does not let a stranger place the attempt. Testing an older
version than the one the issue or thread confirms the bug on, without
noting it, means the artifact is about the old version and not the
bug.

## Steps

<!-- Where the reproduction steps live, and what makes them followable
by a stranger, starting state to trigger. -->

**Where it lives.** In the bundle: the `## Candidate repro report`
section, in a "Steps:" list, a fenced command block, or prose
describing what was run. The trigger they must reach is in the
`## Issue` section (its own reproduction steps, minimal command, or
input file) and sometimes sharpened in `## Thread highlights` (an
owner saying a flag is also required). Live: the draft repro comment
for the steps; the issue body and comments for the trigger.

**What good looks like.** Every input needed to get from a clean start
to the trigger is present: the exact command or action, the input data
or file contents (or "the issue's file verbatim"), any config, and the
starting state (fresh directory, empty config, which driver). A reader
with only public material could type the same thing. Steps fail the
stranger test when they rest on something the reader cannot get (a
private monorepo, an unshared `.golangci.yml`, an internal hook) or
when they drop an input the issue names as part of the trigger (the
issue runs `minikube start --driver vmware`, the report runs
`minikube start`). Describing the input precisely is enough; the
report does not need to paste a file when it says exactly what the
file contains.

## Behavior shown

<!-- Where the artifacts live (output excerpts, logs, screenshots),
and what it means for an artifact to show the issue's behavior rather
than an adjacent one. -->

**Where it lives.** In the bundle: fenced code blocks, quoted log
lines, and screenshot descriptions inside `## Candidate repro report`,
and the "Actual:" sentences that point at them. The behavior to match
is in the `## Issue` section: the error text, exit code, output shape,
panic message, or missing element the reporter shows. Live: the draft
repro comment's code blocks and attached images; the issue body's own
output blocks and any maintainer comment that narrows the symptom.

**What good looks like.** First, an artifact exists: at least one
block of output, error text, log lines, or a screenshot described down
to what it displays, produced by running the report's own steps. A
report that only tells the reader what happened has no artifact.
Second, the artifact shows the same behavior the issue shows, from the
same trigger: the same error text or class of failure (a panic where
the issue panics, a missing header where the issue's header is
missing), produced with the issue's syntax and code path. Adjacent
behavior is a miss: a graceful "invalid value" error (exit 1) where
the issue reports a capacity-overflow abort (exit 101); a compile
error from a rewritten expression where the issue reports a wrong
runtime result; a colon in the input where the issue uses an equals
sign; a version banner and session list where the issue describes a
blank pane; garbled text with the terminal still open where the issue
describes a crash. A control run (the same command minus the trigger,
behaving correctly) strengthens a match but is not required. For a
cannot-reproduce, the artifact shows the issue's stated steps, input,
and configuration being run and the outcome that was actually
observed. The trigger is what the issue tells you to do (the layout,
the command, the config); the shell, OS, or hardware it was done on is
environment, and a named environment difference is what makes the
cannot-reproduce informative rather than a miss.

## Honesty

<!-- Where claims and their backing meet: how to tell a report that
says exactly what happened (including an honest cannot-reproduce) from
one that claims more than its evidence shows. -->

**Where it lives.** In the bundle: the report's concluding sentences
inside `## Candidate repro report` ("Actual:", "Result:", "confirmed",
"root cause", "not limited to", "100% reproducible"), read against the
artifacts and steps in the same section. The claim comment's
assertions about what was reproduced also count. Live: the same
sentences in the draft repro comment and draft claim comment.

**What good looks like.** Each claim in the conclusion points at an
artifact in the report that shows exactly that. "The panic shown
above, on the final element of the range" is backed; "the crash is
confirmed" over a validation error is not. A root cause is honest only
when the report shows the trace, test, or code path that establishes
it; "I verified this race condition" with no artifact is an assertion.
Generalizing ("also on the Store release", "on two machines", "every
single time") is honest only when those runs are shown. An honest
cannot-reproduce says so in the first line ("Result: I could NOT
reproduce scenario 2"), shows the attempt, and names what differed from
the report's conditions and what a triggering setup would likely need.
Confidence words with nothing under them ("clearly", "definitely",
"conclusively", "guaranteed") are a signal to check the artifact
harder, not evidence themselves.

## Comms

<!-- Where the words meet the repo: the claim comment against the
issue, the comments against the repo's stated templates and
contribution policy (including AI-use disclosure requirements), and
what specific-and-honest looks like next to boilerplate. -->

**Where it lives.** In the bundle: `## Candidate claim comment` read
against the `## Issue` title, body, and `## Thread highlights`; the
`contribution policy` and `bug reports` lines in `## Repo facts` for
the repo's rules. Live: the draft claim comment; the repo's
CONTRIBUTING.md, any AI policy file (AI_POLICY.md, AI_USAGE_POLICY.md,
or a section of CONTRIBUTING.md), the issue and PR templates under
`.github/`, and the issue thread for other claims and maintainer
pointers. For the Path Review repo, `docs/CONTRIBUTING.md` and the PR
template are the policy sources.

**What good looks like.** A specific claim names something only this
issue has (the behavior, a version, a file or function, a flag, a
pointer a maintainer left in the thread) and says what the author will
do next in concrete terms ("read how the translator loads locale files
relative to when FES fires"). Boilerplate could be pasted onto any
issue: "+1", "assign me", "great project", "looks like a good one for
me". A modest claim promises only what the author controls (reproduce,
read, test a patch, report back) and names no delivery date; "I will
fix it within 2 days guaranteed" and "keep this issue reserved for me"
are over-promises. On policy: when the repo requires disclosing AI
assistance in comments ("all AI usage in any form must be disclosed"),
a passing comment names the tool and the extent of its use ("I used an
AI assistant to help me organize this report; I ran and verified every
step myself"). When the policy is silent, governs only code or pull
requests, or states no disclosure ask for issue comments, nothing is
required. Every course package is AI-assisted work, so under a
disclosure rule, silence fails. A rule that comments must be in the
contributor's own words is satisfied by a first-person account of what
they ran and saw.
