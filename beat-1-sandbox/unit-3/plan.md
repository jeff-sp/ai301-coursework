# Plan: a curl example under each endpoint in docs/API.md (#47)

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/47
Repro this plan builds on: my repro comment on the issue,
https://github.com/codepath/pathreview-ai301-fa26-s3/issues/47#issuecomment-5860025720,
run on commit `2f4e82f` (Windows 11, Git Bash, curl 8.17.0, Python 3.12 venv, Docker Desktop 29.8.0).

## Diagnosis

`docs/API.md` describes each endpoint in one line and never shows a request. My repro pinned that on commit `2f4e82f`:

```
$ grep -c -i curl docs/API.md
0
$ grep -c '^```' docs/API.md
0
$ grep -c -E '^`(GET|POST|PUT|PATCH|DELETE) ' docs/API.md
9
```

The reason a one-line description is not enough is that the nine endpoints take three different body shapes and the descriptions do not say which. From my repro's Actual line: "register takes a JSON body, login takes an OAuth2 form body (`username`, not `email`), and profiles takes multipart form fields, so the one-line descriptions alone are not enough to write a first call." I only found those shapes by reading `api/routes/`. Another repro on the thread shows what happens to a reader who guesses: JSON with `email` sent to `/auth/login` gets a 422 "Field required" for `username`.

So the cause is missing text in `docs/API.md`: no request example, and no statement of the body shape, under any endpoint. The API itself answered every documented call in my repro (one 503, seven 200s, one 204), so this is a documentation gap and not an API bug.

## Scope

**In scope:** `docs/API.md` only. One fenced code block under each of the nine documented endpoints, containing the `curl` command that worked in my repro and, as comment lines in the same block, the HTTP status and a trimmed response body. One short "Running the examples" paragraph near the top of the file saying the examples assume the local server from `docs/SETUP.md` on `http://localhost:8000`, that `<token>` comes from `POST /auth/login` (or the register response), and that `<profile_id>` and `<review_id>` are the ids returned by the create calls.

**Not in scope:**

- The `/health` 503. My repro's uvicorn log shows two health-check code bugs (`postgres_health_check_failed ... 'SELECT 1' should be explicitly declared as text`, `redis_health_check_failed ... 'Settings' object has no attribute 'redis_host'`). Those live in `api/routes/health.py` and the settings module, not in the docs. The `/health` example will show the 503 the API returns today, with a one-line note that it is a separate bug.
- The two routes in `api/routes/` that `docs/API.md` does not list (`PUT /profiles/{profile_id}`, `GET /reviews/{review_id}/status`). Whether they belong in the doc is an open question on the thread that no maintainer has answered. I will not add them unless one says so.
- The `vector-db` container's NumPy crash and the `seed_db.py` cp1252 encoding error, both noted in my repro as "not part of this issue".
- The "Interactive Docs" section and the file's heading structure, which stay as they are.
- Any change outside `docs/API.md`. No code, no tests, no CI.

## Files

- `docs/API.md` (the only file this plan touches)

## Approach

1. After the `Base URL` line, add a "Running the examples" paragraph: start the stack per `docs/SETUP.md` (or `docker compose up -d` plus `uvicorn api.main:app --host 127.0.0.1 --port 8000`), seeded login is `user1@example.com` / `password1` from `scripts/seed_db.py`, placeholders `<token>`, `<profile_id>`, `<review_id>`.
2. Keep the existing order (Health, Authentication, Profiles, Reviews). Under each endpoint's description line, add one ```` ```bash ```` block. The first line is the `curl` command as I ran it in the repro (no `$` prompt, so it pastes cleanly). Below it, lines starting with `#` give the HTTP status and the response body trimmed to its shape, with JWTs as `<token>` and UUIDs as `<profile_id>` / `<review_id>` where they appear in the request.
3. Under `/health`, one `#` line: on a fresh setup this returns 503 because of a health-check bug that is tracked separately; the rest of the API works.
4. Under `/auth/login`, one `#` line: form body with `username`, not JSON with `email`.
5. Under `POST /profiles`, one `#` line: multipart form fields, `-F`, not JSON.
6. Commit as `docs(api): add curl example for each documented endpoint`, `Fixes #47`, per `docs/CONTRIBUTING.md`.

