# Mami Supermarket – Full-Stack Grocery Delivery Platform

deployed on render go check my website -
https://mami-supermarket-frontend.onrender.com/

Modern online supermarket system with customer-facing shopping experience and internal operational tools for employees, managers, and administrators.

**Customer features** · Cart · Checkout · Order tracking · Wishlist  
**Operations features** · Order picking · Inventory management · Stock requests · Catalog administration · Analytics · User & branch management

## Project Structure (Monorepo / Separate Repos)
mami-supermarket/
├── back/                    # Flask + PostgreSQL REST API
├── front/                   # React + Vite + TypeScript SPA
└── README.md                   # ← this file


Alternatively the two parts can live in separate repositories:

- `mami-supermarket-backend`
- `mami-supermarket-frontend`

## Architecture Overview

**Backend**  
Flask · PostgreSQL · SQLAlchemy · JWT · Pydantic · Alembic migrations  
→ production-grade REST API with role-based authorization

**Frontend**  
React 19 · TypeScript · Vite · Tailwind CSS · Zustand + Context · React Router · Zod + React Hook Form  
→ responsive SPA with separate customer & operations interfaces

**Communication**  
Frontend ↔ Backend via REST + JWT Bearer tokens  
API base path: `/api/v1/`

**User Roles & Permissions**

| Role      | Shopping | Order Picking | Inventory | Catalog Mgmt | Delivery Slots | Analytics | User Mgmt | Audit Logs |
|-----------|----------|---------------|-----------|--------------|----------------|-----------|-----------|------------|
| CUSTOMER  | ✓        |               |           |              |                |           |           |            |
| EMPLOYEE  |          | ✓             | ✓         |              |                |           |           |            |
| MANAGER   |          | ✓             | ✓         | ✓            | ✓              | ✓         |           | ✓          |
| ADMIN     |          | ✓             | ✓         | ✓            | ✓              | ✓         | ✓         | ✓          |

## Tech Stack – Unified View

| Layer              | Technology                              | Purpose                              |
|--------------------|-----------------------------------------|--------------------------------------|
| **Backend**        | Python 3.11+, Flask 3.1+                | Web framework                        |
| **API Validation** | Pydantic 2.x                            | Request / response schemas           |
| **Database**       | PostgreSQL 13+ · SQLAlchemy 2.0         | ORM & query building                 |
| **Migrations**     | Alembic                                 | Schema versioning                    |
| **Auth**           | Flask-JWT-Extended · bcrypt             | JWT + password hashing               |
| **Email**          | Brevo (Sendinblue)                      | Transactional emails                 |
| **Frontend**       | React 19 · TypeScript 5.6+ · Vite 6     | UI & build tool                      |
| **Styling**        | Tailwind CSS 4 (alpha)                  | Utility-first CSS                    |
| **State**          | Zustand 5 + React Context               | Global & feature state               |
| **Forms**          | React Hook Form 7.5 + Zod 3.2           | Form handling & validation           |
| **HTTP**           | Axios 1.7                               | API client                           |
| **UI**             | Radix UI · Lucide icons · react-hot-toast | Primitives, icons, notifications   |
| **Testing**        | pytest (BE) · Vitest + RTL (FE)         | Unit / integration tests             |

## Quick Start – Development

### Backend

```bash
cd backend
python -m venv venv
source venv/bin/activate    # or venv\Scripts\activate on Windows

pip install -r requirements.txt

# Create .env (see backend README for required variables)
cp .env.example .env
# → edit DATABASE_URL, JWT_SECRET_KEY, ...

alembic upgrade head

# Optional: seed sample data
python scripts/seed/seed_initial_data.py

python run.py
# → http://localhost:5000

Frontend
cd frontend
npm install

# Create .env.local if needed
# VITE_API_BASE_URL=/api
# VITE_DEV_API_PROXY=http://localhost:5000

npm run dev
# → http://localhost:5173

Vite automatically proxies /api/* → backend in development.
Important Environment Variables

Backend (.env)

DATABASE_URL           = postgresql://...
JWT_SECRET_KEY         = super-secret-32+-chars-random
APP_ENV                = development | production
CORS_ALLOWED_ORIGINS   = http://localhost:5173   # comma separated in prod
BREVO_API_KEY          = ...
DELIVERY_SOURCE_BRANCH_ID = ...

Frontend (.env / .env.local)
VITE_API_BASE_URL      = /api               # dev proxy
# VITE_API_BASE_URL    = https://api.example.com   # production

Production Deployment Notes
Backend

WSGI server: Gunicorn
Reverse proxy: Nginx (SSL termination)
Database: Managed PostgreSQL (RDS, Supabase, Neon, …)
Secrets: platform secrets manager
Run migrations before starting app: alembic upgrade head

Frontend

Static hosting: Vercel / Netlify / Cloudflare Pages / AWS S3 + CloudFront
Build: npm run build
Serve dist/ with SPA-friendly routing (try_files $uri /index.html)

Security Must-Dos Before Going Live

Strong, unique JWT_SECRET_KEY
Specific CORS_ALLOWED_ORIGINS (never *)
HTTPS everywhere
Secure database credentials + SSL connection
Rate limiting enabled
Security headers (CSP, HSTS, …) verified
Brevo sender email verified
Remove/disable debug mode & seed scripts

API & UI Documentation

API endpoints → try /api/v1/ or generate OpenAPI/Swagger
Postman collection → import from /backend/docs/ (if available)
UI flows → customer /store/*, operations /* (dashboard, picking, admin)

Testing

# Backend
cd backend
pytest --cov=app

# Frontend
cd frontend
npm test
# or
npm run coverage

Contributing / Next Steps
See CONTRIBUTING.md (if present)
Common improvement areas:

OpenAPI / Swagger UI integration
Real-time order status (WebSocket / SSE)
Background jobs (Celery / RQ)
Performance monitoring & error tracking (Sentry)
End-to-end tests (Playwright / Cypress)
Mobile PWA capabilities

Questions → open an issue or contact the team.
Happy coding! 🛒

This version is shorter than the sum of the two original READMEs, removes duplication, uses consistent tone and formatting, and serves as a good entry point for new developers while still pointing to deeper details when needed.

Let me know if you'd like to emphasize certain parts more (e.g. deployment, testing, security), shorten it further, or add sections (roadmap, screenshots, etc.).