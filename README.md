# Confluence Docker

This repository runs a standard Atlassian Confluence container with PostgreSQL.

## Prerequisites

- Docker
- Docker Compose, either the classic `docker-compose` command or the newer `docker compose` plugin
- At least 2 GB of memory available for Confluence
- A valid Atlassian Confluence license or evaluation license for setup

## First Run

Create a local environment file:

```bash
cp .env.example .env
```

Edit `.env` and change `POSTGRES_PASSWORD`.

Start Confluence:

```bash
docker-compose up -d
```

If your Docker install uses the newer Compose plugin, this equivalent command also works:

```bash
docker compose up -d
```

Open Confluence at:

```text
http://localhost:8090
```

The first boot can take several minutes. Confluence data is stored in the `confluence-home` Docker volume, and PostgreSQL data is stored in the `postgres-data` Docker volume.

## Useful Commands

View logs:

```bash
docker-compose logs -f confluence
```

Stop the stack:

```bash
docker-compose down
```

Stop and remove all local Confluence/PostgreSQL data:

```bash
docker-compose down -v
```

## Notes

- The official image is `atlassian/confluence`. The older `atlassian/confluence-server` name is deprecated and kept only for compatibility.
- `CONFLUENCE_PLATFORM` defaults to `linux/amd64`, which is the safest default for Atlassian's image on Apple Silicon Macs. Remove or change it in `.env` if your Docker host does not need it.
- Do not commit `.env`; it may contain database credentials.
