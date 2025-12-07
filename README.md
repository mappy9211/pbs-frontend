PBS (Projects)
===============

This workspace contains three related projects for the PBS application:

- `pbs-backend` — FastAPI backend (Python)
- `pbs-frontend` — Web frontend (React + Vite + TypeScript)
- `pbs-mobile`   — React Native mobile app

This README explains how to run each part locally and notes about the media endpoints used by the apps.

Repository layout
-----------------

- pbs-backend/
  - app/
  - requirements.txt
  - create_tables.py
- pbs-frontend/
  - src/
  - package.json
- pbs-mobile/
  - app/
  - package.json

Prerequisites
-------------

- Node.js (>=18) and npm
- Python 3.11
- Java (for Android builds) and Android SDK (if running mobile on Android)
- Xcode (if running mobile on iOS)

Quick start — backend
---------------------

1. Create a virtual environment and install dependencies:

```bash
cd pbs-backend
python3.11 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

2. Configure environment variables (optional):
- `DATABASE_URL` — Postgres connection (if used)
- `SECRET_KEY` — JWT secret (defaults exist for development)

3. Run the backend (development):

```bash
uvicorn app.main:app --reload
```

Notes:
- Uploaded media are served from the `uploads/` directory. The backend mounts that directory at both `/uploads` and `/mobile/uploads` so web and mobile clients can access files.
- Mobile API routes are prefixed with `/mobile` (for example `POST /mobile/login`). A mobile-friendly media endpoint exists at `GET /mobile/media?user_id=<id>&date=YYYY-MM-DD`.

Quick start — frontend
----------------------

1. Install and run:

```bash
cd pbs-frontend
npm install
npm run dev
```

2. Open the app in the browser (Vite will show the URL, usually `http://localhost:5173`).

Quick start — mobile
--------------------

1. Install dependencies:

```bash
cd pbs-mobile
npm install
```

2. Configure `app/config/api.js` if you need to override the host/IP used by physical devices (it contains convenience values for simulator/emulator and LAN fallback).

3. Run on Android emulator:

```bash
npx react-native run-android
```

4. Run on iOS simulator:

```bash
npx react-native run-ios
```

Notes and important endpoints
-----------------------------

- Dashboard media (web API):
  - `GET /dashboard/media?user_id=<id>&date=YYYY-MM-DD` — list media for a subscriber and date.
  - `POST /dashboard/media` — upload media (multipart FormData; `files` entries, `user_id`, `date`).

- Mobile media (mobile-friendly endpoint):
  - `GET /mobile/media?user_id=<id>&date=YYYY-MM-DD` — same shape as dashboard media (returns `id, original_name, url, media_type, created_at`).

- Static uploads mounting: the backend serves uploaded files from `uploads/` and also from `/mobile/uploads` to match mobile URL construction.

UI notes (mobile)
-----------------
- The mobile app displays Today's and Tomorrow's posts in a list; images render full-width and videos show a placeholder with a play icon. A viewer modal offers Download/Open, WhatsApp share, and native Share.
- The mobile header uses `SafeAreaView` to avoid status bar gaps.

Contributing / next steps
-------------------------
- Add inline video playback in the mobile app by adding `react-native-video` and rendering it inside the modal.
- Replace remote logo with a bundled asset for faster startup and offline usage.

If you'd like, I can:
- Add a root-level README per-project (more detailed instructions per subproject),
- Add sample .env templates for backend/frontend,
- Wire inline video playback for mobile.

Contact
-------
If you want me to make further changes (inline video, custom fonts, better thumbnails, or automated tests), tell me which part to implement next and I'll apply the changes.
