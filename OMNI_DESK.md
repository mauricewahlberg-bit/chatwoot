# Omni Desk V1 (Chatwoot CE)

Private demo: Zendesk-style agent console + website chat widget (MyRepublic Broadband SG, Customer Journey). Public fork of Chatwoot.

- Fork: https://github.com/mauricewahlberg-bit/chatwoot
- **MIT Community Edition only.** Never import, copy, call, or depend on `enterprise/`.
- Docker image: `chatwoot/chatwoot:latest-ce`
- V1 data: synthetic only. Channel: website widget only. No live customer data.

## Run

```sh
cp .env.example .env
# set POSTGRES_PASSWORD, REDIS_PASSWORD, SECRET_KEY_BASE, FRONTEND_URL=http://localhost:3000
# add OMNI_DESK_V1=1
# Compose reads POSTGRES_PASSWORD and REDIS_PASSWORD from .env — do not hardcode secrets.

docker compose -f docker-compose.omni-desk.yaml run --rm rails bundle exec rails db:chatwoot_prepare
docker compose -f docker-compose.omni-desk.yaml up -d
curl -I http://localhost:3000/api
# expect HTTP 200
```

## PRs

1. this — CE compose, runbook, and CE-gate CI
2. seed Contact attrs: `plan`, `tier` (`gamer` | `standard` | `fibre`), `account_status` (`active` | `pending_install` | `recontract`)
3. agent context strip for those Contact attrs

Guard: block merge unless the Chatwoot image is `*-ce`, the compose file exists, and the diff has zero `enterprise/` paths.
