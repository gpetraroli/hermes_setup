# Hermes

Docker Compose setup for [Nous Research Hermes Agent](https://github.com/NousResearch/hermes-agent). The gateway runs an OpenAI-compatible API and a web dashboard, both bound to localhost. Agent data is stored in `~/.hermes` on the host.

## Setup

Docker and Docker Compose are required.

```bash
cp .env.example .env
```

Edit `.env` before starting. Set a real dashboard password and a signing secret. `.env` is gitignored.

| Variable | Purpose |
| --- | --- |
| `HERMES_API_PORT_BINDING` | Host binding for the API (`127.0.0.1:8642:8642`) |
| `HERMES_DASHBOARD_PORT_BINDING` | Host binding for the dashboard (`127.0.0.1:9119:9119`) |
| `HERMES_DASHBOARD_BASIC_AUTH_USERNAME` | Dashboard username |
| `HERMES_DASHBOARD_BASIC_AUTH_PASSWORD` | Dashboard password |
| `HERMES_DASHBOARD_BASIC_AUTH_SECRET` | Signing key for dashboard session cookies |

Generate the secret once and keep the same value. Hermes signs session cookies with it, so a new value logs everyone out. If the variable is empty, Hermes creates a new key on every start and sessions do not survive a restart.

```bash
openssl rand -base64 32
```

Put the output in `HERMES_DASHBOARD_BASIC_AUTH_SECRET`.

Optional: `HERMES_UID` and `HERMES_GID` default to `1000` and control the user that owns `~/.hermes`.

## Run

```bash
docker compose up -d
```

- API: http://127.0.0.1:8642
- Dashboard: http://127.0.0.1:9119

## Extra folders

`docker-compose.yaml` mounts only `~/.hermes`. To share more host folders with the container, add a `docker-compose.override.yml` next to it. Compose merges that file automatically, and appends its volumes to the main one. Add or remove any number of lines there; leave the main file unchanged.

```yaml
services:
  hermes:
    volumes:
      - ~/hermes-shared/editable:/workspace/editable:rw
      - ~/hermes-shared/readonly:/workspace/readonly:ro
      - ~/projects/app1:/workspace/app1:ro
      - ~/projects/sandbox:/workspace/sandbox:rw
```

Apply the list with:

```bash
docker compose up -d
```

Confirm the folders are visible inside the container:

```bash
docker exec hermes ls -la /workspace
```
