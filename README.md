# Valkrypt — Multiplayer RPG with an AI Game Master

A browser-based multiplayer fantasy RPG where **Google Gemini acts as the game master**, narrating the story live and reacting to every player's decision, with tactical turn-based combat, co-op mini-games and a full social layer.

> DAM final project (2025–26) · Institut Pedralbes · team project
> Core developer: **Pariskar Rijal**

![Valkrypt](doc/A_resum_presentacio/captures/p1.png)

---

## Highlights

- 🧙 **AI game master** — Gemini generates the narration in real time and streams it to players as it is written, driven by each party's choices, characters and campaign state.
- ⚙️ **Admin-tunable AI** — model, temperature and output limits are configurable from an admin panel, with a live "test prompt" endpoint and audit logs.
- ⚔️ **Tactical turn-based combat** — party combat with classes, levels, experience and stat progression, plus a shared party inventory.
- 🎮 **Mini-games** — memory, reflex and a real-time **co-op reflex** mode over WebSockets, rewarding XP based on score and speed.
- 🌐 **Real-time multiplayer** — room codes, lobbies, roles, turn choices and in-game chat over WebSockets.
- 👥 **Social layer** — profiles, public profiles, friend requests, presence (online status) and direct chat.
- 📜 **Chronicles** — every campaign's story can be exported as a PDF.
- 🔐 **Secure by design** — JWT auth, bcrypt, email OTP verification, rate limiting, request context and a consistent API response envelope; idempotent turn processing so a repeated action is never applied twice.

---

## Screenshots

| | |
|---|---|
| ![](doc/A_resum_presentacio/captures/p3.png) | ![](doc/A_resum_presentacio/captures/p5.png) |
| ![](doc/A_resum_presentacio/captures/p7.png) | ![](doc/A_resum_presentacio/captures/p9.png) |

---

## Architecture

```
┌────────────────────────────────────────┐
│  Vue 3 SPA (Vite · Pinia · Vue Router) │
│  Three.js / Vanta visuals              │
└──────────┬──────────────────┬──────────┘
           │ REST (axios)     │ WebSocket
┌──────────▼──────────────────▼──────────┐
│  Node.js · Express API                 │
│  auth · rooms · engine · social · admin│
│  middleware: JWT · rate limit ·        │
│  request context · response envelope   │
│  WebSocketManager: rooms, chat, co-op  │
└──────┬──────────────────────┬──────────┘
       │                      │ streaming      
┌──────▼──────┐        ┌──────▼──────────────┐
│  MongoDB    │        │  Google Gemini API  │
│  users,     │        │  AI narration       │
│  rooms,     │        └─────────────────────┘
│  saves,     │
│  sessions   │   + Resend (email OTP) · PDFKit (chronicles)
└─────────────┘
```

---

## Tech stack

| Layer | Technology |
|---|---|
| Frontend | Vue 3, Vite, Pinia, Vue Router, axios, Three.js, Vanta |
| Backend | Node.js, Express, WebSocket (`ws`) |
| AI | Google Gemini API (streaming generation) |
| Data | MongoDB |
| Auth & security | JWT, bcrypt, email OTP (Resend), rate limiting |
| Other | PDFKit, Docker, Docker Compose, PM2 |

---

## API overview

| Area | Examples |
|---|---|
| **Auth** | register, verify OTP, resend OTP, login |
| **Rooms** | create, join, leave, start, choose character/role, submit turn choice |
| **Engine** | narrative context, room history, send action, chronicles, export PDF |
| **Social** | presence heartbeat, profiles, user search, friend requests, chat |
| **Admin** | AI settings (get/update/test), inventory CRUD, user suspension, logs |

---

## Project structure

```
Valkrypt-RPG/
├── backend/
│   ├── controllers/   # Auth, Room, Engine, Narrative (AI), Social, Admin, Game
│   ├── middleware/    # auth, rateLimit, requestContext, responseEnvelope
│   ├── models/        # User, Room, GameSave, Session
│   ├── routes/
│   ├── websocket/     # WebSocketManager
│   └── scripts/       # campaign + narrative seeders
├── frontend/src/
│   ├── views/         # Game, Lobby, Mini-games, Codex, Profile, Admin…
│   ├── components/    # TacticalCombatPanel, PartyInventoryPanel
│   └── services/      # apiClient, WebSocketService, presenceService
└── docker-compose.yml
doc/                   # planning, design (UML, ER, wireframes), technical docs, user manual
```

---

## Run it

```bash
cd Valkrypt-RPG
# backend/.env needs: MONGODB_URI, JWT_SECRET, GEMINI_API_KEY (+ email provider settings)
docker compose up --build
```

Or locally:

```bash
cd Valkrypt-RPG/backend  && npm install && npm run seed:all && npm start
cd Valkrypt-RPG/frontend && npm install && npm run dev
```

---

## Documentation

- [Technical documentation](doc/E_documentacio_tecnica/documentacio_tecnica_2425_Valkrypt.pdf)
- [Design: UML, ER diagram, wireframes](doc/C_disseny/)
- [Planning](doc/B_planificacio/planificacio_2425_Valkrypt.pdf)
- [User manual](doc/I_manual_usuari/manual_usuari_2425_Valkrypt.pdf)
- [Technical presentation](tecnica_2425_Valkrypt.pdf) · [Commercial presentation](comercial_2425_Valkrypt.pdf)

---

## Team

Built as a team for the DAM final project at Institut Pedralbes.

**Pariskar Rijal** — core developer, contributing across the full stack, including:
- Mini-games system (memory, reflex, real-time co-op reflex) and XP reward logic
- API hardening middleware: authentication, rate limiting, request context, response envelope
- WebSocket manager and the central frontend API client
- Tactical combat redesign with levels and experience; admin and user pages
- Final documentation, pitch and delivery

Team members (GitHub): [@Javaguper](https://github.com/Javaguper) · [@zaxtronic](https://github.com/zaxtronic) · a24gaujaghar · [@pariskarrijal](https://github.com/pariskarrijal)

[LinkedIn](https://www.linkedin.com/in/pariskar-rijal-2b25a0257/)
