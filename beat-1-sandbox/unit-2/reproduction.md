# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

jeff-sp

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/47#issuecomment-5859418678

Hi, first contribution here. I'd like to take #47: docs/API.md lists nine endpoints against `http://localhost:8000` and none has an example invocation.

Next I'll set up the project from docs/SETUP.md, confirm on the current main commit that the file has no curl examples, and call each endpoint with curl against the local server. I'll post that as a repro report here, with my OS, versions, and commit, before touching any docs.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/47#issuecomment-5860025720

Reproduced: `docs/API.md` at the current `main` commit lists nine endpoints and contains no example invocation for any of them. I set up the sandbox from `docs/SETUP.md`, then called all nine with `curl` to record what a working request looks like today.

**Environment:** Windows 11 Home build 26200 (x86-64), Git Bash from git 2.52.0.windows.1, curl 8.17.0, Python 3.12.10 venv, Docker Desktop 29.8.0 (Compose v5.5.1), repo commit `2f4e82f` on `main` (fork of codepath/pathreview-ai301-fa26-s3).

**Steps:**

1. `cp .env.example .env` and `docker compose up -d`. Postgres and Redis came up healthy.
2. Ran the `make setup` recipe by hand (`make` is not on Git Bash here): venv with Python 3.12, `pip install -e ".[dev]"`, `alembic upgrade head` (migrations 001 and 002 applied), `scripts/seed_db.py`. Skipped `pre-commit install` and the frontend `npm install` since neither affects the API.
3. Started the API alone: `uvicorn api.main:app --host 127.0.0.1 --port 8000`.
4. Checked the doc, then called each documented endpoint.

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

**Expected:** each endpoint entry in `docs/API.md` carries an example request a new developer can paste to check the API is working.

**Actual:** zero examples and zero code blocks in the file. Working out the requests above took reading `api/routes/`: register takes a JSON body, login takes an OAuth2 form body (`username`, not `email`), and profiles takes multipart form fields, so the one-line descriptions alone are not enough to write a first call.

**Also observed, not part of this issue:** `/health` returns 503 on a fresh setup. The uvicorn log for that request shows why, so a curl example for `/health` will show 503 until these are fixed:

```
[error    ] postgres_health_check_failed   error="Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')"
[error    ] redis_health_check_failed      error="'Settings' object has no attribute 'redis_host'"
```
 The `vector-db` container (chromadb 0.4.22) exits at start with `AttributeError: np.float_ was removed in the NumPy 2.0 release`; the health route does not actually contact it, so the API runs without it. `scripts/seed_db.py` crashed once on a `cp1252` encoding error printing a status glyph; re-running with `PYTHONUTF8=1` worked.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

All runs on 2026-09-27 from WSL, `python3 run_eval.py` with `--rubric` and `--evidence` pointed at `~/.claude/skills/repro-check/`, model sonnet (pinned by the harness).

1. Full run: 19/20 (bar: 18/20: PASS). One disagreement, pkg-10, note "failed: Artifact shows the issue's behavior". Categories line: "clear-accept 7/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4".
2. `--only pkg-10,pkg-02,pkg-17,calib-03 --include-calibration`: 3/3 scored items (calib-03 also agreed, reject). pkg-10 now accept; the three canaries stayed reject.
3. Full run with `--save-run eval-run.txt`: 20/20 scored items (bar: 18/20: PASS). Categories "clear-accept 8/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4". This is the committed `eval-run.txt`.

**Package analysis**

pkg-10 (starship/starship#7648). My rubric on run 1: reject. Gold: accept, note "honest cannot-reproduce: exact layout and config, prompt artifact shown, names the environment differences (Linux+zsh vs macOS+fish) and the PWD-resolution hypothesis for why fish matters".

Eight of my nine checks passed it. The one that failed was "Artifact shows the issue's behavior", and the grader's evidence line was: "Report admits fish's logical PWD resolution "looks necessary" to hit contract_repo_path's failure and that it lacked a fish shell for the attempt, so the issue's actual trigger was not executed". My first wording of the cannot-reproduce clause was "the artifact shows the issue's trigger being run with the outcome that was observed", and it never said what "trigger" meant. The report itself hands the grader a reading: it hypothesises that fish is what makes the bug fire, so the grader counted the shell as part of the trigger and decided the trigger had not been run. In the bundle, the issue's own reproduction is the symlink layout (`~/projects/app-dir` symlinked to `/Users/user/code/monorepo/packages/app-dir`) plus `[directory]` with `repo_root_style = "bold red"`, and the report's steps run exactly that (`ln -s ~/code/monorepo/packages/app-dir ~/projects/app-dir`, the same `starship.toml`). The shell is where it was run, not what was run, and the report names the shell and OS differences in its environment line. pkg-09, the other cannot-reproduce, passed the same check on run 1 because its report ran the issue's mechanism on the same OS class the issue named, so nothing invited the environment-as-trigger reading. After the revision below, pkg-10 accepted on runs 2 and 3.

**Check rationale**

Quoted from `tools/repro-check/rubric.md`:

| Artifact shows the issue's behavior | The artifact read against the behavior the issue describes (error text, exit code, output shape, what is missing), and the command or input that produced the artifact read against the issue's trigger | Pass when the artifact exhibits the behavior the issue reports and was produced by the issue's trigger (same syntax, same operator, same code path). Also pass when the report states it could not reproduce and the artifact shows the issue's stated steps, input, and configuration being run with the outcome that was observed; a difference in OS, shell, or hardware is an environment difference, not a missing trigger, and belongs to Target version honored. Fail when the artifact shows a different behavior (a graceful validation error where the issue reports a crash, a compile error where the issue reports a wrong result, output that only shows the program running) or when the input differs from the issue's trigger in a way that changes which code path runs | required |

The first version's cannot-reproduce clause read "the artifact shows the issue's trigger being run with the outcome that was observed". After run 1 I replaced "the issue's trigger" with "the issue's stated steps, input, and configuration" and added the sentence that OS, shell, and hardware are environment differences that belong to the "Target version honored" check. I rejected two other fixes. Moving the cannot-reproduce case out of this check entirely (letting "Outcome stated as observed" carry it) would have made every honest cannot-reproduce fail this required check, since by definition its artifact does not show the bug. Softening the "same syntax, same operator, same code path" condition would have let pkg-02 (a `18446744073709551614:` prefix range instead of the issue's `:-N` offset-from-end syntax) and calib-03 (a colon where the issue uses `=`) through, and those are exactly the wrong-target packages the check exists to hold. Keeping one check for "did the artifact come from the issue's trigger" and a separate check for "was the version or environment difference named" also follows the Unit 1 feedback to split compound checks so each row decides one thing.

**Trade-offs**

The revision loosened a required check, so before the confirming run I re-ran the affected package with canaries: `--only pkg-10,pkg-02,pkg-17,calib-03 --include-calibration`. pkg-02 (graceful arg-validation error narrated as a capacity-overflow crash), pkg-17 (garbled escape output with the terminal alive, presented as a crash) and calib-03 (colon for equals, syntax error narrated as the panic) all stayed reject, and pkg-10 flipped to accept. The single-package category, disclosure (pkg-20), is decided by "AI disclosure per repo policy" and does not read this check, so it was not in the canary list; it held at 1/1 on the confirming full run. What the check now gives up: a report that runs the issue's exact steps on a very different platform and names that difference will pass this check even when the platform is the obvious reason the bug did not fire. I accept that, because the report has then done what a stranger can verify, and the named difference is what makes the cannot-reproduce useful to the maintainer. A report that runs the issue's steps on a different platform and does not name it is still caught, by "Target version honored".

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
