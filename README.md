# Sora Backend - Docker Deployment

One-command deployment for Sora Backend using pre-built Docker images from GHCR.

## Quick Start

```bash
# 1. Clone this repo
git clone https://github.com/uyiai/sorabackend-deploy.git
cd sorabackend-deploy

# 2. Create your environment file
cp .env.example .env
# Edit .env — fill in SECRET_KEY, passwords, domain, etc.

# 3. Start all services
docker compose up -d

# 4. Check status
docker compose ps
```

## Build Profiles

Three pre-built image profiles are available:

| Profile | Image Tag | Modules |
|---------|-----------|---------|
| **base** | `v1.9.0-base` | conversation, billing |
| **pro** | `v1.9.0-pro` | base + video, image, music, creative_studio, agents, sms, support |
| **enterprise** | `v1.9.0-enterprise` | pro + referral, wechat |

Set your desired profile in `.env`:

```bash
IMAGE_TAG=v1.9.0-pro
FRONTEND_IMAGE_TAG=v1.9.0
```

## Services

| Service | Description | Default Port |
|---------|-------------|-------------|
| `web` | Django API server (Gunicorn + Uvicorn) | 8080 |
| `frontend` | React frontend (Nginx) | 3000 |
| `db` | PostgreSQL 17 | - |
| `redis` | Redis 7 | - |
| `celery-default` | Background task worker | - |
| `celery-creative-studio` | Creative studio task worker | - |
| `celery-beat` | Periodic task scheduler | - |

## Health Check

```bash
# Simple health check
curl http://localhost:8080/up

# Detailed health check (with modules info)
curl http://localhost:8080/up/detail

# Authenticated health check
curl "http://localhost:8080/health/?token=YOUR_HEALTH_CHECK_TOKEN"
```

## Updating

```bash
# Pull new images
docker compose pull

# Restart with new images
docker compose up -d
```

## Data

Data is persisted in Docker volumes:

- `postgres_data` — Database
- `static_volume` — Django static files
- `media_volume` — Uploaded media

To back up the database:

```bash
docker compose exec db pg_dump -U postgres sorabackend > backup.sql
```

## Logs

```bash
# All services
docker compose logs -f

# Specific service
docker compose logs -f web
```
