# Flizen

A full-stack flight booking platform with a dark-mode React frontend and a FastAPI backend. Flight data is sourced in real time from the [Duffel Air API](https://duffel.com).

## Repository structure

```
Flizen/
+-- Flizen-Frontend/   # React + Vite + Tailwind CSS
+-- Flizen-Backend/    # FastAPI + Python
+-- README.md
+-- LICENSE
```

## Tech stack

| Layer    | Technology                                              |
|----------|---------------------------------------------------------|
| Frontend | React 18, Vite 5, Tailwind CSS, react-router-dom v7     |
| Backend  | FastAPI, uvicorn, httpx, Pydantic                       |
| Data     | Duffel Air API (flight offers), Firebase Firestore (airport cache) |
| Logging  | Python `logging` with `RotatingFileHandler`             |

## Quick start

### Backend

```bash
cd Flizen-Backend
python -m venv venv
venv\Scripts\activate        # Windows: venv\Scripts\activate | macOS/Linux: source venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload
```

Add `--log-terminal` to also print logs to the console:

```bash
uvicorn app.main:app --reload --log-terminal
```

API available at `http://localhost:8000`. Interactive docs at `/docs`.

### Frontend

```bash
cd Flizen-Frontend
npm install
npm run dev
```

App runs at `http://localhost:3000`.

## Environment variables

### Backend � `Flizen-Backend/.env`

```
DUFFEL_ACCESS_TOKEN=duffel_test_...
FIREBASE_CREDENTIALS_PATH=firebase-admin-key.json
FIREBASE_PROJECT_ID=your-project-id
CORS_ORIGINS=http://localhost:3000
ENVIRONMENT=development
```

### Frontend � `Flizen-Frontend/.env`

```
VITE_API_BASE_URL=http://localhost:8000
```

## Pages

| Route      | Description                                                           |
|------------|-----------------------------------------------------------------------|
| `/`        | Dashboard � booking form, quick actions, flight history, suggested routes |
| `/flights` | Flight listing � results, sorting, flight details panel               |

## Notes

- The Duffel integration runs in **test mode** by default. Test tokens start with `duffel_test_`. Offers are synthetic and cannot be booked.
- Logs are always written to `Flizen-Backend/logs/flizen.log` (rotating, 10 MB, 5 backups). Pass `--log-terminal` to also emit to stdout.
- See each subdirectory README for component-level and API-level details.

## License

Apache License 2.0. See [LICENSE](./LICENSE).
