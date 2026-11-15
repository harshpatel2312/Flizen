# Flizen

A full-stack flight booking platform with a dark-mode React frontend and a FastAPI backend. Flight offers and orders come from the [Duffel Air API](https://duffel.com), payments are processed with [Stripe](https://stripe.com), and users sign in with Firebase (Google).

## Repository structure

```
Flizen/
├── Flizen-Frontend/   # React + Vite + Tailwind CSS (git submodule)
├── Flizen-Backend/    # FastAPI + Python (git submodule)
└── README.md
```

Both apps are git submodules. Clone with `git clone --recurse-submodules`, or run `git submodule update --init` after cloning. Commit inside each submodule first, then commit the updated submodule pointers here.

## Features

- Flight search (one-way and round trip) with live Duffel offers, sorting, and client-side filters.
- Passenger details with a country-code phone picker (per-country formatting and validation) and searchable nationality.
- Optional seat selection (visual preference; not yet confirmed with the airline).
- Stripe checkout with server-side pricing, webhook-driven fulfillment, and automatic refunds when the airline order fails.
- Booking status page with live progress and airline reference.
- **My Trips:** all bookings grouped into upcoming, pending, completed, cancelled, and refunded; a Trip Details drawer shows the itinerary, airline references, seats and baggage, travellers, and payment history for each trip.
- User profile (personal, contact, passport, and travel preferences).
- Booking flow selections survive browser Back / Forward (session-only, never for payment data).

## Tech stack

| Layer     | Technology                                                                     |
|-----------|--------------------------------------------------------------------------------|
| Frontend  | React 18, React Router 7, Vite 5, Tailwind CSS, Stripe Elements, libphonenumber-js |
| Backend   | FastAPI, uvicorn, httpx, Pydantic v2, Stripe Python SDK                        |
| Auth      | Firebase Authentication (Google sign-in), Firebase Admin SDK                   |
| Data      | Duffel Air API, Firebase Firestore (profiles, bookings, payment records, Stripe event log) |
| Payments  | Stripe PaymentIntents + webhooks                                               |
| Logging   | Python `logging` with `RotatingFileHandler`                                    |

## Quick start

### 1. Backend

```bash
cd Flizen-Backend
python -m venv venv
venv\Scripts\activate        # Windows
# source venv/bin/activate   # macOS/Linux
pip install -r requirements.txt
uvicorn app.main:app --reload
```

API at `http://localhost:8000`, docs at `http://localhost:8000/docs`. To also print logs to the terminal, run `python -m app.main --log-terminal` instead (the uvicorn CLI does not accept that flag).

### 2. Stripe webhook forwarding

Bookings are only fulfilled when Stripe's webhook reaches the backend. Locally, keep the [Stripe CLI](https://docs.stripe.com/stripe-cli) running in a separate terminal:

```bash
stripe login
stripe listen --forward-to http://localhost:8000/api/payments/webhook
```

Put the `whsec_...` secret it prints into `STRIPE_WEBHOOK_SECRET`. Without this, payments succeed in Stripe but bookings stay on "Securing your booking".

### 3. Frontend

```bash
cd Flizen-Frontend
npm install
npm run dev
```

App runs at `http://localhost:3000`.

## Environment variables

### Backend: `Flizen-Backend/.env`

```
FIREBASE_CREDENTIALS_PATH=firebase-admin-key.json
FIREBASE_PROJECT_ID=your-project-id
CORS_ORIGINS=http://localhost:3000
ENVIRONMENT=development
DUFFEL_ACCESS_TOKEN=duffel_test_...
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...
FRONTEND_URL=http://localhost:3000
```

### Frontend: `Flizen-Frontend/.env.local`

```
VITE_API_BASE_URL=http://localhost:8000
VITE_STRIPE_PUBLISHABLE_KEY=pk_test_...
VITE_FIREBASE_API_KEY=AIza...
VITE_FIREBASE_AUTH_DOMAIN=your-project.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your-project-id
VITE_FIREBASE_STORAGE_BUCKET=your-project.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=123456789
VITE_FIREBASE_APP_ID=1:123:web:abc
```

Never commit `.env`, `.env.local`, or `firebase-admin-key.json`.

## Pages

| Route                                   | Description                                         |
|-----------------------------------------|-----------------------------------------------------|
| `/`, `/login`                           | Login                                               |
| `/signup`                               | Sign-up with Google                                 |
| `/dashboard`                            | Search form and recent trips                        |
| `/flights`                              | Results, sort, filters, flight details              |
| `/passengers`                           | Traveller and contact details                       |
| `/seat-choice`, `/seats`                | Optional seat selection                             |
| `/payment/:bookingId`                   | Stripe checkout                                     |
| `/payment/:bookingId/processing`        | Stripe return URL, live booking status              |
| `/booking/:bookingId`                   | Live booking status                                 |
| `/trips`, `/trips/:bookingId`           | My Trips and trip details                           |
| `/profile`                              | User profile                                        |

## Notes

- Duffel and Stripe run in **test mode** locally (`duffel_test_...`, `sk_test_...` / `pk_test_...`). Duffel test orders are confirmed by its test airline, but no real ticket is issued. Use Stripe test card `4242 4242 4242 4242`.
- Firestore security rules and indexes live in `Flizen-Backend/` (`firestore.rules`, `firestore.indexes.json`); deploy them with `firebase deploy --only firestore:rules,firestore:indexes`. Data model: [Flizen-Backend/docs/FIRESTORE.md](./Flizen-Backend/docs/FIRESTORE.md).
- Logs are written to `Flizen-Backend/logs/flizen.log` (rotating, 10 MB, 5 backups).
- See [Flizen-Backend/README.md](./Flizen-Backend/README.md) and [Flizen-Frontend/README.md](./Flizen-Frontend/README.md) for API, architecture, and deployment details.

## License

Apache License 2.0. See [LICENSE](./LICENSE).
