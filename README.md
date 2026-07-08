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

This local profile gives Confluence a 1 GB minimum heap and 3 GB maximum heap. It also disables Synchrony/collaborative editing JVM flags, caps Synchrony heap at 512 MB if Confluence starts it, disables password confirmation prompts for admin actions, and does not publish port `8091`, which keeps the local setup lighter.

The `upmconfig/truststore` directory is mounted read-only and contains Atlassian's Marketplace app-signing CA certificates. Confluence 10 includes UPM 8, where app signature checks are enabled by default for new app installations.

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
- Docker chooses the native image architecture automatically. On Apple Silicon, the official Confluence image includes an ARM64 variant, which avoids AMD64 emulation.
- The bundled Atlassian Marketplace signing certificates are from `atlassian_ca_bundle-v1.tar.gz`, verified with SHA-256 `373f4142d72eb111333f8bd2bd618cf02ae380f027878e7f0a23fdcd77b9df5a`.
- Do not commit `.env`; it may contain database credentials.
