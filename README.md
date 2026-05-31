# Multimedia Review Platform

> A Letterboxd-style multimedia review web app — full-stack TypeScript with Angular, Express, MongoDB, JWT auth, and real-time review feeds via Socket.io.

## Overview

A full-stack web app where users review and rate movies/TV shows, follow other reviewers, and get real-time updates when followed users post new content. Built to practice production-grade auth, real-time messaging, and search-heavy MongoDB workloads against the TMDb API.

## Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | Angular, TypeScript |
| **Backend** | Node.js, Express.js |
| **Database** | MongoDB (Mongoose) |
| **Real-time** | Socket.io WebSockets |
| **External API** | TMDb (The Movie Database) |
| **Auth** | JWT with refresh token rotation, bcrypt |

## Key Features

- **Genre filtering with multi-tag AND/OR search logic** — compound MongoDB queries backed by text + compound indexes
- **Secure JWT auth flow** — short-lived access tokens, rotated refresh tokens, protected CRUD routes
- **Server-side validation** — Mongoose schema validation + Express middleware request validation
- **Real-time review feed** — Socket.io broadcasts new reviews and live user events to followers
- **MongoDB performance** — compound and text indexes on the review collection for sub-100ms search

## Architecture

```
Angular (TypeScript)  ──HTTP──▶  Express API  ──Mongoose──▶  MongoDB
       ▲                              │                          │
       │                              │                       indexes
       └─────WebSocket (Socket.io)────┘                     (text + compound)
                                      │
                                      └──HTTP──▶  TMDb API
```

## Running Locally

### Prerequisites
- Node.js 18+
- MongoDB running locally (or a connection string)
- TMDb API key

### Setup

```bash
git clone https://github.com/SahilJ10319/LetterBoxdClone.git
cd LetterBoxdClone

# Backend
cd backend
npm install
cp .env.example .env   # set MONGO_URI, JWT_SECRET, TMDB_API_KEY
npm run start

# Frontend (in a second terminal)
cd ../frontend
npm install
ng serve
```

App runs at `http://localhost:4200`. Backend API runs at `http://localhost:3000`.

## What I Learned

- **Refresh token rotation** is non-trivial — revoking old tokens, issuing new ones, and handling race conditions on parallel requests was the most interesting piece of the auth work.
- **MongoDB indexing strategy** matters more than I expected. The first version of multi-tag search took 800ms+; a compound index on `(genres, rating, createdAt)` plus a text index on `title` dropped it under 100ms.
- **Socket.io rooms** are the cleanest way to scope broadcasts to followers without flooding the whole user base.
