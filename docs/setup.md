# Setup & Run

## Prerequisites

Python 3.11+ (backend — FastAPI, SQLAlchemy)
A modern browser (frontend is plain static HTML/CSS/JS, no Node/npm needed, no build step)
No database server to install — SQLite ships built into Python; the .db file is created automatically on first run
No PostGIS, no PostgreSQL — there's no geometry in this system yet (see architecture.md section 2 for the honest gap between the template and what's built)

## Environment variables

None are required to run this locally or in the current deployment. There is no .env file and no environment-variable-driven configuration in the codebase — DATABASE_URL is hardcoded in database.py as sqlite:///./civicmysuru.db, and API_BASE is hardcoded in frontend/js/common.js (auto-switches between localhost and the deployed URL by checking location.hostname, not an env var).

The one environment variable that is read, but isn't something you set yourself: PORT, which Railway/Render inject automatically in production. main.py checks os.environ.get("PORT") to decide whether to run in local dev mode (127.0.0.1:8000, auto-reload on) or production mode (0.0.0.0:<PORT>, auto-reload off). You never need to set this by hand.

## Local install & run

Terminal 1
cd backend
pip install -r requirements.txt
python main.py
Runs at http://127.0.0.1:8000. Interactive API docs at /docs. civicmysuru.db is created automatically — delete it any time to reset to a clean state.
Terminal 2 
cd frontend
python -m http.server 5500
Open http://127.0.0.1:5500/index.html. Don't open the HTML files directly as file:// — some browsers block the fetch() calls to the backend that way.

## Seed / sample data

`<Describe the synthetic dataset you loaded — e.g. "120 synthetic complaints across 6 wards, including N deliberately placed at boundary edges between MCC and a newly absorbed panchayat.">`

## Testing offline mode

Cut entirely. As established in the previous document (defense.md, section 5), there is no offline queue, no service worker, and no sync-on-reconnect anywhere in this codebase. Airplane mode would simply make every fetch() call fail with no special handling — the citizen would just see "Backend not reachable," the same message shown whenever the backend is down for any reason. Don't put these four steps in a real setup guide; a judge following them would find broken behavior, not a feature.

## If the live deployment is down

https://civicmysuru-hackathon-4td3zj67f-naikaj1814-4974s-projects.vercel.app
