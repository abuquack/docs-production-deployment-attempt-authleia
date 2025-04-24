# Production Setup Guide

This guide explains how to set up and run the production environment from a fresh clone of this repository.

## 1. Clone the Repository

```
git clone <your-repo-url>
cd <repo-directory>
```

## 2. Restore Environment Files

The project uses several environment files for configuration. Restore them from their `.dist` templates:

```
make create-env-files
```

This will copy the required files into `env.d/development/`.

## 3. Build and Start the Production Stack

Use the provided Makefile rule to build and start all production services:

```
make run-prod
```

This will:
- Build all necessary Docker images
- Start all services defined in `docker-compose.prod.yml`

## 4. Run Database Migrations

After the stack is up, run Django migrations to initialize the database schema:

```
docker compose -f docker-compose.prod.yml exec app python manage.py migrate
```

## 5. Wait for Services to Start

Some services may take a few seconds to become healthy. You can check their status with:

```
docker compose -f docker-compose.prod.yml ps
```

## 6. Access the Services

- **Frontend:** http://localhost:8080 (or your configured domain)
- **y-provider (collaboration server):** ws://localhost:4444
- **API/Backend:** (see nginx config for routes)

## 7. Custom Domains

To use custom domains, update the nginx config in `docker/files/etc/nginx/conf.d/` and point your DNS records to your server's IP.

## 8. Cleaning Up

To remove all containers, networks, and volumes:

```
make down
```

---

**If you encounter issues:**
- Ensure your environment files are present and correct
- Check container logs with `docker compose -f docker-compose.prod.yml logs <service>`
- Ask for help or open an issue 