# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/47

**Verdict output**

Live-mode run on 2026-09-20 (`claude -p --model sonnet`, three candidates: #73, #47, #37). Output pasted verbatim from the run's saved transcript.

````
## Findings

**Repo-level facts (apply to all three):** not archived; last 5 default-branch commits are all by a human (Aburke225 / Andrew Burke), most recent 2026-09-16, well within 90 days → maintainer alive, repo in use. No stated AI-use ban anywhere (no `AI_POLICY.md`, `docs/CONTRIBUTING.md` and PR template are silent on AI) → policy passes for all three.

### Issue #73: README and `.env.example` disagree about which LLM API key to set
- Maintainer alive: pass (recent human commits)
- Repo in use: pass (recent push, not archived)
- Scope fits newcomer: pass, single described wrong behavior + expected fix, two named files, 1-2h estimate
- Unclaimed: pass, no assignees/comments/linked PRs
- AI policy: pass, no ban
- **Verdict: accept**

### Issue #47: API docs don't include example `curl` commands
- Maintainer alive: pass
- Repo in use: pass
- Scope fits newcomer: pass, one deliverable (add curl examples to `docs/API.md`), opener is a COLLABORATOR
- Unclaimed: pass, no assignees/comments/linked PRs
- AI policy: pass
- **Verdict: accept**

### Issue #37: API reference doc is missing the `POST /profiles` request body schema
- Maintainer alive: pass
- Repo in use: pass
- Scope fits newcomer: pass, one outcome (document request bodies for two endpoints), touches 3 named files for that single outcome, opener is COLLABORATOR, no undecided inputs
- Unclaimed: pass
- AI policy: pass
- **Verdict: accept**

**Ranking (all three accepted):** #47 first, #37 second, #73 third. #47 is the closest match to your background: writing `curl` invocations against documented endpoints is essentially the Postman/Newman API-testing work you already do daily, with no code changes required. #37 is also API-documentation work in the same vein, but it requires reading `api/routes/profiles.py` and `api/schemas/review.py` (Python, and multipart-form handling specifically) to get the field examples right, so it's a slightly bigger lift. #73 is the safest and quickest (1-2h) but is a generic docs/config consistency fix, less connected to your API-testing strength than the other two.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/47",
    "checks": [
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Last 5 default-branch commits all by human Aburke225, most recent 2026-09-16"},
      {"name": "Repo in use", "grade": "pass", "evidence": "Not archived; pushedAt 2026-09-16, within 90 days"},
      {"name": "Scope fits newcomer", "grade": "pass", "evidence": "Body names one file (docs/API.md) and one outcome: add curl examples; opener is COLLABORATOR"},
      {"name": "Unclaimed", "grade": "pass", "evidence": "No assignees, no comments, no linked PRs"},
      {"name": "AI policy allows contribution", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PR template state no AI restriction"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/37",
    "checks": [
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Last 5 default-branch commits all by human Aburke225, most recent 2026-09-16"},
      {"name": "Repo in use", "grade": "pass", "evidence": "Not archived; pushedAt 2026-09-16, within 90 days"},
      {"name": "Scope fits newcomer", "grade": "pass", "evidence": "Body names one outcome (document POST /profiles and POST /reviews bodies) across 3 named files, one deliverable; opener is COLLABORATOR"},
      {"name": "Unclaimed", "grade": "pass", "evidence": "No assignees, no comments, no linked PRs"},
      {"name": "AI policy allows contribution", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PR template state no AI restriction"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73",
    "checks": [
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Last 5 default-branch commits all by human Aburke225, most recent 2026-09-16"},
      {"name": "Repo in use", "grade": "pass", "evidence": "Not archived; pushedAt 2026-09-16, within 90 days"},
      {"name": "Scope fits newcomer", "grade": "pass", "evidence": "Body describes wrong behavior (README/.env.example key mismatch) with expected fix, two named files, 1-2h estimate"},
      {"name": "Unclaimed", "grade": "pass", "evidence": "No assignees, no comments, no linked PRs"},
      {"name": "AI policy allows contribution", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PR template state no AI restriction"}
    ],
    "verdict": "accept"
  }
]
```
````

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

1. --limit 3 smoke run: 2/3. issue-01 rejected, note "failed: Maintainer alive, Scope fits newcomer". (Two earlier attempts errored at 0/0 before any grading: an invalid Write(**) permission rule in WSL settings, then an expired WSL login.)
2. --only issue-01: 0/1. Still "failed: Scope fits newcomer" after switching liveness to commit dates.
3. --only issue-01,issue-05,issue-10,issue-15,issue-20: 4/5. issue-01 now accepted; issue-20 "graded accept" against gold reject.
4. --only issue-20,issue-09: 1/2. issue-20 still accepted; the endorsement edit had not been saved to disk.
5. Full run: 19/20 (bar: PASS). issue-04 "failed: Scope fits newcomer"; categories clear-accept 7/8.
6. Full run with --save-run: 20/20 (bar: 18/20: PASS). This is the committed eval-run.txt.

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

**Issue analysis**

issue-20 (excalidraw/excalidraw#11811). My rubric: reject. Gold: reject, note "one-line feature wish with no spec and a product decision hiding inside".

My first scope wording accepted it, because the issue does name a place and a success test: "Likely surface: `packages/excalidraw` editor (toolbar + element)" and "Success looks like: logo tool in the shapes toolbar". The bundle line "opened by cursor[bot] (NONE) on 2026-08-02, state open, labels: none" and "Comments (0 total)" showed what my rule did not account for: no one on the project asked for this, and the body admits "Logo asset TBD". All gold accepts in the set are opened by a COLLABORATOR, MEMBER, or CONTRIBUTOR and carry a triage label. The one accepted feature request, issue-09, has a MEMBER opener who said "just give it a try". So I added a maintainer-endorsement requirement for feature requests and a fail on undecided inputs. So issue-20 fails both checks: a bot-filed feature without maintainer endorsement is a product decision and not a bounded task.

**Check rationale**

Quoted from tools/issue-select/rubric.md:

| Maintainer alive | "last 5 default-branch commits" in the repo-facts block (commit dates and authors); the "maintainer first-response sample" as supporting evidence | At least one default-branch commit by a human, or a bot merging a human's PR, dated within 90 days of the capture date | required |

My first version used "a maintainer commented within 30 days" and failed issue-01 because its bundle says "Comments (0 total, first 0 shown)". Since seven of eight gold accepts have empty threads, thread activity does not show maintainer is alive. The commit list is better: issue-01's newest commit is "2026-08-04 by codewithdaniel1", one day before capture. A 90-day cutoff separates all accepts (newest commit at most 22 days before capture) from dead repos (issue-02 last commit 2023-12-09, issue-07 2023-02-01, issue-17 2021-11-13).

**Trade-offs**

The check measures commits, not review. Repos with unreviewed PRs can still pass. calib-03 (httpie/cli#1898) shows this: the gold note says "PRs pile up unreviewed". First-response sample would catch it, but I kept that as supporting evidence only because this sample is mostly "no maintainer comment in thread" even for healthy repos (issue-01 has for of five with no maintainer comment). Switching to commit dates changed nothing: all three dead repos still reject, and the 20/20 run lost no accepts.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[Answer all three:

1. The issue's fit to your interests and to the time available.
2. What the verdict identified correctly, and what you weighed that the rubric could
   not.
3. The anticipated difficulty in claiming it.]

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
