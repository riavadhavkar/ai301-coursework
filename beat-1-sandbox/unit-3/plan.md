# Plan: #62, health check reads `settings.redis_host`, which does not exist on Settings

## Diagnosis

From my reproduction (https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62#issuecomment-5904347204), run on commit `2f4e82f52efbcfcc57d65b3fa5348672163ca088`:

- `docker compose exec redis redis-cli ping` returned `PONG`, so Redis is up.
- `GET /health` returned HTTP 503 with `"redis":"unhealthy"`.
- The server log for that request read: `redis_health_check_failed error="'Settings' object has no attribute 'redis_host'"`.
- `api/routes/health.py` builds its Redis client from `settings.redis_host` (line 47) and `settings.redis_port` (line 48). `Settings` in `core/config.py` only defines `redis_url` (line 14).

The probe asks `Settings` for two attributes it never defined. The `AttributeError` is raised before any connection to Redis is attempted, and the probe's `except Exception` reports it as unhealthy. That matches what I saw: Redis answers, the endpoint says it doesn't.

## Scope

In scope:
- Change the Redis probe in `api/routes/health.py` so it reads its connection details from `redis_url`, the Redis field `Settings` actually has.
- Remove the mypy suppression this bug is tracked under. `docs/CONTRIBUTING.md` and the comment in `pyproject.toml` both name `api/routes/health.py` `attr-defined` as issue #62 and say fixing it should remove the entry. With the `api.routes.health` override taken out, mypy reports `attr-defined` on lines 47-48 (`redis_host`, `redis_port`), which is this bug.
- Add a unit test for the Redis probe, since CONTRIBUTING.md asks for tests with code changes and no health route test exists.

Not in scope:
- The Postgres probe failure (raw SQL string under SQLAlchemy 2.x). That is #61.
- The `vector_db` probe reporting `"healthy"` while its container was down. I noticed it during the repro but did not investigate it.
- `Settings` fields, `.env.example`, `docker-compose.yml`, or the response shape.
- The `index` errors mypy reports on `health_status[...]` assignments in the same file (lines 53-74). They are not caused by the Redis attributes, so that entry stays suppressed.

## Files I will touch

- `api/routes/health.py`, the Redis probe only (around lines 46-51).
- `pyproject.toml`, the `api.routes.health` mypy override: remove `attr-defined`, and `call-overload` too if mypy shows it goes away with the fix. `index` stays.
- `tests/unit/test_health.py`, a new file. I checked `tests/`: there is no health route test today. It calls the `health_check` function directly with a mocked database session and a patched `redis.Redis.from_url`, so it needs no Docker.

## Approach

1. Open `api/routes/health.py` and find the Redis probe (lines 47-48).
2. Replace the `redis_host` / `redis_port` reads with a client built from `settings.redis_url` (`redis.Redis.from_url(settings.redis_url, decode_responses=True)`).
3. Leave the probe's `try/except` and what it returns as they are, so the only thing that changes is where the connection details come from.
4. Remove `attr-defined` from the `api.routes.health` override in `pyproject.toml`, run mypy on `api/routes/health.py`, and remove `call-overload` as well only if mypy stops reporting it.
5. Add `tests/unit/test_health.py` with two cases: ping succeeds so `redis` is `"healthy"`, and ping raises `ConnectionError` so `redis` is `"unhealthy"`.
6. Run the test plan below.

I'm choosing `redis_url` over adding `redis_host` and `redis_port` to `Settings` because `Settings` already defines one Redis setting, and adding two more would give the project two ways to configure the same connection.

## Test plan

I'll re-run my unit 2 reproduction with the same setup: `docker compose up -d`, `uvicorn api.main:app --host 127.0.0.1 --port 8000`, and Redis healthy (`redis-cli ping` returns `PONG`).

Automated: the new unit test, `make test-unit`, `make lint`, and mypy. `make typecheck` fails on unmodified `main` in my local setup (Python 3.12 venv against the repo's `python_version = "3.11"`, in numpy's stubs), so I run `mypy api/ core/ ingestion/ rag/ agent/ safety/ --ignore-missing-imports --python-version 3.12` locally, which passes on `main`, and rely on CI's 3.11 run for the real result.

Before the fix (from my repro comment):
- `curl -s -o health.json -w "HTTP %{http_code}\n" http://127.0.0.1:8000/health` gives `HTTP 503` and `"redis":"unhealthy"`.
- The log has `redis_health_check_failed error="'Settings' object has no attribute 'redis_host'"`.

After the fix:
- `"redis":"healthy"` in `health.json`.
- No `redis_health_check_failed` line in the server log for that request.
- `"postgres":"unhealthy"` and HTTP 503 will still show, because of #61. That is expected and not a failure of this change. I'll judge this fix on the Redis entry and the log only.

Negative check: `docker compose stop redis`, then call `/health` again. I expect `"redis":"unhealthy"` with a connection error in the log instead of the `AttributeError`. That shows the probe now connects to Redis, rather than just no longer erroring.

## Risks and unknowns

- I searched the repo for Redis clients: nothing else builds one (`safety/rate_limiter.py` takes a client as an argument), so there is no existing pattern to match and `redis.Redis.from_url(settings.redis_url, ...)` would be new here.
- `.env.example` has `REDIS_URL=redis://localhost:6379/0` and `docker-compose.yml` publishes the `redis` service on `6379:6379`. Confirmed by running it: after the fix, `/health` shows `"redis":"healthy"` and the log has `redis_health_check_passed`. Stopping Redis gives `Error 61 connecting to localhost:6379. Connection refused.` in the log, not the `AttributeError`.
- Confirmed that `call-overload` goes away with the fix. Mypy is clean with only `index` suppressed for `api.routes.health`, so `pyproject.toml` drops `attr-defined` and `call-overload`. I ran mypy with `--python-version 3.12` locally and have not seen CI's 3.11 run.
- Confirmed that patching `redis.Redis.from_url` works with the function-local `import redis` in `health.py`. Both new tests pass.
- The healthy-case test fails without the fix. The unhealthy-case test passes with or without it, because the old `AttributeError` was also reported as unhealthy. The stopped-Redis run is what shows the probe now connects.
- The overall `/health` status can't reach 200 from this change alone, because Postgres is also failing.
- I only reproduced this on macOS, Apple Silicon, Python 3.12, with the `vector-db` container down. I haven't tested anything else.

## Deviations

Nothing in the plan changed. I touched the three files I named, `api/routes/health.py`, the `api.routes.health` override in `pyproject.toml`, and a new `tests/unit/test_health.py`, and the probe now uses `redis.Redis.from_url(settings.redis_url, decode_responses=True)` as planned. The unknowns I flagged resolved without changing scope: `call-overload` went away along with `attr-defined`, so `pyproject.toml` keeps only `index`; patching `redis.Redis.from_url` works with the function-local `import redis`; and `redis_url` reaches the compose Redis, giving `"redis":"healthy"` after the fix and `Error 61 connecting to localhost:6379. Connection refused.` with Redis stopped. One limit: the unhealthy-case test passes with or without the fix, and only the healthy-case test fails without it, so the stopped-Redis run is what shows the probe connects.
