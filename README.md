# Lumen Railway Installer v23

Standalone public installer hosted on Railway. This folder contains the complete deployable service.

## Deploy

1. Create a Railway service from this repository.
2. Set **Root Directory** to `/railway-installer`.
3. Railway reads `railway.json` and builds the included `Dockerfile`.
4. Generate a public domain after the `/health` check becomes healthy.

No environment variables or stored GitHub/Railway tokens are required. Tokens entered in the form remain request-scoped and are never written to disk or logs.

## Deployment-time network check

At startup the service tests all six embedded HTTP CONNECT proxies against both GitHub and Railway. The fastest proxy that passes both checks is selected. Direct Railway egress is checked only if every proxy fails. `/health` returns HTTP 503 until one complete route is available, so a broken network path cannot produce a falsely successful Railway deployment. The scan repeats every five minutes and runs again for each installation.

- `GET /health` — Railway health check and current route status
- `GET /api/network` — current redacted route report
- `POST /api/network/refresh` — repeat the route scan
