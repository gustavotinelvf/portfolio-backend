# 🚀 Portfolio Fullstack - Gustavo Tínel

A dynamic full-stack portfolio platform for managing academic and personal projects, featuring complete CRUD operations, secure JWT authentication, and cloud deployment.

🔗 **Live Demo:** [portfolio-gustavo-tinel.vercel.app](https://gustavotinel.vercel.app/))

---

## 📋 Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Features](#features)
- [Key Challenges Solved](#key-challenges-solved)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [Deployment](#deployment)

---

## Overview

A personal portfolio built from scratch with a decoupled frontend/backend architecture. The public-facing side displays projects dynamically fetched from a PostgreSQL database. An admin dashboard protected by JWT authentication allows the owner to create, edit, and delete projects in real time.

---

## 🛠️ Tech Stack

**Backend**
- Java 21
- Spring Boot 4
- Spring Security + JWT (java-jwt 4.4.0)
- Spring Data JPA + Hibernate
- PostgreSQL
- Maven

**Frontend**
- React 18 + Vite
- Axios (with JWT interceptor)
- React Router DOM
- CSS Modules

**Infrastructure**
- Backend: [Render](https://render.com) (Docker, free tier)
- Frontend: [Vercel](https://vercel.com)
- Database: [Neon](https://neon.tech) (Serverless PostgreSQL)

---

## 🏗️ Architecture

```
[Browser]
    │
    ▼
[Vercel - React/Vite]
    │  REST API calls (Axios + JWT interceptor)
    ▼
[Render - Spring Boot]
    │  Spring Data JPA
    ▼
[Neon - PostgreSQL]
```

**Authentication Flow:**
1. Admin submits credentials via `/api/login`
2. Backend validates and returns a signed JWT token
3. Frontend stores token in `localStorage`
4. Axios interceptor injects `Authorization: Bearer <token>` on every subsequent request
5. Spring Security validates token on protected routes

---

## ✨ Features

- **Public Portfolio** — visitors can browse and search projects by title, language, or type
- **Secure Admin Dashboard** — JWT-protected CRUD operations for managing projects
- **Dynamic Categories** — projects categorized by programming language and application type
- **Responsive Design** — mobile-friendly layout with custom CSS
- **Cloud Database** — real-time data persistence with Neon PostgreSQL

---

## 🔐 Key Challenges Solved

### 1. Persistent 403 Forbidden Error
The most complex issue of the project. Root causes identified and fixed:
- `CorsFilter` bean conflicting with Spring Security filter chain order
- CORS reconfigured inline inside `SecurityFilterChain` via `.cors(cors -> cors.configurationSource(...))` to eliminate filter conflicts
- JWT token key mismatch in `localStorage` (`portfolio-Token` with capital P)

### 2. H2 In-Memory Database on Production
The application was falling back to H2 instead of connecting to Neon. Fixed by:
- Moving H2 dependency to `test` scope only
- Removing `spring-dotenv` dependency (Render injects environment variables natively)

### 3. CORS Between Domains
Backend blocking requests from Vercel domain. Solved by explicitly listing all allowed origins in `SecurityConfigurations.java`.

---

## 🚀 Getting Started

### Prerequisites
- Java 21+
- Node.js 18+
- Maven 3.9+
- PostgreSQL (or Neon account)

### Backend

```bash
cd backend/portfolio-backend
mvn clean package -DskipTests
java -jar target/portfolio-backend-0.0.1-SNAPSHOT.jar
```

### Frontend

```bash
cd frontend
npm install
npm run dev
```

---

## 🔑 Environment Variables

### Backend (Render / `.env`)

| Variable | Description |
|----------|-------------|
| `DB_URL` | PostgreSQL connection string |
| `DB_USERNAME` | Database username |
| `DB_PASSWORD` | Database password |
| `JWT_SECRET` | Secret key for signing JWT tokens |
| `DDL_AUTO` | Hibernate DDL mode (`validate` for production) |

### Frontend (Vercel / `.env`)

| Variable | Description |
|----------|-------------|
| `VITE_API_URL` | Backend base URL (e.g. `https://your-backend.onrender.com`) |

---

## ☁️ Deployment

### Backend — Render (Docker)
The backend is containerized using a multi-stage Dockerfile:
- **Stage 1:** Maven builds the JAR (`maven:3.9-eclipse-temurin-21`)
- **Stage 2:** Lightweight JRE runs the app (`eclipse-temurin:21-jre`)

### Frontend — Vercel
- Framework: Vite (auto-detected)
- Build command: `npm run build`
- Output directory: `dist`
- `vercel.json` configured to redirect all routes to `index.html` for React Router support

### Database — Neon
Serverless PostgreSQL with SSL required. Connection pooling enabled via Hikari.

---

## 👨‍💻 Author

**Gustavo Tínel**
- LinkedIn: [linkedin.com/in/gustavotinel](https://linkedin.com/in/gustavotinel)
- GitHub: [github.com/gustavotinelvf](https://github.com/gustavotinelvf)

---

*Developed with ☕ and code*
