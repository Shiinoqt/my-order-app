# Orders Platform

A microservices-based order management platform. This repository ties all individual services together using Docker Compose and Git submodules.

***

## Services

| Service | Description | Repository |
|---|---|---|
| `auth-service` | Authentication & authorization (JWT, sessions) | [auth-service](./auth-service) |
| `notification-service` | Email / push notification delivery | [notification-service](./notification-service) |
| `order-service` | Core order lifecycle management | [order-service](./order-service) |
| `orders-gateway` | API gateway & request routing | [orders-gateway](./orders-gateway) |
| `payment-service` | Payment processing & transaction handling | [payment-service](./payment-service) |

***

## Project Structure

```
orders-platform/
├── auth-service/            # submodule
├── notification-service/    # submodule
├── order-service/           # submodule
├── orders-gateway/          # submodule
├── payment-service/         # submodule
├── docker-compose.yml       # orchestrates all services
├── .env.example             # environment variable template
└── init.sql                 # database initialization script
```

***

## Getting Started

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) & [Docker Compose](https://docs.docker.com/compose/install/)
- [Git](https://git-scm.com/)

### 1. Clone with submodules

```bash
git clone --recursive https://github.com/your-username/orders-platform.git
cd orders-platform
```

If you already cloned without `--recursive`:

```bash
git submodule update --init --recursive
```

### 2. Configure environment variables

```bash
cp .env.example .env
```

Open `.env` and fill in your values (database credentials, JWT secret, API keys, etc.).

### 3. Start all services

```bash
docker compose up --build
```

To run in detached mode:

```bash
docker compose up --build -d
```

### 4. Stop services

```bash
docker compose down
```

To also remove volumes (wipes database data):

```bash
docker compose down -v
```

***

## nvironment Variables

Copy `.env.example` to `.env` and configure the following:

| Variable | Description | Example |
|---|---|---|
| `DB_HOST` | Database host | `postgres` |
| `DB_PORT` | Database port | `5432` |
| `DB_NAME` | Database name | `orders_db` |
| `DB_USER` | Database user | `postgres` |
| `DB_PASSWORD` | Database password | `changeme` |
| `JWT_SECRET` | Secret key for JWT signing | `supersecretkey` |
| `AUTH_SERVICE_URL` | Internal URL of auth service | `http://auth-service:3000` |
| `PAYMENT_SERVICE_URL` | Internal URL of payment service | `http://payment-service:3001` |
***

## Docker Compose Overview

All services are orchestrated via `docker-compose.yml`. The `orders-gateway` is the public-facing entry point; all other services communicate internally over the Docker network.

```
Client → orders-gateway → auth-service
                        → order-service → payment-service
                                        → notification-service
```

***

## Database

The `init.sql` file is automatically run on first startup to seed the database schema and any initial data. If you change it after the first run, you'll need to reset the volume:

```bash
docker compose down -v
docker compose up --build
```

***

## Updating Submodules

To pull the latest commits from all service repos:

```bash
git submodule update --remote
git add .
git commit -m "chore: update submodules to latest"
git push
```

To update a single service:

```bash
git submodule update --remote auth-service
```

***
