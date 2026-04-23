# 🌱 AgriSmart — Agriculture Advisory Platform

A full-stack microservices application for smart crop recommendations, weather-based advisory, and direct produce marketplace.

## Architecture

```
┌─────────────┐     ┌─────────────────┐     ┌─────────────────────┐
│  Frontend   │     │  User Service   │     │ Crop Advisory Svc   │
│  React+Vite │────▶│  FastAPI:8001   │     │   FastAPI:8002      │
│  nginx:3000 │     │  PostgreSQL     │     │  PostgreSQL+Redis   │
└─────────────┘     └─────────────────┘     └─────────────────────┘
       │                                              │
       │            ┌─────────────────┐               │
       └───────────▶│ Marketplace Svc │◀──────────────┘
                    │  FastAPI:8003   │
                    │  PostgreSQL     │
                    └─────────────────┘
```

## Services

| Service | Port | Database | Description |
|---------|------|----------|-------------|
| User Service | 8001 | userdb | Auth, JWT, profiles |
| Crop Advisory | 8002 | cropdb + Redis | Weather + crop recommendations |
| Marketplace | 8003 | marketdb | Produce listings & orders |
| Frontend | 3000 | — | React SPA via nginx |
| PostgreSQL | 5432 | — | Shared database server |
| Redis | 6379 | — | Caching layer |

## Quick Start

### Prerequisites
- Docker & Docker Compose installed
- (Optional) OpenWeatherMap API key for live weather data

### 1. Configure Environment

Edit `.env` and set your OpenWeatherMap API key:
```bash
OPENWEATHER_API_KEY=your_actual_api_key_here
```

> **Note:** The app works without a valid API key — advisory will use soil-type-only recommendations.

### 2. Build and Run

```bash
docker-compose up --build
```

### 3. Access the Application

| URL | Description |
|-----|-------------|
| http://localhost:3000 | Frontend UI |
| http://localhost:8001/docs | User Service API docs |
| http://localhost:8002/docs | Crop Advisory API docs |
| http://localhost:8003/docs | Marketplace API docs |
| http://localhost:8001/health | User Service health |
| http://localhost:8002/health | Crop Advisory health |
| http://localhost:8003/health | Marketplace health |

### 4. Usage

1. **Register** at http://localhost:3000/register
2. **Login** at http://localhost:3000/login
3. **Dashboard** — View stats, charts, profile
4. **Advisory** — Get crop recommendations by location + soil
5. **Marketplace** — List produce, browse, place orders

## API Endpoints

### User Service (8001)
- `POST /api/v1/users/register` — Register new farmer
- `POST /api/v1/users/login` — Login, returns JWT
- `GET /api/v1/users/profile` — Get profile (protected)
- `PUT /api/v1/users/profile` — Update profile (protected)
- `GET /health` — Health check

### Crop Advisory Service (8002)
- `GET /api/v1/crops/advisory?location=X&soil_type=Y` — Get advisory (protected)
- `GET /api/v1/crops/history` — Advisory history (protected)
- `GET /health` — Health check

### Marketplace Service (8003)
- `POST /api/v1/market/listings` — Create listing (protected)
- `GET /api/v1/market/listings` — Browse listings (public)
- `GET /api/v1/market/listings/{id}` — Single listing (public)
- `DELETE /api/v1/market/listings/{id}` — Delete listing (protected)
- `POST /api/v1/market/orders` — Place order (protected)
- `GET /api/v1/market/orders` — User orders (protected)
- `GET /health` — Health check

## Tech Stack

- **Backend:** FastAPI, SQLAlchemy (async), Alembic, Pydantic v2
- **Auth:** JWT (python-jose), bcrypt (passlib)
- **Frontend:** React 18, Vite, TailwindCSS, Recharts, Axios
- **Database:** PostgreSQL 15, Redis 7
- **Infra:** Docker, Docker Compose, nginx

## Stopping

```bash
docker-compose down
```

To remove all data:
```bash
docker-compose down -v
```
