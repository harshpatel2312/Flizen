# Flizen

A full-stack flight booking platform with a dark-mode React frontend and a FastAPI backend. Flight data is sourced in real time from the [Duffel Air API](https://duffel.com).

## Repository structure

```
Flizen/
├── Flizen-Frontend/   # React + Vite + Tailwind CSS
├── Flizen-Backend/    # FastAPI + Python
└── README.md
```

## Tech stack

| Layer    | Technology                                                                |
|----------|---------------------------------------------------------------------------|
| Frontend | React 18, Vite 5, Tailwind CSS, React Router v6                           |
| Backend  | FastAPI, uvicorn, httpx, Pydantic v2                                      |
| Auth     | Firebase Authentication (Google sign-in), Firebase Admin SDK              |
| Data     | Duffel Air API (live flight offers), Firebase Firestore (airport cache)   |
| Logging  | Python `logging` with `RotatingFileHandler`                               |

## Quick start

### Backend

```bash
cd Flizen-Backend
python -m venv venv
venv\Scripts\activate        # Windows
# source venv/bin/activate   # macOS/Linux
pip install -r requirements.txt
uvicorn app.main:app --reload
```

Add `--log-terminal` to also print logs to the console:

```bash
uvicorn app.main:app --reload --log-terminal
```

API available at `http://localhost:8000`. Interactive docs at `http://localhost:8000/docs`.

### Frontend

```bash
cd Flizen-Frontend
npm install
npm run dev
```

App runs at `http://localhost:5173`.

## Environment variables

### Backend — `Flizen-Backend/.env`

```
DUFFEL_ACCESS_TOKEN=duffel_test_...
FIREBASE_CREDENTIALS_PATH=firebase-admin-key.json
FIREBASE_PROJECT_ID=your-project-id
CORS_ORIGINS=http://localhost:5173
ENVIRONMENT=development
```

### Frontend — `Flizen-Frontend/.env.local`

```
VITE_API_BASE_URL=http://localhost:8000
VITE_FIREBASE_API_KEY=AIza...
VITE_FIREBASE_AUTH_DOMAIN=your-project.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your-project-id
VITE_FIREBASE_STORAGE_BUCKET=your-project.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=123456789
VITE_FIREBASE_APP_ID=1:123:web:abc
```

The app loads without Firebase config — Firebase is only required when the user clicks **Continue with Google**.

## Pages

| Route          | Description                                                                   |
|----------------|-------------------------------------------------------------------------------|
| `/`            | Login — entry point for all users                                             |
| `/login`       | Same as `/`                                                                   |
| `/signup`      | Sign-up with Google                                                           |
| `/dashboard`   | Dashboard — booking form and flight history                                   |
| `/flights`     | Flight listing — results, sort, filter panel, and flight details side panel   |
| `/passengers`  | Passenger details — traveller info form before booking confirmation           |


- The Duffel integration runs in **test mode** by default. Test tokens start with `duffel_test_`. Offers are synthetic and cannot be booked.
- Logs are always written to `Flizen-Backend/logs/flizen.log` (rotating, 10 MB, 5 backups). Pass `--log-terminal` to also emit to stdout.
- See each subdirectory README for component-level and API-level details.

## License

Apache License 2.0. See [LICENSE](./LICENSE).
