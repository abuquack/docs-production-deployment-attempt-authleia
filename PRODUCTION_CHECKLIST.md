# Production Dockerization Checklist

Track your progress as you prepare your project for production-ready, self-contained Docker deployment.

## 1. Docker Compose
- [x] Create `docker-compose.prod.yml` with all services, no host volume mounts, and only production-ready services
- [x] Remove all bind/host mounts from production compose file
- [x] Use environment variables or Docker secrets for all sensitive values

## 2. Dockerfiles
- [x] Backend: COPY all code, configs, and static assets into image
- [x] Frontend: COPY all build artifacts and configs into image
- [x] Nginx: COPY all config files and static assets into image
- [x] y-provider: COPY all code and dependencies into image
- [x] Authelia: COPY config, set up for env vars or Docker secrets

## 3. Build & Tag Images
- [x] Build backend production image (`impress:backend-production`)
- [x] Build frontend production image (`impress:frontend-production`)
- [x] Build nginx production image (`impress:nginx-production`)
- [x] Build y-provider production image (`impress:y-provider-production`)
- [x] Build authelia production image (`impress:authelia-production`)
- [ ] Push images to registry (if applicable)

## 4. Secrets & Configuration
- [x] Set all required environment variables for production (`.env.production` created and used)
- [x] Authelia storage encryption key error resolved by:
  - Ensuring all containers, images, and build cache were pruned
  - Explicit `docker build` for Authelia image to guarantee a fresh writable layer
  - Confirming no stale db.sqlite3 in build context
  - Verified successful Authelia startup and schema migration
- [ ] (Optional) Set up Docker secrets for Authelia if using secret files

## 5. Deployment
- [x] Run stack with `docker compose --env-file .env.production -f docker-compose.prod.yml up -d`
- [x] Verify all services start and run without local file dependencies

## 6. Documentation
- [ ] Update README to reflect new production workflow, image names, and environment/config requirements

---

## Deployment Issues & Solutions

- **Issue:** All containers were failing initially due to missing environment variables (secrets, DB credentials, etc.).
  - **Solution:** Generated a `.env.production` file with required variables and restarted the stack.

- **Issue:** Authelia failed to start due to storage encryption key mismatch ("the configured encryption key does not appear to be valid for this database").
  - **Solution:** As this is a new deployment, we reset all Docker volumes with `docker compose down -v` and restarted the stack, allowing Authelia to initialize fresh with the provided key.

- **Issue:** Nginx container was missing from `docker ps` and failed to start because its dependency (Authelia) was unhealthy.
  - **Solution:** Reset all Docker volumes and containers again to ensure a clean state for Authelia and Nginx, then restarted the stack with all services.

- **Issue:** App container failed due to missing `DJANGO_SETTINGS_MODULE` environment variable.
  - **Solution:** Will update `.env.production` and Docker Compose to ensure all required Django environment variables are set for the backend, then restart the stack.

- **Result:** All containers (including backend, frontend, celery, mailcatcher, minio, postgresql, redis, y-provider) are now up and healthy using only the production compose file and no local file dependencies. Authelia is also now running as expected.

Check off each item as you complete it to ensure a smooth, reproducible production deployment!
