# Flizen 🛫

**A premium, modern flight booking platform with a sleek dark-mode UI and powerful backend infrastructure.**

Flizen is a full-stack flight booking application that brings together a beautiful React frontend with a robust FastAPI backend. Whether you're searching for flights, comparing prices, or managing bookings, Flizen makes it effortless with an Uber-inspired design language and seamless user experience.

## 🎯 Project Overview

Flizen is a complete flight booking solution with:

- **🎨 Modern UI**: Dark-mode dashboard with Material Design 3 colors
- **⚡ Fast & Responsive**: Built with React, Vite, and Tailwind CSS
- **🔍 Smart Search**: Real-time airport autocomplete with IATA code support
- **📅 Flexible Booking**: Interactive calendar, passenger selection, cabin class options
- **🗺️ Route Visualization**: Interactive SVG maps showing flight routes
- **🚀 Scalable Backend**: FastAPI with Firebase Firestore database
- **💾 Persistent Data**: Airport database with Firestore integration
- **🔌 REST API**: Comprehensive API with real-time flight search

## 📦 Repository Structure

This is a **monorepo** containing two independent but interconnected submodules:

```
Flizen/
├── Flizen-Frontend/          # React + Tailwind CSS frontend (submodule)
│   ├── src/
│   │   ├── components/       # Reusable React components
│   │   ├── services/         # API integration layer
│   │   └── App.jsx           # Main app component
│   ├── package.json
│   └── vite.config.js
│
├── Flizen-Backend/           # FastAPI backend (submodule)
│   ├── app/
│   │   ├── routes/           # API endpoints
│   │   ├── services/         # Business logic
│   │   ├── models/           # Data models
│   │   └── main.py           # FastAPI app
│   ├── requirements.txt
│   └── seed_airports.py
│
├── README.md                 # This file
└── LICENSE
```

### Frontend Repository
**[Flizen-Frontend](./Flizen-Frontend)** - React + Tailwind CSS

A modern, responsive flight booking dashboard with:
- **AirportAutocomplete**: Search airports by city, IATA code, or country
- **DatePickerField**: Custom calendar with validation
- **PassengerSelector**: Select adults, children, infants
- **CabinClassSelector**: Choose from 4 cabin classes
- **RouteMap**: Dynamic SVG visualization of flight routes
- **FlightCard**: Beautiful flight result cards with details
- **QuickActions**: Fast booking shortcuts
- **FlightStatusCheck**: Track flight numbers

### Backend Repository
**[Flizen-Backend](./Flizen-Backend)** - FastAPI + Firebase

A powerful REST API backend providing:
- **Airport Search**: Full-text search across 10,000+ airports
- **Flight Search**: Route-based flight discovery
- **Data Persistence**: Firestore database with airport catalog
- **Health Monitoring**: System status endpoints
- **CORS Support**: Cross-origin requests from frontend

## 🚀 Quick Start

### Prerequisites
- **Node.js** 16+ and npm/yarn
- **Python** 3.9+
- **Firebase Project** with credentials
- **Git** with submodule support

### Setup: Clone with Submodules

```bash
# Clone the main repo with submodules
git clone --recurse-submodules https://github.com/harshpatel2312/Flizen.git
cd Flizen

# Or if you already cloned without submodules:
git submodule update --init --recursive
```

### Backend Setup (FastAPI)

```bash
cd Flizen-Backend

# Create virtual environment
python -m venv venv

# Activate virtual environment
# Windows:
venv\Scripts\activate
# macOS/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Configure environment
cp .env.example .env
# Edit .env and add your Firebase credentials

# Add your Firebase admin key
# Place firebase-admin-key.json in this directory

# Seed airport data
python seed_airports.py

# Start the backend server
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

**Backend will run at:** `http://localhost:8000`
- API Docs: `http://localhost:8000/docs`
- ReDoc: `http://localhost:8000/redoc`

### Frontend Setup (React + Vite)

```bash
# In a new terminal, navigate to frontend
cd Flizen-Frontend

# Install dependencies
npm install

# Configure environment
cp .env.example .env.local
# Edit .env.local and set VITE_API_BASE_URL=http://localhost:8000

# Start the development server
npm run dev
```

**Frontend will run at:** `http://localhost:5173`

## 📡 API Endpoints

### Health Check
```
GET /health
```
Returns backend status and environment info.

### Airport Search
```
GET /airports/search?q=<query>
```
Search airports by city, name, IATA code, ICAO code, or country.

