# Hala Saudi _  AI-Powered Tourism Platform

<div align="center">

**An intelligent, full-stack travel planning platform for Saudi Arabia**
built with React, Node.js, MongoDB, and OpenAI

[![React](https://img.shields.io/badge/React-18-61DAFB?logo=react)](https://reactjs.org/)
[![Node.js](https://img.shields.io/badge/Node.js-Express-339933?logo=node.js)](https://nodejs.org/)
[![MongoDB](https://img.shields.io/badge/MongoDB-Mongoose-47A248?logo=mongodb)](https://mongodb.com/)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT--3.5-412991?logo=openai)](https://openai.com/)
[![JWT](https://img.shields.io/badge/Auth-JWT-000000?logo=jsonwebtokens)](https://jwt.io/)

**Developed by Ziyad Alshahrani**
Prince Sultan University — Software Engineering Senior Project
Supervised by Prof. Dr. Eng. Mohammed Akkour

</div>

---

## 👨‍💻 About Me & This Project

**Ziyad Alshahrani** | Full-Stack Developer

I built **Hala Saudi** as my Software Engineering senior project at Prince Sultan University. I was responsible for the full-stack implementation — database schema design, REST API architecture, React frontend, AI itinerary generation engine, authentication system, admin dashboard, testing suite, and deployment configuration.

The platform is aligned with Saudi Arabia's **Vision 2030** tourism goals, helping tourists discover destinations, hotels, and restaurants across the Kingdom and generate personalized multi-day travel itineraries powered by AI.

---

## 🖥️ Screenshots

### Destinations — Search & Discovery
![Destinations](docs/screenshots/destinations_search.jpg)

### Admin Dashboard
![Dashboard](docs/screenshots/admin_dashboard.jpg)

### Admin — Edit Modal
![Edit Modal](docs/screenshots/edit_modal.jpg)

### User Profile — Favorites
![Favorites](docs/screenshots/favorites_profile.jpg)

### User Profile — Management
![Profile](docs/screenshots/profile_page.jpg)

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🤖 AI Itinerary Generator | Personalized day-by-day plans via OpenAI based on 6 user inputs |
| 🗺️ Destination Discovery | Filter by city, category, price, rating with interactive Leaflet maps |
| 🏨 Hotels | Search by price range, amenities, and hotel class |
| 🍽️ Restaurants | Filter by cuisine, category, opening hours, and price range |
| ⭐ Ratings & Reviews | Star ratings with score breakdown for all listing types |
| ❤️ Likes / Favorites | Save and manage favorites across all listing types per user |
| 🔍 Global Search | Case-insensitive debounced search across all content |
| 🔐 Auth System | JWT + HTTP-only cookies, email verification, password reset |
| 👤 User Profiles | Photo upload, favorites tab, review history |
| 🛡️ Admin Dashboard | Full CRUD, stats cards, reusable modal, role-based access |
| 🔔 Notifications | Real-time toast notifications via react-toastify |
| 🧪 Test Suite | Unit, integration, and black-box tests for backend & frontend |

---

## 🗓️ Built Across 4 Agile Sprints

| Sprint | Key Deliverables |
|--------|-----------------|
| **Sprint 1** | System architecture, MVC design, database schema, security requirements |
| **Sprint 2** | Admin dashboard, CRUD for destinations/hotels/restaurants, filtering & sorting |
| **Sprint 3** | Ratings & reviews, likes/favorites, global search, real-time notifications |
| **Sprint 4** | User profile management, search optimization, end-to-end testing, deployment |

---

## 🛠️ Tech Stack

### Frontend
| Technology | Purpose |
|-----------|---------|
| React 18 + Vite | SPA framework & build tool |
| React Router v6 | Client-side routing |
| Framer Motion | UI animations |
| Leaflet / React-Leaflet | Interactive maps |
| Axios | HTTP client |
| Bootstrap 5 + CSS | Responsive UI |
| react-toastify | Real-time notifications |
| Vitest + React Testing Library | Frontend testing |

### Backend
| Technology | Purpose |
|-----------|---------|
| Node.js + Express.js | REST API server (MVC pattern) |
| MongoDB + Mongoose | NoSQL database & ODM |
| JWT + bcryptjs | Auth & password hashing (salt factor 10) |
| OpenAI API | AI itinerary generation |
| Multer | File/image uploads |
| Nodemailer | Email (reset & verification) |
| express-rate-limit | API rate limiting |
| xss | Input sanitization |
| Jest + Supertest + mongodb-memory-server | Backend testing |

---

## 🏗️ Project Architecture

```
hala-saudi/
├── frontend/
│   └── src/
│       ├── pages/
│       │   ├── Home/
│       │   ├── Destinations/
│       │   ├── Hotels/
│       │   ├── Restaurants/
│       │   ├── ItineraryPlanner/     # AI wizard (6-step form)
│       │   ├── Dashboard/            # Admin CRUD + stats
│       │   ├── Profile/
│       │   ├── ItemDetails/
│       │   ├── SearchAll/
│       │   └── SignIn / SignUp / ResetPassword
│       ├── components/
│       │   ├── ItineraryPlanner/
│       │   ├── Rating/
│       │   ├── LikeButton/
│       │   ├── FilterPanel/
│       │   └── LocationMap/
│       └── context/                  # AuthContext, ItineraryContext
│
└── backend/
    ├── Controllers/
    │   ├── itineraryController.js    # OpenAI AI generation
    │   ├── authController.js
    │   ├── destinationController.js
    │   ├── hotelController.js
    │   ├── restaurantController.js
    │   ├── ratingController.js
    │   ├── likeController.js
    │   ├── profileController.js
    │   └── dashboardController.js
    ├── Models/
    ├── Routes/
    ├── middleware/
    └── tests/
```

---

## 🤖 AI Itinerary Generator — Core Feature

```
Destination → Duration → Interests → Budget → Travelers → Food Preferences
```

```js
// itineraryController.js
const [destinations, hotels, restaurants] = await Promise.all([
  Destination.find({ locationCity: destination }),
  Hotel.find({ locationCity: destination }),
  Restaurant.find({ locationCity: destination }),
]);

const prompt = constructPrompt(userData, { destinations, hotels, restaurants });

const completion = await openai.createChatCompletion({
  model: "gpt-3.5-turbo",
  messages: [{ role: "user", content: prompt }]
});

const itinerary = JSON.parse(completion.data.choices[0].message.content);
await Itinerary.create({ user: userId, ...itinerary });
```

> The AI only recommends venues that exist in the database — no hallucinated places.

---

## 🔐 Security Implementation

| Requirement | Implementation |
|------------|---------------|
| Authentication | JWT tokens, 24h expiry + refresh token mechanism |
| Passwords | bcrypt salt factor 10, never stored in plaintext |
| Token storage | HTTP-only cookies (not localStorage) |
| CORS | Approved domains only; `Secure` + `SameSite` in production |
| Authorization | Role-based: `user` and `admin` with dedicated middleware |
| Input security | Client + server validation, XSS sanitization |
| Password reset | Time-limited JWT (1h expiry) + email verification |
| Rate limiting | express-rate-limit on auth and sensitive routes |

---

## 📐 System Design Diagrams

### Class Diagram
![Class Diagram](docs/diagrams/class_diagram.jpg)

### User Journey Sequence
![User Journey](docs/diagrams/user_journey_sequence.jpg)

### Admin Operations Sequence
![Admin Sequence](docs/diagrams/admin_operations_sequence.jpg)

### ER Diagram
![ER Diagram](docs/diagrams/er_diagram.jpg)

---

## 🧪 Testing

```bash
cd backend && npm run test:coverage
cd frontend && npm run test:coverage
```

| Test Area | Coverage |
|-----------|---------|
| `authController` | Login, register, token validation |
| `destinationController` | CRUD, filtering, search |
| `hotelController` | Listing, admin operations |
| `restaurantController` | CRUD, search |
| `likeController` | Like/unlike logic |
| `itineraryController` | AI generation, retrieval |
| `authMiddleware` | JWT verification, role guard |
| `validationMiddleware` | XSS, sanitization |
| Frontend components | LikeButton, Rating, Auth flows |

> All backend tests use **in-memory MongoDB** — production data is never touched.

---

## 🚀 Getting Started

```bash
git clone https://github.com/Ziyadalshrani507/UNI-Senior-Project-HalaSaudi.git
cd UNI-Senior-Project-HalaSaudi
cd backend && npm install
cd ../frontend && npm install
```

`backend/.env`:
```env
MONGODB_URI=your_mongodb_uri
JWT_SECRET=your_jwt_secret
PORT=5000
FRONTEND_URL=http://localhost:5173
OPENAI_API_KEY=your_openai_key
EMAIL_USER=your_email
EMAIL_PASS=your_email_password
```

`frontend/.env`:
```env
VITE_API_URL=http://localhost:5000
```

```bash
cd backend && npm run dev
cd frontend && npm run dev
```

---

<div align="center">
  <strong>Built by Ziyad Alshahrani</strong><br/>
  Prince Sultan University · Software Engineering · Vision 2030
</div>
