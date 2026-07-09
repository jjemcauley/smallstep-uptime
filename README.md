# smallstep-uptime

External uptime probe for `suite.mwprogram.com` (the smallstep box's public front door
— suite down ⇒ every edge-guarded site is down with it).

All of the box's own alerting is on-box, so a network blackout there alerts nobody.
This repo is the off-box dead-man's watch: a scheduled GitHub Actions workflow
(`.github/workflows/probe.yml`) probes the URL every ~5 minutes from GitHub's
infrastructure and messages the existing Telegram alert bot on state changes.

- **Down** (3 failed attempts): one Telegram alert + an `outage` issue opens here.
- **Recovered**: one Telegram alert + the issue closes. The open issue IS the
  down-state — no spam while an outage is ongoing.
- Test the alert path: run the workflow manually (Actions → uptime probe →
  Run workflow) with a known-dead URL override, then again with no override.

Secrets (repo Actions secrets): `TELEGRAM_BOT_TOKEN`, `TELEGRAM_CHAT_ID` — same bot
as the box's monitoring stack (values live in the box's gitignored monitoring `.env`).
