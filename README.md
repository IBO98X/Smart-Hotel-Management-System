<div align="center">

<img src="https://img.shields.io/badge/MERN-Stack-00D4AA?style=for-the-badge&logo=mongodb&logoColor=white" />
<img src="https://img.shields.io/badge/TypeScript-Powered-3178C6?style=for-the-badge&logo=typescript&logoColor=white" />
<img src="https://img.shields.io/badge/AI-Integrated-FF6B35?style=for-the-badge&logo=google&logoColor=white" />
<img src="https://img.shields.io/badge/Version-2.0-brightgreen?style=for-the-badge" />
<img src="https://img.shields.io/badge/University_of_Sulaimani-2026-8B0000?style=for-the-badge" />

<br /><br />

# 🏨 Smart Hotel Management System
### *AMI Hotel — Where Luxury Meets Timeless Elegance*

> A full-stack, AI-powered hotel management platform developed as a **Bachelor of Science in Computer Science** graduation project at the **University of Sulaimani**, May 2026.

<br />

**[✨ Features](#-features) · [🏗️ Architecture](#️-architecture) · [🗄️ Database](#️-database-design) · [🤖 AI Chatbot](#-ai-chatbot) · [📡 API](#-api-reference) · [⚙️ Setup](#️-installation--setup) · [🖼️ Screenshots](#️-screenshots) · [👥 Team](#-team)**

</div>

---

## 📖 Overview

The **Smart Hotel Management System** is a comprehensive full-stack web application designed to centralize and automate the daily operations of a hotel. It transitions hotel administration from fragmented, manual processes to a modern, data-driven platform — reducing human error, improving operational efficiency, and empowering staff with AI-assisted support.

The system was developed over multiple Agile sprints, with the core booking engine built and tested before integrating more advanced features such as role-based access control, dynamic invoicing, discount management, and an AI-powered chatbot.

### 🎯 The Problem It Solves

| Problem | Solution |
|---|---|
| Manual booking errors & double-booked rooms | Real-time availability tracking with conflict prevention |
| Fragmented guest & financial data | Centralized MongoDB database with structured collections |
| No real-time operational insights | Live dashboard with KPIs, occupancy charts & daily summaries |
| Staff needing supervisor help for policy queries | Role-aware AI chatbot with live system context |
| No promotional pricing support | Full discount code system with role-based apply/remove permissions |

---

## 🆕 What's New in v2.0

### 🏷️ Discount Management System
- Added a dedicated **Discount Management page** for admins and managers
- Supports two discount types: **percentage-based** and **fixed-amount**
- Each code is configurable with validity period, usage limits, minimum booking amount, and maximum discount cap
- Discount codes can be created, edited, activated, deactivated, and deleted with real-time usage tracking
- Discounts can be applied to invoices at creation time or post-creation
- Receptionists can only apply discounts from a pre-approved active list and cannot remove them once applied
- A confirmation dialog with an irreversibility warning is shown to receptionists before applying a discount
- Discount amount is always clamped to the invoice subtotal — total can never go negative

### 🐛 Bug Fixes
- Tax is now correctly calculated **after** the discount is deducted, not before
- Fixed discount dialog not reopening the selection view after removal
- Fixed input fields losing focus on every keystroke in the Discount Form
- Fixed role detection that was preventing discount UI elements from rendering correctly

### 🤖 Chatbot Enhancement
- Added support for **two AI models**: Gemma 4 (31B) as default and Gemini 2.5 Flash as a faster alternative
- Staff can switch between models per request based on their needs
- Automatic fallback to Gemma 31B if an unrecognized model is provided

### 🎨 UI Improvements
- Added a **Discount column** to the invoice table showing the applied code and savings inline
- Original price shown with strikethrough when a discount is applied
- Receptionist discount dialog shows available codes as **clickable cards** instead of a text input
- Revenue stat cards are now hidden from receptionists
- Minor spacing, color, and consistency improvements across invoice and discount interfaces

---

## ✨ Features

### Core Modules
- **📊 Dashboard** — Real-time hotel overview: room statuses, today's arrivals/departures, revenue, and quick actions
- **🛏️ Room Management** — Full CRUD for rooms and room types, including pricing, amenities, capacity, and live status tracking (`Available`, `Occupied`, `Dirty`, `Maintenance`)
- **📅 Reservations** — Multi-step booking wizard with date conflict detection, guest linking, and status lifecycle management
- **👥 Guest Management** — Guest profiles with booking history, contact details, and ID records
- **🧾 Invoicing & Billing** — Automated invoice generation with room charges, itemized services, tax calculation, discount application, and payment status tracking
- **🏷️ Discount Management** — Full promotional code system with percentage and fixed-amount types, usage tracking, validity windows, and role-based permissions
- **🛎️ Services** — Manage hotel amenities (spa, dining, laundry, etc.) with taxable/non-taxable flags
- **👤 User Management** — Staff accounts with role-based access control (Admin, Manager, Receptionist, Housekeeping)
- **⚙️ Settings** — Hotel profile configuration, tax rates, and currency settings

### Highlights
- 🤖 **AI-Powered Chatbot** — Context-aware assistant with role-based data access, multi-turn memory, dual AI model support, and predictive insights
- 📱 **Fully Responsive** — Every page is optimized for desktop and mobile
- 🔒 **JWT Authentication** — Secure, token-based session management across all endpoints
- 🌗 **Light & Dark Themes** — System-wide theme support

---

## 🏗️ Architecture

The system follows a **decoupled client–server architecture** where the frontend, backend, and database operate independently and communicate through RESTful APIs.

```
┌─────────────────────────────────────────────────────────────┐
│                        CLIENT LAYER                         │
│          React.js + TypeScript + Vite + MUI                 │
│              Single Page Application (SPA)                  │
└──────────────────────────┬──────────────────────────────────┘
                           │  REST API (JSON)
┌──────────────────────────▼──────────────────────────────────┐
│                      APPLICATION LAYER                      │
│              Node.js + Express.js + TypeScript              │
│     Business Logic · Auth · Routing · AI Integration       │
└──────────────────────────┬──────────────────────────────────┘
                           │  Mongoose ODM
┌──────────────────────────▼──────────────────────────────────┐
│                        DATA LAYER                           │
│               MongoDB Atlas (Cloud · AWS)                   │
│         Document-oriented · Flexible Schema Design         │
└─────────────────────────────────────────────────────────────┘
```

### Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | React.js, TypeScript, Vite, MUI (Material UI) |
| **Backend** | Node.js, Express.js, TypeScript |
| **Database** | MongoDB, Mongoose ODM, MongoDB Atlas |
| **AI / Chatbot** | Google Gemma 4 (31B) · Gemini 2.5 Flash via `@google/generative-ai` SDK |
| **Auth** | JWT (JSON Web Tokens), bcrypt password hashing |
| **Dev Tools** | Postman, Draw.io, Figma, GitHub, TickTick |

---

## 🗄️ Database Design

The system uses **8 MongoDB collections** with application-level referential integrity enforced via Mongoose middleware:

```
Hotel ──────────────────────────────────── (global config, standalone)
User ───────────────────────────────────── (staff accounts, JWT auth)
RoomType ──────────────────────┐
                               │ 1:N
Room ──────────────────────────┘ ─────────┐
                                          │ N:1
Guest ─────────────────────────┐          │
                               │ 1:N      │ N:1
Booking ───────────────────────┘──────────┘
   │ 1:1
Invoice ────── usedServices[] ──── Service (N:M via embedded array)
```

### Collections Summary

| Collection | Key Fields |
|---|---|
| `User` | username (unique), password (hashed), role, isActive, invoicesCreated |
| `RoomType` | name (unique), basePrice, capacity, amenities[] |
| `Room` | roomNumber (unique), roomType (ref), status, floor, image |
| `Guest` | idNumber (unique), firstName, lastName, email, phoneNumber, bookingCount |
| `Booking` | guest (ref), room (ref), checkIn/OutDate, status, adults, children |
| `Service` | name (unique), price, isTaxable, category |
| `Invoice` | booking (ref), createdBy (ref), usedServices[], appliedDiscount, subtotal, discountAmount, taxableAmount, totalAmountDue, paymentStatus |
| `Hotel` | name, address, taxRate, currency, discountCodes[] |

> **Design decision:** Invoice totals (`totalRoomCharge`, `taxAmount`, `discountAmount`, `totalAmountDue`) are stored as computed snapshots at creation time — ensuring historical accuracy even if room prices or discount codes change later.

---

## 🤖 AI Chatbot

The chatbot is a **role-aware, context-driven assistant** that gives hotel staff real-time access to live system data through natural language.

### How It Works

```
User Message
     │
     ▼
chatController ──► validates role & sessionId
     │
     ▼
chatService ──► buildContextForRole()
                    │
                    ├── queries MongoDB in real time
                    ├── formats data as structured text
                    └── injects anti-hallucination rules
     │
     ▼
Gemini API (gemma-4-31b-it)
  multi-turn session with history (max 20 turns)
     │
     ▼
JSON response ──► React frontend
```

### Role-Based Data Access

| Role | Data Access |
|---|---|
| **Admin** | Rooms, bookings, guests, invoices, services, forecasts, system users |
| **Manager** | Rooms, bookings, invoices, services, forecasts (no guest PII) |
| **Receptionist** | Rooms, bookings, guests, services, operational forecasts (no financials) |
| **Housekeeping** | Room statuses and today's snapshot only |

### Key Technical Details
- **Models:** Google Gemma 4 (31B) — default · Gemini 2.5 Flash — fast alternative
- **Memory:** Per-session conversation history stored server-side in a `Map<sessionId, Content[]>` (max 20 turns)
- **Security:** API key stored server-side only, never exposed to the client
- **Reliability:** Retry loop (max 3 attempts) with intelligent backoff on HTTP 429
- **Anti-hallucination:** Strict system prompt rules — the model is instructed to only use data present in the injected context

---

## 👥 Role-Based Access Control

| Feature | Admin | Manager | Receptionist | Housekeeping |
|---|:---:|:---:|:---:|:---:|
| Dashboard | ✅ | ✅ | ✅ | ✅ |
| Room Management | ✅ | ✅ | ✅ | ✅ (view/status) |
| Reservations | ✅ | ✅ | ✅ | ❌ |
| Guests | ✅ | ✅ | ✅ | ❌ |
| Services | ✅ | ✅ | ✅ | ❌ |
| Invoices | ✅ | ✅ | ✅ | ❌ |
| Apply Discount to Invoice | ✅ | ✅ | ✅ | ❌ |
| Remove Discount from Invoice | ✅ | ✅ | ❌ | ❌ |
| Discount Management Page | ✅ | ✅ | ❌ | ❌ |
| User Management | ✅ | ✅ | ❌ | ❌ |
| Hotel Settings | ✅ | ❌ | ❌ | ❌ |
| Tax Settings | ✅ | ❌ | ❌ | ❌ |

---

## 📡 API Reference

All endpoints require JWT authentication **except**: `POST /auth/login`, `POST /auth/signup`, and `GET /api/hotel`.

### Authentication — `/api/auth`
| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/auth/login` | User login |
| `POST` | `/auth/create-user` | Create new staff user |
| `GET` | `/auth` | Get all users |
| `GET` | `/auth/:id` | Get user by ID |
| `PUT` | `/auth/:id` | Update user |
| `DELETE` | `/auth/:id` | Delete user |
| `PATCH` | `/auth/:id/role` | Update role only |
| `PATCH` | `/auth/:id/reset-password` | Reset password |
| `POST` | `/auth/change-password` | Change own password |

### Bookings — `/api/bookings`
| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/bookings` | Get all bookings |
| `GET` | `/bookings/:id` | Get booking by ID |
| `POST` | `/bookings` | Create new booking |
| `PUT` | `/bookings/:id` | Update booking |
| `DELETE` | `/bookings/:id` | Delete booking |

### Hotel & Discounts — `/api/hotel`
| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/hotel` | Get hotel information |
| `POST` | `/hotel` | Create hotel settings |
| `PUT` | `/hotel` | Update hotel information |
| `POST` | `/hotel/discounts` | Create a new discount code |
| `PUT` | `/hotel/discounts/:code` | Update an existing discount code |
| `DELETE` | `/hotel/discounts/:code` | Delete a discount code |
| `POST` | `/hotel/discounts/:code/validate` | Validate a discount code against a booking amount |

### Invoices — `/api/invoices`
| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/invoices` | Get all invoices |
| `GET` | `/invoices/:id` | Get invoice by ID |
| `GET` | `/invoices/booking/:bookingId` | Get invoice by booking ID |
| `POST` | `/invoices` | Create invoice for a booking |
| `PATCH` | `/invoices/:id/payment` | Update payment status and method |
| `PATCH` | `/invoices/:id/services` | Add a service to an invoice |
| `PATCH` | `/invoices/:id/discount` | Apply a discount code to an invoice |
| `DELETE` | `/invoices/:id/discount` | Remove an applied discount from an invoice |

### Other Modules
| Module | Base Path | Operations |
|---|---|---|
| Rooms | `/api/rooms` | CRUD + `PATCH /:id` (status only) |
| Room Types | `/api/room-types` | Full CRUD |
| Guests | `/api/guests` | Full CRUD |
| Services | `/api/services` | Full CRUD |
| Dashboard | `/api/dashboard` | GET (aggregated KPIs) |
| Chatbot | `/api/chat` | `POST /chat` · `POST /chat/clear` |

---

## ⚙️ Installation & Setup

### Prerequisites
- Node.js `v18+`
- MongoDB Atlas account (or local MongoDB instance)
- Google Gemini API key

### 1. Clone the repository
```bash
git clone https://github.com/your-org/smart-hotel-management.git
cd smart-hotel-management
```

### 2. Backend Setup
```bash
cd backend
npm install
```

Create a `.env` file based on `.env.example`:
```env
PORT=5000
MONGODB_URI=mongodb+srv://<user>:<password>@cluster.mongodb.net/hotel
JWT_SECRET=your_jwt_secret_key
GEMINI_API_KEY=your_google_gemini_api_key
```

```bash
npm run dev
```

### 3. Frontend Setup
```bash
cd frontend
npm install
npm run dev
```

The application will be available at `http://localhost:5173`

### Default Admin Account
On first run, use the signup endpoint or seed script to create the initial Admin account, then configure hotel information via the **Settings** page.

---

## 🖼️ Screenshots

| Page | Desktop | Mobile |
|---|---|---|
| Landing Page | Dark hero with hotel imagery | Responsive layout |
| Login | Split panel with hotel background | Full-screen form |
| Dashboard | KPI cards + room distribution chart | Stacked card layout |
| All Rooms | Grid view with status badges | Single-column scroll |
| Reservations | Tabular list with filters | Card-based list |
| Invoices | Financial summary + discount column + table | Compact card view |
| Discount Management | Code cards with usage tracking | Single-column grid |
| Chatbot | Floating overlay panel | Full-screen modal |

> All pages support both **dark mode** (default) and **light mode**, toggled via the Settings icon in the top navigation bar.

---

## 🚀 Future Roadmap

- [ ] **Guest-Facing Mobile App** — React Native app for self-service booking, check-in/out, and service requests
- [ ] **Payment Gateway Integration** — Stripe/PayPal for in-system invoice settlement and advance deposits
- [ ] **Predictive Analytics & Dynamic Pricing** — AI-powered occupancy forecasting and revenue optimization
- [ ] **Microservices Migration** — Decompose the monolith into independently deployable services (auth, reservations, billing, notifications)
- [ ] **Real-Time Notifications** — WebSocket (Socket.IO) push alerts for reservations, check-outs, and maintenance requests
- [ ] **Load Testing & Performance Hardening** — Apache JMeter / k6 benchmarking + Redis caching + compound indexing

---

## 📋 Changelog

### v2.0 — May 2026
- Added full Discount Management System with role-based permissions
- Added dual AI model support for the chatbot (Gemma 4 31B + Gemini 2.5 Flash)
- Fixed tax calculation order (discount applied before tax)
- Fixed Discount Form input focus loss bug
- Fixed role detection for discount UI rendering
- UI improvements across invoice and discount interfaces

### v1.0 — Initial Release
- Core hotel management modules: rooms, bookings, guests, invoices, services
- Role-based access control with JWT authentication
- AI chatbot with role-aware context injection
- Responsive design with light and dark theme support

---

## 👥 Team

| Name | Role |
|---|---|
| **Ibrahim Qahtan Adnan** | Developer |
| **Abdulazeez Qusay Abdulrazaq** | Developer |
| **Mustafa Musab Abdulkareem** | Developer |
| **Kozhin Kamal Khafoor** | Developer |

**Supervised by:** Assist. Prof. Dr. Miran Taha Abdullah

**Institution:** College of Science, University of Sulaimani
**Degree:** Bachelor of Science in Computer Science
**Submitted:** May 2026

---

## 📄 License

This project was developed as an academic graduation project at the University of Sulaimani. All rights reserved by the authors.

---

<div align="center">

Made with ❤️ by the AMI Hotel Team · University of Sulaimani · 2026

</div>
