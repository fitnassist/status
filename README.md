# Fitnassist status

Uptime monitoring and the public status page for [Fitnassist](https://www.fitnassist.co),
built on [Upptime](https://upptime.js.org).

**[📈 Live status](https://fitnassist.github.io/status/)**

## Why this is a separate repository

The status page used to live inside the web app, at `fitnassist.co/status`. It
was served by Vercel and it fetched the API on Railway, so it could only ever
report on outages it did not depend on: if Vercel went down the page went with
it, and if Railway went down the page loaded and said everything had failed
without being able to say why.

This runs on GitHub Actions and publishes to GitHub Pages. It shares no
infrastructure with the thing it watches.

## What is checked

| Check | Endpoint | What a failure means |
| --- | --- | --- |
| Website | `www.fitnassist.co` | Vercel. The marketing site and web app are gone; the mobile app is unaffected. |
| API | `api.fitnassist.co/health` | A liveness probe that touches nothing, so a failure means the Railway process itself is not running. |
| Database | `api.fitnassist.co/health/detailed` | Runs `SELECT 1` against Postgres and returns 503 if it cannot. The API can be healthy while Neon is not. |

Checks run every five minutes. Results are committed to `history/`, so the
commit log of each file is the raw record.

## Changing it

Edit `.upptimerc.yml` and nothing else. The workflows under `.github/workflows`
are generated from it and are overwritten when the Upptime template updates.

## Notifications

Deliberately off. GitHub's scheduled workflows are not guaranteed to run on
time and can be delayed under load, which makes this an honest public record
and a poor pager. Alerting is a separate external monitor pointed at the same
endpoints.

## License

The Upptime template is MIT licensed — see [LICENSE](./LICENSE).
