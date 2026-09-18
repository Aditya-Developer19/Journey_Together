# 🚗 JourneyTogether — Long-Term Ride Sharing Platform

> **Carpooling reimagined.** JourneyTogether is a full-stack MERN ride-sharing platform built for daily commuters who want a smarter, greener, and safer way to travel together — without the friction of rebooking every day.

[![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=white)](https://react.dev/)

[![Node.js](https://img.shields.io/badge/Node.js-Express-339933?logo=node.js&logoColor=white)](https://nodejs.org/)

[![MongoDB](https://img.shields.io/badge/MongoDB-Mongoose-47A248?logo=mongodb&logoColor=white)](https://www.mongodb.com/)

[![TailwindCSS](https://img.shields.io/badge/TailwindCSS-4.0-06B6D4?logo=tailwindcss&logoColor=white)](https://tailwindcss.com/)

[![Socket.io](https://img.shields.io/badge/Socket.io-Realtime-010101?logo=socket.io)](https://socket.io/)

[![TomTom](https://img.shields.io/badge/TomTom-Maps%20API-DF1B12)](https://developer.tomtom.com/)

[![Twilio](https://img.shields.io/badge/Twilio-SMS%20API-F22F46?logo=twilio&logoColor=white)](https://www.twilio.com/)

---

## 📖 Table of Contents

- [Overview](#-overview)

- [Key Features](#-key-features)

- [Tech Stack](#-tech-stack)

- [Project Structure](#-project-structure)

- [Architecture](#️-architecture)

- [API Reference](#-api-reference)

- [Getting Started](#-getting-started)

- [Environment Variables](#-environment-variables)

- [Carbon Savings Formula](#-how-carbon-savings-are-calculated)

- [User Journey](#-user-journey)

- [Safety Features](#️-safety-features)

---

## 🌟 Overview

JourneyTogether solves a core inefficiency in ride-sharing — the daily rebooking cycle. By enabling users to form **Fixed Commuter Circles**, the platform lets regular commuters lock in recurring rides with trusted people on their route, cutting booking overhead entirely.

Beyond convenience, JourneyTogether actively tracks the **environmental impact** of every shared ride, delivering personalized CO₂ savings metrics and contributing to a measurable reduction in per-commute emissions. The platform also prioritizes **safety** through an integrated SOS emergency system and women-only ride filtering.

---

## ✨ Key Features

### 🔄 Ride Management

- **Create & Browse Rides** — Post rides with origin, destination, date, time, available seats, and price

- **Ride Booking** — Passengers can confirm their seat; seat count decrements in real time

- **Ride Completion** — Drivers mark rides as complete, triggering the carbon savings calculation

- **Filter Rides** — Search by origin, destination, date, or ride type (women-only)

- **Ongoing & History Views** — Separate views for active and past rides per user

### 🌿 Carbon Footprint Tracker

- Automatically calculates CO₂ savings for every completed shared ride

- Uses a physics-based formula: `CO₂ Saved = distance × (1 - 1/totalPeople) × 0.21 kg/km`

- Carbon savings are accumulated per user and displayed on the profile dashboard

- Results in an estimated **25% reduction** in individual per-commute emissions

### 🚨 SOS Emergency System (Twilio)

- One-tap SOS alert sends an SMS with the user's **live GPS coordinates** to all registered emergency contacts

- Message includes a **TomTom MapShare live location link** for instant tracking

- Users can add, view, and delete emergency contacts from their profile

### 👩 Women-Only Ride Mode

- Drivers can flag rides as **Women-Only** at creation time

- Booking is restricted to verified female passengers

- Both the driver and passenger must be female — enforced at the server level

- Provides a safer, more trusted commuting option

### 🔐 Secure Multi-Factor Authentication

- **Email OTP verification** on signup via Brevo transactional email API

- **JWT sessions** stored in secure HTTP-only cookies

- `protectRoute` middleware validates every authenticated request

- Profile photo upload via **Cloudinary** (Multer + cloud storage)

- Vehicle registration (number plate, type, fuel) stored per user profile

### 🗺️ TomTom Maps Integration

- **Geocoding** — Converts city/address input to precise lat/lon coordinates

- **Route Distance Calculation** — Uses TomTom Routing API to compute real driving distances in km

- **Autocomplete Search** — Location suggestions scoped to India with proximity-aware bias

- **Live Map** — Real-time driver location streaming via Socket.io, rendered on an interactive map

- **Route Visualization** — Leaflet + React-Leaflet renders the full route on ride detail cards

### ⭐ Driver Rating System

- Passengers can rate their driver (1–5 stars) after a ride is completed

- Ratings are averaged and displayed on driver profiles and ride listings

- Duplicate rating prevention — one rating per passenger per ride

### ⚡ Real-Time Location Tracking (Socket.io)

- Drivers broadcast their GPS location via WebSocket using the `sendLocation` event

- Passengers in the same ride room receive live `locationUpdate` events

- Room-based architecture: each ride has its own isolated Socket.io room (`ride-<rideId>`)

---

## 🛠️ Tech Stack

### Frontend

| Technology | Purpose |

|---|---|

| **React 19** | UI framework |

| **Vite 6** | Build tool & dev server |

| **TailwindCSS 4** | Utility-first styling |

| **Framer Motion** | Page transitions & micro-animations |

| **GSAP** | Advanced SVG & timeline animations |

| **React Router v7** | Client-side routing |

| **Socket.io Client** | Real-time WebSocket communication |

| **TomTom Web SDK** | Interactive maps |

| **Leaflet + React-Leaflet** | Route rendering |

| **Lucide React** | Icon library |

| **Axios** | HTTP client |

| **React Hot Toast** | Toast notifications |

### Backend

| Technology | Purpose |

|---|---|

| **Node.js + Express** | REST API server |

| **MongoDB + Mongoose** | Database & ODM |

| **Socket.io** | Real-time WebSocket server |

| **JWT (jsonwebtoken)** | Auth token generation & verification |

| **bcryptjs** | Password hashing |

| **Twilio** | SMS SOS alerts |

| **Brevo (@getbrevo/brevo)** | Transactional email (OTP & welcome) |

| **Cloudinary** | Profile photo storage |

| **Multer** | File upload handling |

| **cookie-parser** | HTTP-only cookie management |

### External APIs

| API | Usage |

|---|---|

| **TomTom Geocoding API** | Address → lat/lon coordinates |

| **TomTom Routing API** | Real driving distance & ETA calculation |

| **TomTom Search API** | Autocomplete location suggestions |

| **Twilio Messaging API** | SOS SMS alerts to emergency contacts |

| **Brevo Email API** | OTP verification & welcome emails |

| **Cloudinary API** | Image upload & CDN storage |

---

## 📁 Project Structure

```

JourneyTogether/

├── backend/

│   ├── controllers/

│   │   ├── auth.controller.js      # Signup, login, logout, OTP verify, vehicle info, carbon stats

│   │   ├── ride.controller.js      # CRUD, confirm, complete, rate, ongoing/completed, carbon calc

│   │   ├── sos.controller.js       # Send SOS SMS, manage emergency contacts

│   │   └── maps.controller.js      # Geocoding, autocomplete, distance calculation

│   ├── models/

│   │   ├── user.model.js           # User schema (auth, vehicle, ratings, carbonSaved, emergency contacts)

│   │   ├── ride.model.js           # Ride schema (route, passengers, ratings, womenOnly, distance)

│   │   └── contact.model.js        # Emergency contact schema

│   ├── routes/

│   │   ├── auth.routes.js          # /api/auth/*

│   │   ├── rides.routes.js         # /api/rides/*

│   │   ├── sos.routes.js           # /api/sos/*

│   │   └── maps.routes.js          # /api/maps/*

│   ├── middlewares/

│   │   ├── auth.middleware.js      # JWT protectRoute + Brevo email sender

│   │   └── multer.middleware.js    # File upload config (disk storage)

│   ├── utils/

│   │   ├── generateToken.js        # JWT generation + HTTP-only cookie setter

│   │   ├── cloudinary.js           # Cloudinary upload helper

│   │   ├── email.config.js         # Brevo API client setup

│   │   ├── emailTemplate.js        # HTML email templates (OTP + welcome)

│   │   └── getCoordinates.js       # TomTom geocoding utility

│   ├── db/

│   │   └── db.js                   # MongoDB connection

│   └── server.js                   # Express app bootstrap, Socket.io config, CORS

│

└── frontend/

    └── src/

        ├── pages/

        │   ├── Start.jsx               # Landing page with GSAP animations

        │   ├── UserSignup.jsx          # Registration form

        │   ├── UserLogin.jsx           # Login form

        │   ├── VerifyEmail.jsx         # OTP email verification

        │   ├── VehicleInfoPage.jsx     # Driver vehicle registration

        │   ├── Home.jsx                # Dashboard: ongoing rides, SOS button, quick actions

        │   ├── AllRides.jsx            # Browse & search all available rides

        │   ├── Card.jsx                # Ride detail page with map, booking, live tracking

        │   ├── CreateRide.jsx          # Ride creation form with autocomplete

        │   ├── ProfilePage.jsx         # User profile, carbon stats, ride history, SOS contacts

        │   └── UserProtectWrapper.jsx  # Auth guard higher-order component

        ├── components/

        │   ├── MapComponent.jsx        # TomTom + Leaflet route map

        │   ├── LiveMap.jsx             # Real-time driver tracking map (Socket.io)

        │   ├── LiveRouteStatusBar.jsx  # Live ride progress indicator

        │   ├── OngoingRidesSection.jsx # Active rides widget on dashboard

        │   ├── RateDriverModal.jsx     # Post-ride star rating modal

        │   ├── RideDetailsLoading.jsx  # Skeleton loading state

        │   ├── Navbar.jsx              # Top navigation bar

        │   ├── Buttons.jsx             # Reusable button components

        │   └── Cursor.jsx              # Custom animated cursor

        ├── context/

        │   └── UserContext.jsx         # Global user state via React Context

        ├── hooks/

        │   └── useGeoLocation.js       # Browser Geolocation API hook

        └── socket.js                   # Socket.io client singleton

```

---

## 🏗️ Architecture

```

┌─────────────────────────────────────────────────────────────┐

│                     CLIENT (React + Vite)                    │

│   ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌───────────┐  │

│   │  Pages   │  │Components│  │ Context  │  │ Socket.io │  │

│   │          │  │          │  │(UserCtx) │  │  Client   │  │

│   └────┬─────┘  └────┬─────┘  └────┬─────┘  └─────┬─────┘  │

└────────┼─────────────┼─────────────┼───────────────┼────────┘

         │   Axios (HTTP/REST)                  WebSocket

         ▼                                         ▼

┌─────────────────────────────────────────────────────────────┐

│                 SERVER (Express + Socket.io)                 │

│   ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │

│   │  /auth   │  │  /rides  │  │   /sos   │  │  /maps   │   │

│   └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘   │

│        └─────────────┴──────────── ┴──────────────┘         │

│                       Controllers + Middleware               │

│                       Mongoose Models                        │

└───────────────────────────┬─────────────────────────────────┘

                            ▼

                   ┌─────────────────┐

                   │  MongoDB Atlas  │

                   └─────────────────┘

   ┌───────────┐  ┌───────────┐  ┌───────────┐  ┌───────────┐

   │ TomTom API│  │Twilio API │  │Brevo Email│  │Cloudinary │

   │Maps/Routes│  │ SOS SMS   │  │OTP/Welcome│  │  Images   │

   └───────────┘  └───────────┘  └───────────┘  └───────────┘

```

---

## 📡 API Reference

### Auth — `/api/auth`

| Method | Endpoint | Auth | Description |

|--------|----------|------|-------------|

| `POST` | `/signup` | ❌ | Register new user, sends OTP verification email |

| `POST` | `/login` | ❌ | Login with email/password, returns JWT cookie |

| `POST` | `/logout` | ✅ | Clears JWT cookie and ends session |

| `POST` | `/verifyEmail` | ❌ | Verify email using 6-digit OTP code |

| `GET` | `/me` | ✅ | Get current authenticated user's profile |

| `POST` | `/vehicleInfo` | ✅ | Save driver's vehicle details |

| `POST` | `/pfp` | ✅ | Upload profile photo (stored on Cloudinary) |

| `GET` | `/carbon-stats` | ✅ | Get user's cumulative CO₂ saved in kg |

### Rides — `/api/rides`

| Method | Endpoint | Auth | Description |

|--------|----------|------|-------------|

| `POST` | `/create` | ✅ | Create a new ride (distance auto-calculated via TomTom) |

| `GET` | `/all` | ❌ | Get all available (incomplete) rides |

| `GET` | `/ride/:id` | ❌ | Get a single ride by ID |

| `PUT` | `/update/:id` | ✅ | Update ride details (driver only) |

| `DELETE` | `/delete/:id` | ✅ | Delete a ride (driver only) |

| `POST` | `/filter` | ✅ | Filter rides by from/to/date/womenOnly |

| `POST` | `/confirm/:id` | ✅ | Book a seat on a ride |

| `POST` | `/complete/:id` | ✅ | Mark ride complete & trigger carbon calculation |

| `POST` | `/rate/:id` | ✅ | Rate the driver after a completed ride |

| `GET` | `/ongoing` | ✅ | Get user's currently active rides |

| `GET` | `/completed` | ✅ | Get user's completed ride history |

| `GET` | `/pending-ratings` | ✅ | Get completed rides where user hasn't rated yet |

### SOS — `/api/sos`

| Method | Endpoint | Auth | Description |

|--------|----------|------|-------------|

| `POST` | `/send` | ✅ | Trigger SOS — SMS with live location sent via Twilio |

| `POST` | `/contacts` | ✅ | Add a new emergency contact |

| `GET` | `/contacts` | ✅ | Get all emergency contacts for the user |

| `DELETE` | `/contacts/:id` | ✅ | Remove an emergency contact |

### Maps — `/api/maps`

| Method | Endpoint | Auth | Description |

|--------|----------|------|-------------|

| `GET` | `/autocomplete` | ❌ | Get location search suggestions from TomTom |

| `GET` | `/coordinates` | ❌ | Geocode an address to lat/lon coordinates |

### WebSocket Events (Socket.io)

| Event | Direction | Payload | Description |

|-------|-----------|---------|-------------|

| `joinRide` | Client → Server | `{ rideId }` | Join a ride's location tracking room |

| `sendLocation` | Client → Server | `{ rideId, lat, lon }` | Driver broadcasts current GPS position |

| `locationUpdate` | Server → Client | `{ rideId, lat, lon }` | Passengers receive live driver location |

| `leaveRide` | Client → Server | `{ rideId }` | Leave the ride's tracking room |

---

## 🚀 Getting Started

### Prerequisites

- Node.js v18+

- MongoDB (local instance or [MongoDB Atlas](https://www.mongodb.com/atlas))

- TomTom API Key — [Get one free](https://developer.tomtom.com/)

- Twilio Account — [Sign up](https://www.twilio.com/)

- Brevo Account — [Sign up](https://www.brevo.com/)

- Cloudinary Account — [Sign up](https://cloudinary.com/)

### Installation

**1. Clone the repository**

```bash

git clone https://github.com/payalgupta25/JourneyTogether.git

cd JourneyTogether

```

**2. Set up the Backend**

```bash

cd backend

npm install

```

Create a `.env` file in `backend/` (see [Environment Variables](#-environment-variables) below), then:

```bash

npm run dev

# Server starts on http://localhost:8000

```

**3. Set up the Frontend**

```bash

cd ../frontend

npm install

```

Create a `.env` file in `frontend/`:

```env

VITE_BASE_URL=http://localhost:8000

```

```bash

npm run dev

# App starts on http://localhost:5173

```

---

## 🔑 Environment Variables

Create a `.env` file in the `backend/` directory with the following:

```env

# Server

PORT=8000

NODE_ENV=development

# MongoDB

MONGO_URI=mongodb+srv://<username>:<password>@cluster.mongodb.net/journeytogether

# JWT

JWT_SECRET=your_super_secret_jwt_key

# Client URL (for CORS)

CLIENT_URL=http://localhost:5173

# TomTom Maps API

TOMTOM_API_KEY=your_tomtom_api_key

# Twilio (SOS SMS)

TWILIO_SID=your_twilio_account_sid

TWILIO_AUTH_TOKEN=your_twilio_auth_token

TWILIO_PHONE_NUMBER=+1XXXXXXXXXX

# Brevo (Transactional Email)

BREVO_API_KEY=your_brevo_api_key

EMAIL_SENDER_NAME=JourneyTogether

EMAIL_SENDER_ADDRESS=noreply@yourdomain.com

# Cloudinary (Profile Photos)

CLOUDINARY_CLOUD_NAME=your_cloud_name

CLOUDINARY_API_KEY=your_cloudinary_api_key

CLOUDINARY_API_SECRET=your_cloudinary_api_secret

```

---

## 💡 How Carbon Savings Are Calculated

When a driver marks a ride as complete, the backend calculates CO₂ savings for every participant:

```js

const emissionFactor = 0.21; // kg CO₂ per km (average petrol car)

const totalPeople = 1 + passengers.length; // driver + all passengers

// CO₂ saved compared to everyone driving alone

const totalCarbonSaved = distance * (1 - 1 / totalPeople) * emissionFactor;

// Distributed equally across all participants

const carbonPerUser = totalCarbonSaved / totalPeople;

```

Each user's `carbonSaved` field (in kg) is incremented in MongoDB, building a cumulative environmental impact score that is displayed prominently on the profile dashboard.

---

## 👤 User Journey

```

Register → Verify Email OTP → Add Vehicle Info

      ↓

Dashboard (Home) → Browse Rides  OR  Create a Ride

      ↓                  ↓

      └──────── Confirm Seat Booking ──────────┘

                         ↓

              Live Map Tracking (Socket.io)

                         ↓

              Driver Marks Ride Complete

                         ↓

         Rate Driver → Carbon Stats Updated

                         ↓

              Profile Dashboard (total CO₂ saved,

               ride history, emergency contacts)

```

---

## 🛡️ Safety Features

| Feature | Implementation |

|---|---|

| **SOS Alert** | One-tap Twilio SMS with TomTom live location link sent to all emergency contacts |

| **Women-Only Rides** | Server-side gender validation enforced for both driver and passenger |

| **JWT Auth** | Tokens stored in HTTP-only cookies — immune to XSS token theft |

| **Email OTP MFA** | 6-digit verification code with 24-hour expiry via Brevo |

| **Password Hashing** | bcryptjs with salt rounds = 10 |

| **Route Protection** | All sensitive endpoints require a valid JWT via `protectRoute` middleware |

| **Driver-Passenger Separation** | Drivers cannot book their own rides; passengers cannot complete rides |
