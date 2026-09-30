# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong label is not graded.

---

## Your identity upstream

**GitHub username**

riavadhavkar

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62#issuecomment-5904070863

Hi, I'd like to work on issue #62 (settings.redis_host doesn't exist on Settings) as my Unit 2 reproduction.

I'll investigate whether the Redis probe in api/routes/health.py is reading a field Settings doesn't define, and post a reproduction report with my environment, steps, and actual output whether it is confirmed or not. Not promising a fix or a timeline.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62#issuecomment-5904347204

Reproduced on commit `2f4e82f52efbcfcc57d65b3fa5348672163ca088` (current `main`).

**Environment:** macOS 26 (Darwin 27.0.0), Apple Silicon (arm64), Python 3.12.14 (venv via `python3 -m venv`), Docker 29.8.0, Docker Compose v5.5.1. Backing services from the repo's own `docker-compose.yml`: `postgres:16-alpine` and `redis:7-alpine`. Installed package versions: fastapi 0.142.1, uvicorn 0.54.0, redis-py 8.1.0, pydantic-settings 2.15.0.

Two deviations from `docs/SETUP.md`, neither of which touches the Redis probe:

1. I ran the backend only (`uvicorn api.main:app --host 127.0.0.1 --port 8000`) instead of `make run`, since reaching `/health` doesn't need the frontend dev server.
2. The `vector-db` (ChromaDB) container failed to start — its `chromadb/chroma:0.4.22` image hits `AttributeError: np.float_ was removed in the NumPy 2.0 release` on startup, an unrelated image/dependency incompatibility, not something in this repo's code. `postgres` and `redis` both started and reported healthy.

**Steps:**

```bash
cp .env.example .env                  # defaults untouched: LLM_PROVIDER=mock, no API key needed
docker compose up -d                  # db + redis healthy; vector-db exited (see above)
python3 -m venv .venv
.venv/bin/pip install -e ".[dev]"
.venv/bin/alembic upgrade head        # 001 -> 002, applied cleanly
.venv/bin/uvicorn api.main:app --host 127.0.0.1 --port 8000
```

Confirmed Redis is actually reachable, independent of the app:

```
$ docker compose exec redis redis-cli ping
PONG
```

Called the health endpoint:

```
$ curl -s -o health.json -w "HTTP %{http_code}\n" http://127.0.0.1:8000/health
HTTP 503
$ cat health.json
{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"},"safety_events_last_hour":0,"timestamp":"2026-09-30T04:41:51.383486"}}
```

Server log for that same request:

```
2026-09-30 00:41:51 [error] postgres_health_check_failed error="Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')"
2026-09-30 00:41:51 [error] redis_health_check_failed error="'Settings' object has no attribute 'redis_host'"
2026-09-30 00:41:51 [debug] vector_db_health_check_passed
```

**This matches the issue exactly:** Redis answers `PONG`, but `/health` reports `"redis": "unhealthy"`, and the log shows the same `AttributeError` on `redis_host` the issue describes.

**Root cause, read from the source:** `api/routes/health.py` builds its Redis client from `settings.redis_host` (line 47) and `settings.redis_port` (line 48). `Settings` in `core/config.py` only defines `redis_url` (line 14). The attribute access raises `AttributeError`, which the probe's `except Exception` catches and reports as `"redis": "unhealthy"` — exactly the log line above.

**Caveats, not part of this claim:**
- `postgres` is also reported unhealthy in the same response, from the separate, unrelated raw-SQL issue tracked as #61.
- The response reports `"vector_db": "healthy"` even though the `vector-db` container wasn't running (see deviation 2 above) — the probe apparently doesn't actually detect this. Noting it only so it isn't mistaken for evidence about this issue; I didn't investigate it further since it's out of scope for #62.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run (no `--save-run`): **17/20 scored items** (bar: 18/20 — below the bar). `categories: clear-accept 5/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`. Disagreements: pkg-09 and pkg-10 (gold `accept`, graded `reject`, failed `behavior matches`) and pkg-12 (gold `accept`, graded `reject`, failed `environment recorded`).
2. Between runs, I revised two required checks in `rubric.md`/`evidence-guide.md`: `behavior matches` now also passes an honest, evidenced cannot-reproduce attempt (pkg-09/pkg-10 were genuine, well-evidenced cannot-reproduce reports my original wording could never pass), and `environment recorded` now asks for OS + stack-relevant language/runtime/tool versions instead of Apple-platform-only fields (pkg-12 is a Node/Prettier package with no Xcode/device to name).
3. Partial re-run with `--only pkg-09,pkg-10,pkg-12,pkg-20` (pkg-20 added as a canary from the `disclosure` category, the one single-package category, since both revised checks could plausibly have let a bad package through): **4/4 agree**. pkg-09, pkg-10, pkg-12 flipped to `accept` (matching gold); pkg-20 correctly held at `reject` (its disclosure-wall failure is untouched by either revision). Partial runs print no bar or floor.
4. Confirming full run with `--save-run eval-run.txt`: **20/20 scored items** (bar: 18/20: **PASS**). `categories: clear-accept 8/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`. This is the run committed as `eval-run.txt`.

