# FixIt Passport

FixIt Passport is a FastAPI backend and Next.js frontend for household asset passports.

## Requirements

- Python 3.12 (Python 3.11+ should also work)
- Node.js 20+
- npm (included with Node.js)

## Backend setup (Windows PowerShell)

From the `backend` directory:

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

Copy `.env.example` to `.env` if `.env` does not already exist. Keep `.env` local and never commit or share it. The API starts without an NVIDIA key; photo recognition and model-generated assistant answers require a valid `NVIDIA_API_KEY` in `backend/.env`.

Start the API:

```powershell
python -m uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload
```

Check `http://127.0.0.1:8000/health` and interactive API docs at `http://127.0.0.1:8000/docs`.

## Frontend setup

Open another PowerShell window and enter the `backend/frontend` directory:

```powershell
npm ci
npm run dev
```

Open `http://localhost:3000`. The frontend uses `http://127.0.0.1:8000/api/v1` by default. To change it, set `NEXT_PUBLIC_API_URL` before starting the frontend, for example:

```powershell
$env:NEXT_PUBLIC_API_URL = 'http://127.0.0.1:8000/api/v1'
npm run dev
```

## Verification

From `backend`:

```powershell
python -m pytest -q
```

From `backend/frontend`:

```powershell
npm run build
```

The development database defaults to SQLite at `backend/fixit_passport.db`. Back up that file before deleting or replacing it; application startup may create tables and seed demo records.

## NVIDIA API troubleshooting

If the API returns an authorization error, create a replacement NVIDIA API key in the NVIDIA developer console and update only the local `NVIDIA_API_KEY` value in `backend/.env`. Restart the backend after changing `.env`. Never paste the key into source code, issues, or chat. The current integration was tested against NVIDIA, but the configured credential was rejected, so live model calls are not currently verified.
