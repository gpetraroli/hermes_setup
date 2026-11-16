# Hermes

Docker Compose setup for [Nous Research Hermes Agent](https://github.com/NousResearch/hermes-agent). The gateway runs an OpenAI-compatible API and a web dashboard, both bound to localhost. Agent data is stored in `~/.hermes` on the host.

## Setup

Docker and Docker Compose are required.

```bash
cp .env.example .env
```

Edit `.env` before starting. Set a real dashboard password. `.env` is gitignored.

| Variable | Purpose |
| --- | --- |
| `HERMES_API_PORT_BINDING` | Host binding for the API (`127.0.0.1:8642:8642`) |
| `HERMES_DASHBOARD_PORT_BINDING` | Host binding for the dashboard (`127.0.0.1:9119:9119`) |
| `HERMES_DASHBOARD_BASIC_AUTH_USERNAME` | Dashboard username |
| `HERMES_DASHBOARD_BASIC_AUTH_PASSWORD` | Dashboard password |

Optional: `HERMES_UID` and `HERMES_GID` default to `1000` and control the user that owns `~/.hermes`.

## Run

```bash
docker compose up -d
```

- API: http://127.0.0.1:8642
- Dashboard: http://127.0.0.1:9119
