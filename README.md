SplitStream

An event-driven expense-splitting REST API — built with Django REST Framework. Think Splitwise: users create groups, add expenses, and the API automatically splits costs among group members and tracks who owes what.

Features
JWT Authentication — registration, login, token refresh via djangorestframework-simplejwt
Groups & Membership — create groups, add/remove members, with ownership-based permissions
Expenses — log an expense; the API automatically splits it equally among group members and generates individual ExpenseShare records
Payments — record direct payments between group members
Balances — real-time calculation of who owes whom, using database aggregation
Notifications — per-user notification feed
Dockerized — full stack (Django + PostgreSQL) runs with a single command
CI Pipeline — GitHub Actions automatically installs dependencies and runs tests against a live PostgreSQL service on every push
Tech Stack

Python · Django · Django REST Framework · PostgreSQL · Docker & Docker Compose · GitHub Actions · JWT (SimpleJWT)

Running Locally

Requires only Docker Desktop — no local Python or PostgreSQL install needed.

bash
git clone https://github.com/faridmmdv/splitstream.git
cd splitstream
docker compose up --build

In a second terminal, run migrations:

bash
docker compose exec web python manage.py migrate

The API is now available at http://127.0.0.1:8000/api/.

API Overview
Endpoint	Description
POST /api/users/register/	Create an account
POST /api/users/login/	Obtain JWT access/refresh tokens
GET, POST /api/groups/	List / create groups
POST /api/groups/{id}/add_member/	Add a member to a group
GET, POST /api/expenses/	List / create expenses (auto-splits among members)
GET, POST /api/payments/	List / create payments
GET /api/groups/{id}/balance/	Get balance summary for a group
GET /api/notifications/	List notifications for the logged-in user
Architecture Notes
Money fields use DecimalField (not Float) to avoid floating-point rounding errors.
Sensitive fields (created_by, paid_by) are server-set via perform_create(), never trusted from client input.
Membership and ExpenseShare are explicit "through models" (not plain ManyToManyField) since they carry extra data — join date and owed amount, respectively.
Balance calculations use .aggregate(Sum(...)) for efficient, database-level summation rather than looping in Python.
Roadmap
 Automated test suite (tests.py) across all apps
 Redis caching for balance calculations
 Celery + RabbitMQ for async notification delivery
 CD pipeline (Docker image build/deploy step)
Author

Built by Farid Mammadov
