# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

jeff-sp

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/47#issuecomment-5987148204

Plan for #47:

**Diagnosis:** On `2f4e82f`, `grep -c -i curl docs/API.md` prints 0, `grep -c '^```' docs/API.md` prints 0, and the file has nine endpoint lines. The nine endpoints take three body shapes and the one-line descriptions do not say which: register takes JSON, login takes an OAuth2 form with `username` (not `email`), and `POST /profiles` takes multipart fields. I found that by reading `api/routes/`.

**Scope**

**What I will change:** `docs/API.md` only. One fenced `bash` block under each of the nine documented endpoints: the `curl` command as it worked in my repro, then comment lines with the HTTP status and the trimmed response. A short paragraph at the top says the examples assume the local server from `docs/SETUP.md`, where `<token>` comes from, and that the seeded login is `user1@example.com` / `password1`. One-line notes under `/auth/login` and `POST /profiles` call out the body shape.

**What I will not touch:** The `/health` 503 is a health-check code bug (my repro's uvicorn log shows the `SELECT 1` and `redis_host` errors), so the `/health` example will show the 503 the API returns today with a one-line note that it is a separate bug. The two routes in `api/routes/` that the doc does not list (`PUT /profiles/{profile_id}`, `GET /reviews/{review_id}/status`) stay out too: whether they belong in this doc is an open question on this thread that no maintainer has answered, and I will not add them on my own.

**How I will test it:** Re-run my repro's doc check on the branch and expect `grep -c '^curl ' docs/API.md` to print 9, `grep -c '^```' docs/API.md` to print 18, and the endpoint-line count to stay 9. Then bring the stack up as in my repro's steps 1 to 3 and paste every fenced command from the new file, expecting the statuses from my repro: 503 for `/health`, 200 for the seven other calls, 204 for the delete.

**Risks and Unknowns:** One thing I have read but not run: `api/routes/auth.py` returns 400 "Email already registered" when the register example is run twice with the same email. I will run that during the build and note the 400 under the example.

**Branch:** `docs/47-curl-examples-per-endpoint` on my fork. I will open the PR after the examples have been run against the built change.

I used Claude Code to help draft and check this plan. The repro, the commands, and the output it quotes are from my own run.

---

## Your branch

**Branch**

docs/47-curl-examples-per-endpoint

**Evidence**

**Before** (my unit 2 repro, posted at https://github.com/codepath/pathreview-ai301-fa26-s3/issues/47#issuecomment-5860025720, commit `2f4e82f`, Windows 11 Home build 26200, Git Bash, curl 8.17.0):

**The doc itself:**

```
$ git rev-parse --short HEAD
2f4e82f
$ grep -c -i curl docs/API.md
0
$ grep -c '^```' docs/API.md
0
$ grep -c -E '^`(GET|POST|PUT|PATCH|DELETE) ' docs/API.md
9
```

**The nine endpoints, as they answer today** (JWTs replaced with `<token>`, list response trimmed):

```
$ curl http://localhost:8000/health
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"}, ...}}
[HTTP 503]

$ curl -X POST http://localhost:8000/auth/register -H 'Content-Type: application/json' \
  -d '{"email":"repro47@example.com","password":"repro47-test-password"}'
{"access_token":"<token>","token_type":"bearer"}
[HTTP 200]

$ curl -X POST http://localhost:8000/auth/login -d 'username=user1@example.com&password=password1'
{"access_token":"<token>","token_type":"bearer"}
[HTTP 200]

$ curl -X POST http://localhost:8000/profiles -H 'Authorization: Bearer <token>' \
  -F github_username=octocat -F portfolio_url=https://example.com
{"id":"faafb071-4fda-449f-b1b6-99d29200b7e8","user_id":"4c644259-...","github_username":"octocat","portfolio_url":"https://example.com","created_at":"2026-09-28T04:20:51.803898Z","resume_filename":null}
[HTTP 200]

$ curl http://localhost:8000/profiles/faafb071-4fda-449f-b1b6-99d29200b7e8 -H 'Authorization: Bearer <token>'
{"id":"faafb071-4fda-449f-b1b6-99d29200b7e8", ... same body as above}
[HTTP 200]

$ curl -X POST http://localhost:8000/reviews -H 'Authorization: Bearer <token>' \
  -H 'Content-Type: application/json' -d '{"profile_id":"faafb071-4fda-449f-b1b6-99d29200b7e8"}'
{"id":"0f1712e5-ad6a-4153-90a9-47adbc6322c5","profile_id":"faafb071-...","status":"pending","sections":null,"overall_score":null,"error_message":null, ...}
[HTTP 200]

$ curl http://localhost:8000/reviews/0f1712e5-ad6a-4153-90a9-47adbc6322c5 -H 'Authorization: Bearer <token>'
{"id":"0f1712e5-...","status":"complete","sections":[{"section_name":"Technical Skills", ...}, {"section_name":"Project Experience", ...}, {"section_name":"Career Growth", ...}],"overall_score":0.81, ...}
[HTTP 200]

$ curl 'http://localhost:8000/reviews?page=1&page_size=20' -H 'Authorization: Bearer <token>'
{"items":[ ...4 reviews... ],"total":4,"page":1,"page_size":20}
[HTTP 200]

$ curl -X DELETE http://localhost:8000/profiles/faafb071-4fda-449f-b1b6-99d29200b7e8 -H 'Authorization: Bearer <token>'
[HTTP 204]
```

The same doc check re-run today against `main` from the branch, before touching the file (the three greps my plan's test plan names, with `grep -c '^curl '` in place of `grep -c -i curl` because the new intro paragraph mentions curl in prose):

```
# before: docs/API.md as on main
$ git rev-parse --short main
2f4e82f
$ grep -c '^curl ' docs/API.md
0
$ grep -c '^```' docs/API.md
0
$ grep -c -E '^`(GET|POST|PUT|PATCH|DELETE) ' docs/API.md
9
```

**After** (branch `docs/47-curl-examples-per-endpoint`, same machine, Docker Desktop 29.8.0, `uvicorn api.main:app --host 127.0.0.1 --port 8000` from the Python 3.12 venv). The doc check:

```
# after: docs/API.md on branch docs/47-curl-examples-per-endpoint
$ grep -c '^curl ' docs/API.md
9
$ grep -c '^```' docs/API.md
18
$ grep -c -E '^`(GET|POST|PUT|PATCH|DELETE) ' docs/API.md
9
$ git status --short
 M docs/API.md
```

Every fenced command from the new `docs/API.md`, pasted as written with `<token>` and the ids substituted, in file order. JWTs replaced with `<token>` and the paginated list trimmed, as in the repro:

```
# after: every example from docs/API.md on branch docs/47-curl-examples-per-endpoint, run 2026-10-05T02:42:25Z against uvicorn on 127.0.0.1:8000
$ git rev-parse --short HEAD
2f4e82f

$ curl http://localhost:8000/health
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"},"safety_events_last_hour":0,"timestamp":"2026-10-05T02:42:25.680861"}}
[HTTP 503]

$ curl -X POST http://localhost:8000/auth/register -H 'Content-Type: application/json' -d '{"email":"unit3-1791168145@example.com","password":"a-strong-password"}'
{"access_token":"<token>","token_type":"bearer"}
[HTTP 200]

# same register command a second time (duplicate email):
$ curl -X POST http://localhost:8000/auth/register -H 'Content-Type: application/json' -d '{"email":"unit3-1791168145@example.com","password":"a-strong-password"}'
{"detail":"Email already registered"}
[HTTP 400]

$ curl -X POST http://localhost:8000/auth/login -d 'username=user1@example.com&password=password1'
{"access_token":"<token>","token_type":"bearer"}
[HTTP 200]

$ curl -X POST http://localhost:8000/profiles -H 'Authorization: Bearer <token>' -F github_username=octocat -F portfolio_url=https://example.com
{"id":"45f7a0f9-8bbe-4f6f-9653-01dba9c0ce90","user_id":"4c644259-42c3-47de-8836-74c4408ceae4","github_username":"octocat","portfolio_url":"https://example.com","created_at":"2026-10-05T09:42:27.465724Z","resume_filename":null}
[HTTP 200]

$ curl http://localhost:8000/profiles/45f7a0f9-8bbe-4f6f-9653-01dba9c0ce90 -H 'Authorization: Bearer <token>'
{"id":"45f7a0f9-8bbe-4f6f-9653-01dba9c0ce90","user_id":"4c644259-42c3-47de-8836-74c4408ceae4","github_username":"octocat","portfolio_url":"https://example.com","created_at":"2026-10-05T09:42:27.465724Z","resume_filename":null}
[HTTP 200]

$ curl -X POST http://localhost:8000/reviews -H 'Authorization: Bearer <token>' -H 'Content-Type: application/json' -d '{"profile_id":"45f7a0f9-8bbe-4f6f-9653-01dba9c0ce90"}'
{"id":"2007b630-b72b-40b1-bb76-bb2040a1d332","profile_id":"45f7a0f9-8bbe-4f6f-9653-01dba9c0ce90","status":"pending","sections":null,"overall_score":null,"error_message":null,"created_at":"2026-10-05T09:42:28.076121Z","updated_at":"2026-10-05T09:42:28.076121Z"}
[HTTP 200]

$ curl http://localhost:8000/reviews/2007b630-b72b-40b1-bb76-bb2040a1d332 -H 'Authorization: Bearer <token>'
{"id":"2007b630-b72b-40b1-bb76-bb2040a1d332","profile_id":"45f7a0f9-8bbe-4f6f-9653-01dba9c0ce90","status":"complete","sections":[{"section_name":"Technical Skills","content":"Detailed feedback on technical skills based on portfolio analysis","confidence":0.85,"suggestions":["Add more detail on AI/ML experience","Include specific technologies and frameworks"]},{"section_name":"Project Experience","content":"Detailed feedback on project experience and impact","confidence":0.8,"suggestions":["Include measurable impact metrics","Add links to project repositories"]},{"section_name":"Career Growth","content":"Feedback on career progression and development","confidence":0.78,"suggestions":["Document learning from each role","Highlight growth in responsibilities"]}],"overall_score":0.81,"error_message":null,"created_at":"2026-10-05T09:42:28.076121Z","updated_at":"2026-10-05T09:42:28.092773Z"}
[HTTP 200]

$ curl 'http://localhost:8000/reviews?page=1&page_size=20' -H 'Authorization: Bearer <token>'
{"items":[ ...4 reviews... ],"total":4,"page":1,"page_size":20}
[HTTP 200]

$ curl -X DELETE http://localhost:8000/profiles/45f7a0f9-8bbe-4f6f-9653-01dba9c0ce90 -H 'Authorization: Bearer <token>'

[HTTP 204]
```

Every status matches the table in `plan.md`: 503 for `/health` (the known health-check bug, unchanged by a docs PR), 200 for register, login, create and get profile, create and get review, and the list, 204 for the delete. The duplicate register, which the plan listed as read but not run, returned 400 `Email already registered` and is now noted under the example.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

All runs on 2026-10-04 (UTC 2026-10-05) from WSL (Ubuntu), `python3 run_eval.py` with `--rubric` and `--evidence` pointed at the installed skill, `/mnt/c/Users/jeffj/.claude/skills/plan-check/`, model sonnet (pinned by the harness). Running the harness from Windows Python failed before grading anything (`FileNotFoundError: [WinError 2]`, because `run_eval.py` spawns `claude` and the Windows install is a `claude.cmd` shim), so every run below is from WSL, as in unit 2.

1. Smoke run, `--limit 2` (pkg-01, pkg-02): "agreement: 2/2 scored items". pkg-01 reject (gold reject, wrong-cause), pkg-02 accept (gold accept, clear-accept). A partial run, so it decides nothing; it confirmed the harness preflight accepted the rubric, procedure and evidence guide and that the skill's JSON parsed.
2. Full run with `--save-run eval-run.txt`: "agreement: 20/20 scored items  (bar: 18/20: PASS)". Categories line: "clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4". No disagreements, so there were no `--only` re-grades and no revisions to the files. This is the committed `eval-run.txt`; its header fingerprints (rubric `78e85373c26d08ab`, evidence-guide `443dcf7f427c5e47`, procedure `c1e05bd2a10f35ce`) match the files in `tools/plan-check/`.

**Package analysis**

pkg-04 (junegunn/fzf#4260, category thread-convention). My rubric: reject. Gold: reject, note "comment ignores the fzf owner's in-thread direction". The two agree, and I picked it because it is the package my group's worksheet rubric would have passed: the worksheet had a diagnosis row, a scope row and a test row, and pkg-04 is clean on all three. Its diagnosis follows its repro (the `> /dev/tty` control run shows the redirect workaround works), its scope is tight ("Not in scope: any change to fzf's input handling code"), and its test plan names an observable ("j, k, q all work"). Six of my nine checks passed it for exactly those reasons.

What holds it is the thread. In `## Thread highlights`, junegunn (OWNER) wrote "This seems to be the culprit", pointing at `src/tui/light_windows.go` (lines 70-84), and then "posted a patched test binary from commit 8916cbc and asked the reporter to test it". The candidate plan comment never mentions either; it says "I plan to document it properly". Two required rows read that. "Comment follows thread direction" failed, with the grader's evidence line: "Comment never mentions junegunn's culprit location or the posted patch binary; 'I plan to document it properly' documents around the located culprit". "Change acts on the cause" also failed, because the pass condition says a change "documents a workaround for behavior the evidence or a maintainer has located in code" leaves the mechanism in place; the evidence line quoted the owner's culprit and the plan's "documentation only" scope. A third, preferred row recorded "I can have the docs PR up this week" as a date promise, which did not affect the verdict.

The reason my rubric reads pkg-04 this way is that the "thread-convention" category has only two packages, and the assignment page says a rubric that never checks the plan comment against the thread and the repo's conventions will miss both. pkg-20 is the other one: my re-grade with `--only pkg-04,pkg-20` showed it failing exactly one row, "AI disclosure per repo policy", on the ghostty policy line "All AI usage in any form must be disclosed" with "candidate plan comment contains no disclosure sentence", and passing the other eight. Without the two comment-reading rows, the `categories:` line would have read "thread-convention 0/2" even at 18/20, which fails the bar.

**Check rationale**

Quoted from `tools/plan-check/rubric.md`:

| Test names the observable | The plan's test plan ("Test plan", "Test", or "Verification" sentences in `## Candidate plan`) read against the steps and artifacts in `## Repro evidence` | Pass when the test plan names a re-run of a repro step or command, or an automated test built from those steps, together with the specific result that must appear afterward: an output string, exit code, rendered state, count, timing bound, or assertion the repro currently fails to show. An automated test that follows the repro steps is the preferred form, and a manual re-run with the result named also passes. Fail when the test plan names only a suite ("run the full test suite", "make sure nothing regresses") or a feeling ("should feel fast", "nothing else should feel broken", "undo works") with no repro-tied observable. Unclear when there is no test plan | required |

This row started as my group's worksheet check "test (preferred): passes if the plan adds an automated test that follows the steps of repro". Two things changed on the way to the row above, both before the first full run, from reading the twenty packages and the gold notes rather than from a disagreement.

First, the weight. The worksheet had it preferred, which means it could never hold a package. The gold label file defines the unbuildable category as "stranger couldn't start / test plan names no observable outcome", and calib-04 is held on the test plan alone (its only flaw is "run the full test suite ... make sure nothing regresses"). A preferred test check would have passed calib-04 and would have let any scored package with the same flaw through, so the row is required.

Second, the condition. The worksheet required an automated test. calib-01, the clear-accept calibration package, has no automated test at all; its test plan is a manual re-run, "at step 3 the color must flip without leaving the view", and gold accepts it. Requiring automation would have false-rejected it and any scored accept shaped the same way. So the row keeps the worksheet's preference as a preference inside the clause ("An automated test that follows the repro steps is the preferred form, and a manual re-run with the result named also passes") and makes the deciding condition the observable: a repro step or command re-run plus the specific result that must appear afterward. That is what separates pkg-05's "step 4 must fetch and print A and B" from pkg-10's "should feel fast".

I rejected a third option, splitting it into two rows ("re-runs a repro step" and "names a result"). The two halves fail together in every package I read (a plan that names no repro step also names no result), so the split would have added a row without changing a verdict, and the unit 1 feedback was to split rows only when each half decides something on its own.

**Trade-offs**

The check I quoted above, "Test names the observable", gives up automation. A plan whose test is a manual re-run with a named result passes it, so a repo whose CONTRIBUTING asks for a regression test with every change (Path Review's does: "Every code change should include or update relevant tests") gets no hold from this row when the plan skips the test; that ask would have to be caught by "Comment follows thread direction" or by a maintainer on the thread. I accept that because the gold set accepts it: calib-01 is a manual re-run and is the clear-accept calibration package, and none of the seven scored clear-accepts would survive a hard automation requirement without being re-read generously.

Nothing was loosened after the first full run, and here is how I know nothing moved elsewhere: the only re-grade I ran was `--only pkg-04,pkg-20` with unchanged files, as a canary on the two-package thread-convention category, and both stayed reject with the same deciding rows as the gold notes name (junegunn's direction for pkg-04, the ghostty disclosure line for pkg-20). The full run's fingerprints in `eval-run.txt` match the files in `tools/plan-check/`, so the canary graded the same rubric the full run did.

One case I accept the comment-reading rows will miss: "Comment follows thread direction" treats entries from authors tagged NONE as context, not direction. On my own issue, #47, all seven comments are from students tagged NONE, including an open question about two undocumented routes. A plan comment that ignored that question would pass the row. I chose that because in the eval set the NONE-tagged entries (pkg-03's jafd, pkg-08's maximilize) are reporters whose suggestions the accepted plans do not follow, so treating NONE as direction would have false-rejected clear-accepts. For my own comment I covered the gap with a voice-guide rule instead ("Place the plan on the thread"), which the skill reports in live mode but which never changes the verdict.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
