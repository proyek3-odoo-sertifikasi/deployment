# Odoo Docker Setup

<!--toc:start-->

- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Important Configuration](#important-configuration)
- [Project Structure](#project-structure)
- [Services](#services)
- [Common Commands](#common-commands)
- [Reset All Data](#reset-all-data)
- [Configuration](#configuration)
<!--toc:end-->

Simple Odoo 19 setup with PostgreSQL using Docker Compose and bind mounts.

## Prerequisites

- Docker
- Docker Compose

## Quick Start

1. **Copy environment file**

   ```bash
   cp .env.example .env
   ```

2. **Edit `.env`** and update the passwords

3. **Start services**

   ```bash
   docker compose --env-file .env up -d
   ```

   or

   ```bash
   docker-compose --env-file .env up -d
   ```

4. **Access Odoo**
   - URL: <http://localhost:8069>
   - Database: `postgres` (or as configured in `.env`)

## Important Configuration

> [!IMPORTANT]
> This step is REQUIRED. Read carefully

**config/odoo.conf must include database connection settings:**

```ini
[options]
db_host = odoo-db
db_user = odoo
db_password = odoo
```

The database credentials in this file must match your `.env` file. Without these settings, Odoo will fail to connect to the database when creating new databases.

## Project Structure

```
.
├── addons/          # Custom Odoo addons
├── config/          # Odoo configuration files
├── data/            # Persistent data
│   ├── odoo/       # Odoo filestore
│   └── postgres/   # PostgreSQL data
├── .env            # Environment variables (not committed)
├── .env.example    # Environment template
└── docker-compose.yml
```

## Services

| Service  | Image              | Port | Description         |
| -------- | ------------------ | ---- | ------------------- |
| odoo-web | odoo:19            | 8069 | Odoo application    |
| odoo-db  | postgres:17-alpine | 5432 | PostgreSQL database |

## Common Commands

```bash
# Start services
docker compose up -d

# Stop services
docker compose down

# View logs
docker compose logs -f

# Restart Odoo
docker compose restart odoo-web
```

## Reset All Data

> [!WARNING]
> This will delete all Odoo filestore data and PostgreSQL databases. Use with caution.

To completely reset and start fresh:

```bash
# Stop containers
docker compose down -v

# Remove all data directories
rm -rf data/odoo
rm -rf data/postgres

# Restart services
docker compose up -d
```

## Configuration

Edit `.env` to customize:

- `ODOO_PORT`: Web interface port (default: 8069)
- `POSTGRES_PASSWORD`: Database password
- `POSTGRES_USER`: Database user
