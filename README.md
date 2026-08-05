# SplitStream

**An event-driven expense-splitting REST API, built with Django REST Framework.**

Think Splitwise: users create groups, log shared expenses, and the API automatically splits the cost among group members, tracks individual balances, and calculates who owes whom.

![Python](https://img.shields.io/badge/Python-3.11-blue)
![Django](https://img.shields.io/badge/Django-5.2-092E20)
![DRF](https://img.shields.io/badge/DRF-3.17-red)
![Docker](https://img.shields.io/badge/Docker-ready-2496ED)
![CI](https://github.com/faridmmdv/splitstream/actions/workflows/ci.yml/badge.svg)

---

## Overview

SplitStream models a real-world problem — shared group expenses — with proper relational design and business logic, rather than being a generic CRUD tutorial project. Every design decision below was made deliberately, not defaulted to.

## Features

- 🔐 **JWT Authentication** — registration, login, and token refresh via `djangorestframework-simplejwt`
- 👥 **Groups & Membership** — create groups, add/remove members, with ownership-scoped permissions
- 💰 **Expenses** — log an expense once; the API automatically splits it evenly across group members and generates individual `ExpenseShare` records
- 💸 **Payments** — record direct settlements between two group members
- 📊 **Balances** — real-time "who owes whom" calculation using database-level aggregation
- 🔔 **Notifications** — per-user notification feed
- 🐳 **Fully Dockerized** — the entire stack (Django + PostgreSQL) runs with one command, no local Python/Postgres install required
- ⚙️ **CI Pipeline** — GitHub Actions automatically installs dependencies and runs the test suite against a live PostgreSQL service on every push

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Python 3.11 |
| Framework | Django 5.2 + Django REST Framework |
| Database | PostgreSQL 15 |
| Auth | JWT (SimpleJWT) |
| Infrastructure | Docker, Docker Compose |
| CI/CD | GitHub Actions |

## Getting Started

The only requirement is [Docker Desktop](https://www.docker.com/products/docker-desktop/) — no local Python, pip, or PostgreSQL installation needed.

```bash
# 1. Clone the repo
git clone https://github.com/faridmmdv/splitstream.git
cd splitstream

# 2. Build and start everything (Django + PostgreSQL)
docker compose up --build
```

In a second terminal, apply migrations:

```bash
docker compose exec web python manage.py migrate
```

The API is now live at **`http://127.0.0.1:8000/api/`**

## API Reference

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/users/register/` | Create an account |
| `POST` | `/api/users/login/` | Obtain JWT access + refresh tokens |
| `POST` | `/api/users/login/refresh/` | Refresh an expired access token |
| `GET, POST` | `/api/groups/` | List / create groups |
| `POST` | `/api/groups/{id}/add_member/` | Add a member to a group |
| `POST` | `/api/groups/{id}/remove_member/` | Remove a member from a group |
| `GET` | `/api/groups/{id}/balance/` | Balance summary for a group |
| `GET, POST` | `/api/expenses/` | List / create expenses — auto-splits among group members |
| `GET, POST` | `/api/expense-shares/` | View individual expense shares |
| `GET, POST` | `/api/payments/` | List / create payments |
| `GET` | `/api/notifications/` | List notifications for the current user |

Full request/response schema available via the DRF browsable API when running locally.


## Project Structure

```
splitstream/
├── config/            # Project settings, root URL config
├── users/             # Auth: registration, JWT login/refresh
├── groups/            # Groups and membership management
├── expenses/          # Expenses and auto-generated shares
├── payments/          # Direct payments between members
├── balances/          # Balance calculation endpoint
├── notifications/     # Per-user notifications
├── Dockerfile
├── docker-compose.yml
└── .github/workflows/ci.yml
```

## Roadmap

- [ ] Automated test suite (`APITestCase`) across all apps
- [ ] Redis caching for balance calculations
- [ ] Celery + RabbitMQ for async notification delivery
- [ ] CD pipeline — build & publish Docker image
- [ ] Deployment (AWS)

## Author

Built by [Farid Mammadov](https://github.com/faridmmdv) 