## Test plan

My repro's doc check, re-run on the branch. Before (from the repro, commit `2f4e82f`): 0 curl lines, 0 fence lines, 9 endpoint lines. After, I expect:

```
grep -c '^curl ' docs/API.md                                 # 9, one command per endpoint
grep -c '^```' docs/API.md                                   # 18, one open and one close per endpoint
grep -c -E '^`(GET|POST|PUT|PATCH|DELETE) ' docs/API.md      # 9, unchanged
```

Then my repro's steps 1 to 3 (compose up, venv and migrations and seed, uvicorn on 127.0.0.1:8000), and every fenced command from the new file pasted as written, with `<token>` and the ids substituted. Each must return the status my repro recorded:

| Example | Expected status |
|---|---|
| `GET /health` | 503 (the known health-check bug; body names postgres and redis unhealthy) |
| `POST /auth/register` | 200, body `{"access_token":"...","token_type":"bearer"}` |
| `POST /auth/login` | 200, same body shape |
| `POST /profiles` | 200, body with `id`, `github_username`, `portfolio_url` |
| `GET /profiles/{profile_id}` | 200, same body |
| `POST /reviews` | 200, body with `status":"pending"` |
| `GET /reviews/{review_id}` | 200, `status":"complete"` with three sections |
| `GET /reviews?page=1&page_size=20` | 200, body with `items`, `total`, `page`, `page_size` |
| `DELETE /profiles/{profile_id}` | 204, empty body |

CI: the change is Markdown only, so `lint`, `typecheck`, `test-unit`, `test-integration`, and `frontend` are not touched. I will still confirm the five jobs are green on the PR, since `docs/CONTRIBUTING.md` requires it.

## Risks and unknowns

- The `/health` example shows a 503. A reader may read that as a broken setup. The one-line note is the mitigation; whether a maintainer would rather drop `/health` from the examples is an open question I will ask in the PR.
- Running `POST /auth/register` twice with the same email: `api/routes/auth.py` raises `HTTPException(status_code=400, detail="Email already registered")` on a duplicate. I have read that but not run it; I will run it during the build and put the 400 in the example's comment line so a reader who re-runs the example is not surprised.
- Response bodies carry timestamps, UUIDs, and scores that differ on every run. The examples show the shape, not exact values, and the note says so.
- The examples are written for bash (Git Bash on Windows, or any Linux or macOS shell). I have not tested them in PowerShell or cmd.exe, where the single quotes would need changing.
- Whether maintainers want the two unlisted routes documented is unknown; if they answer yes on the thread before the PR, that becomes a deviation recorded below.

## Deviations

The build followed the plan: one file, `docs/API.md`, one fenced `bash` block under each of the nine documented endpoints, the "Running the examples" paragraph, the body-shape notes under `/auth/login` and `POST /profiles`, and the one-line `/health` note. The test plan ran as written: the greps went from 0 / 0 / 9 to 9 / 18 / 9, and all nine examples pasted from the new file returned the statuses in the table above (503, then seven 200s, then 204). Three small things differ from the plan as posted:

1. The register example uses `newuser@example.com` / `a-strong-password` instead of the `repro47@example.com` address from my repro, so a reader copying it does not collide with an account that already exists in my database. Same command shape, different literal.
2. I added one note under `DELETE /profiles/{profile_id}` that deleting a profile removes its reviews, so a reader walking the file top to bottom should run the Reviews examples first. The plan kept the file's existing order (Profiles before Reviews) and did not anticipate that the delete example sits above the review examples.
3. The duplicate-register case is now confirmed, not just read: running the register example twice returned `HTTP 400 {"detail":"Email already registered"}`, and that line is under the example as the plan said it would be. The plan listed it as read-but-not-run.

Nothing was removed from scope and nothing was added outside `docs/API.md`. The unlisted routes, the `/health` code bug, and the vector-db and seed_db issues stayed out, as planned. No maintainer answered on the thread before the build, so the plan comment as posted is still true and no follow-up comment is needed.
