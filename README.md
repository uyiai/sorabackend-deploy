# Sora Backend - Docker Deployment

One-command deployment for Sora Backend using pre-built Docker images from GHCR.

## Editions

| Edition | Directory | Modules | Use Case |
|---------|-----------|---------|----------|
| **Base** | `base/` | conversation, billing | Minimal deployment, AI chat only |
| **Pro** | `pro/` | base + video, image, music, creative_studio, agents, sms, support | Full creative suite |
| **Enterprise** | `enterprise/` | pro + referral, wechat, upscale + license server integration | All features, licensed |

## Quick Start

```bash
# 1. Clone this repo
git clone https://github.com/uyiai/sorabackend-deploy.git
cd sorabackend-deploy

# 2. Choose your edition
cd pro  # or base / enterprise

# 3. Create your environment file
cp .env.example .env
# Edit .env — fill in SECRET_KEY, passwords, domain, etc.

# 4. Start all services
docker compose up -d

# 5. Check status
docker compose ps
```

## Services

### Base Edition

| Service | Description | Default Port |
|---------|-------------|-------------|
| `web` | Django API server | 8080 |
| `frontend` | React frontend | 3000 |
| `db` | PostgreSQL 17 | - |
| `redis` | Redis 7 | - |
| `celery-default` | Background task worker | - |
| `celery-beat` | Periodic task scheduler | - |

### Pro & Enterprise Editions

All Base services plus:

| Service | Description |
|---------|-------------|
| `celery-creative-studio` | Creative studio task worker |

### Enterprise Only

Enterprise edition includes license server integration via `LICENSE_KEY`, `LICENSE_SERVER_URL`, and `LICENSE_ENFORCEMENT` environment variables. Place your `license_public.pem` in the `keys/` directory.

## Health Check

```bash
curl http://localhost:8080/up
curl http://localhost:8080/up/detail
```

## Updating

```bash
docker compose pull
docker compose up -d
```

## Data & Backup

Data is persisted in Docker volumes: `postgres_data`, `static_volume`, `media_volume`.

```bash
# Backup database
docker compose exec db pg_dump -U postgres sorabackend > backup.sql
```

## Logs

```bash
docker compose logs -f        # All services
docker compose logs -f web    # Specific service
```
