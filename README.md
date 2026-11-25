# DevPath

DevPath is an AI-enhanced and gamified student portfolio platform for BSCS and BSIT students. It connects to GitHub, organizes repository-based portfolio evidence, generates personalized learning-path suggestions, and tracks progress through experience points, levels, badges, and achievements.

## Main Features

- GitHub OAuth authentication and repository synchronization
- Student portfolio dashboard and public portfolio view
- Repository-based skill and career analysis
- FLAN-T5 recommendation model with a deterministic fallback
- Personalized learning paths and project learning paths
- XP, levels, badges, achievements, and leaderboard
- Admin dashboard, validation tools, analytics, and AI evaluation
- Supabase/PostgreSQL database support

## Project Structure

```text
backend/            FastAPI backend, database models, services, routers, and AI models
frontend/           React and Vite frontend application
supabase_schema.sql Database schema for Supabase/PostgreSQL
```

The large model files in `backend/models/` are tracked with Git LFS.

## Requirements

- Python 3.11 or newer
- Node.js 18 or newer
- Git LFS
- A Supabase/PostgreSQL database for persistent deployment
- A GitHub OAuth application for GitHub sign-in

## Backend Setup

From the repository root:

```powershell
cd backend
python -m venv .venv
.\\.venv\\Scripts\\Activate.ps1
pip install -r requirements.txt
```

Create `backend/.env` and provide the required configuration:

```env
APP_ENV=local
FRONTEND_URL=http://localhost:5173
BACKEND_URL=http://localhost:8000
DATABASE_URL=postgresql://USER:PASSWORD@HOST:5432/postgres
DATABASE_SSLMODE=require
JWT_SECRET=replace_with_a_secure_secret
JWT_ISSUER=devpath
GITHUB_CLIENT_ID=your_github_client_id
GITHUB_CLIENT_SECRET=your_github_client_secret
GITHUB_REDIRECT_URI=http://localhost:8000/auth/github/callback
FLAN_T5_MODEL=models/final_flan_t5_github_recommender
```

Never commit `.env`, passwords, OAuth secrets, database credentials, or API keys.

Run the backend:

```powershell
python -m uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload
```

Backend address: `http://127.0.0.1:8000`

## Frontend Setup

Open a second terminal from the repository root:

```powershell
cd frontend
npm install
npm run dev -- --host 127.0.0.1
```

For local development, create `frontend/.env` with:

```env
VITE_API_BASE=http://127.0.0.1:8000
```

Frontend address: `http://127.0.0.1:5173`

## Database Setup

Run `supabase_schema.sql` in the Supabase SQL Editor or the target PostgreSQL database before using the application. Configure the resulting database URL in `backend/.env`.

## Useful Routes

- Student application: `http://127.0.0.1:5173/`
- Admin login: `http://127.0.0.1:5173/admin-login`
- Learning paths: `http://127.0.0.1:5173/learning-paths`
- AI evaluation: `http://127.0.0.1:5173/admin/ai-evaluation`
- Backend health check: `http://127.0.0.1:8000/api/ping`

## AI Model and Fallback

The backend includes the fine-tuned FLAN-T5 model and the fallback model bundle in `backend/models/`. Git LFS must be installed and the repository must be cloned with LFS files available:

```powershell
git lfs install
git lfs pull
```

If the local FLAN-T5 model is unavailable, the backend can use its rule-based and machine-learning fallback behavior for supported recommendation flows.

## Build Checks

Frontend production build:

```powershell
cd frontend
npm run build
```

Backend tests, when available:

```powershell
cd backend
pytest
```

## Security Note

This is an academic project. Review authentication, OAuth token storage, database access, and deployment settings before using the system with real student data or in production.