```
GET /airports/{iata}
```
Get specific airport details by IATA code (e.g., `/airports/JFK`).

### Flight Search
```
POST /flights/search
```
Search for flights based on route, dates, passengers, and cabin class.

**Request:**
```json
{
  "from_airport": "JFK",
  "to_airport": "LHR",
  "departure_date": "2024-12-20",
  "return_date": "2024-12-27",
  "passengers": {
    "adults": 1,
    "children": 0,
    "infants": 0
  },
  "cabin_class": "Economy"
}
```

**Response:**
```json
{
  "outbound_flights": [...],
  "return_flights": [...],
  "search_date": "2024-01-15T10:30:00",
  "currency": "USD"
}
```

## 💻 Tech Stack

### Frontend
- **React** 18+ - UI library
- **Vite** - Fast build tool and dev server
- **Tailwind CSS** - Utility-first styling
- **Axios** - HTTP client for API calls
- **Dark Mode** - Material Design 3 colors

### Backend
- **FastAPI** - Modern, fast web framework
- **Python 3.9+** - Programming language
- **Firebase/Firestore** - NoSQL database
- **CORS Middleware** - Cross-origin support
- **Uvicorn** - ASGI server

## 🎨 Design Philosophy

Flizen follows **Uber's design language** with:
- Dark theme as the default
- Clean, minimal interfaces
- Smooth interactions and animations
- Mobile-first responsive design
- Accessibility best practices

## 📋 Features

### Current Features ✅
- [x] Airport search and autocomplete
- [x] Flight search API
- [x] Responsive booking interface
- [x] Date picker with validation
- [x] Passenger selection
- [x] Cabin class selector
- [x] Route visualization
- [x] Flight status lookup
- [x] Quick action tiles
- [x] Firestore integration

### Planned Features 🚀
- [ ] Real flight API integration (Amadeus, Skyscanner, Duffel)
- [ ] User authentication & profiles
- [ ] Booking management system
- [ ] Payment processing (Stripe, PayPal)
- [ ] Email confirmations and notifications
- [ ] Flight analytics and trending routes
- [ ] Multi-currency support
- [ ] Mobile app (React Native)

## 🔧 Development Workflow

### Working with Submodules

Update all submodules:
```bash
git submodule update --init --recursive
```

Pull latest changes from submodules:
```bash
git pull --recurse-submodules
```

Push changes to a submodule:
```bash
cd Flizen-Backend  # or Flizen-Frontend
git add .
git commit -m "Your commit message"
git push
cd ..
git add Flizen-Backend
git commit -m "Update Flizen-Backend submodule"
git push
```

### Project Structure Deep Dive

**Frontend** (`Flizen-Frontend/src/`):
- `components/` - Reusable React components
- `services/` - API integration (apiService.js)
- `data/` - Sample data and constants
- `App.jsx` - Main application component

**Backend** (`Flizen-Backend/app/`):
- `routes/` - FastAPI route handlers
- `services/` - Business logic and data operations
- `models/` - Pydantic data models
- `core/` - Configuration and Firebase setup
- `main.py` - FastAPI application entry point

## 🛠️ Environment Configuration

### Backend (.env)
```
FIREBASE_CREDENTIALS_PATH=firebase-admin-key.json
FIREBASE_PROJECT_ID=your-project-id
CORS_ORIGINS=http://localhost:5173,http://localhost:3000
ENVIRONMENT=development
```

### Frontend (.env.local)
```
VITE_API_BASE_URL=http://localhost:8000
```

## 📊 Database Schema

### Firestore Collections

**airports**
```
airports/
  JFK/
    {
      "name": "John F. Kennedy International Airport",
      "city": "New York",
      "country": "United States",
      "iata": "JFK",
      "icao": "KJFK",
      "latitude": 40.6413,
      "longitude": -73.7781,
      "timezone": "America/New_York"
    }
  ...
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the Apache License 2.0. See the [LICENSE](./LICENSE) file for details.

## 🚀 Getting Help

- **Frontend Issues**: Check [Flizen-Frontend/README.md](./Flizen-Frontend/README.md)
- **Backend Issues**: Check [Flizen-Backend/README.md](./Flizen-Backend/README.md)
- **API Documentation**: Visit http://localhost:8000/docs (when backend is running)
- **Open an Issue**: GitHub Issues section

---

**Built with ❤️ by the Flizen team**