**Package analysis**

`pkg-09` (source: `sharkdp/fd#2033`, category `clear-accept`). My rubric's final verdict: `accept`. Gold label: `accept` ("honest cannot-reproduce: real attempt at the argument-size reordering with marker-order artifacts, names what differed... and what a triggering setup likely needs").

My rubric reads it this way because the candidate report is a genuine, well-evidenced attempt: it runs the issue's exact `--exec-batch` ordering scenario five times (plus a padded variant) with a fully recorded environment (Arch Linux, fd 10.4.2, `ARG_MAX` value), pastes the actual marker-order output from each attempt, and — rather than asserting the bug is absent — names precisely what differed from the issue's conditions (uniform file-name lengths vs. the varying lengths the reordering likely needs to trigger). That satisfies `honest outcome` outright. It only satisfies the required `behavior matches` check because of a fix I made after my first full run: my original pass condition demanded the artifact show the *same* error/crash, which a true cannot-reproduce can never do by definition, so it failed pkg-09 regardless of how honest or thorough the attempt was. I added an explicit OR clause — a genuine attempt at the issue's own trigger steps, with what differed named — so an honest failed reproduction is graded on the quality of the attempt, not on whether the bug happened to show up.

**Check rationale**

Quoted exactly from `rubric.md` as uploaded to `tools/repro-check/`, the `behavior matches` row's pass condition:

> the artifact shows the same error type or symptom the issue describes, produced by following the issue's own stated trigger steps, not an adjacent path — OR, when the report honestly states it could not reproduce, it shows a genuine attempt using the issue's own trigger steps and names what differed from the issue's conditions (an evidenced cannot-reproduce is a pass here, not a fail)

It reads this way because my first full eval run scored 17/20 and missed the entire top half of the `clear-accept` category on two packages (pkg-09, pkg-10) that were honest, fully-evidenced cannot-reproduce reports — real environment, real steps run against the issue's exact trigger, real output, and a specific account of what differed. My original wording only checked whether the artifact showed the *same* behavior as the issue, which is structurally impossible for a cannot-reproduce report to pass, no matter how well-evidenced it is. That directly contradicted the course's own stated rule that an honest, evidenced "cannot reproduce" is a valid pass, so I rejected the single-condition wording and added the OR clause, gating it on the attempt being genuine (uses the issue's own trigger steps) and specific (names what differed) — not just any unsupported "I couldn't repro it."

**Trade-offs**

Loosening `behavior matches` to accept an honest cannot-reproduce gives up some strictness: a rubric that grades cannot-reproduce reports on attempt quality rather than outcome could, in principle, let through a lazy "I tried and it didn't happen" with no real effort behind it. I accept that risk rather than the alternative (rejecting every honest cannot-reproduce, which is what my original wording did and which the course's own rules say is wrong), and I mitigated it by requiring the attempt be genuine (the issue's own trigger steps, not an easier path) and specific (names what differed), not just any unsupported claim of failure.

I checked this didn't quietly break the one category most exposed to over-acceptance: after the revision, I re-ran `--only pkg-09,pkg-10,pkg-12,pkg-20`, adding pkg-20 as a canary — the sole package in the one-package `disclosure` category, and gold's own "excellent repro on every proof check" case, meaning it would already pass `behavior matches` and `environment recorded` and could only still be rejected by `comms match repo conventions` correctly catching its missing AI-disclosure. It held at `reject`, confirming the loosened checks didn't spill over into the check that was supposed to catch it.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
