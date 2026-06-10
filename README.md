# KukuFarm

> **Simamia Kuku Wako. Kwa Akili.**
> *Manage Your Flock. Intelligently.*

[![Platform](https://img.shields.io/badge/Platform-Android-brightgreen.svg)](https://www.android.com)
[![Language](https://img.shields.io/badge/Language-Kotlin-purple.svg)](https://kotlinlang.org)
[![UI](https://img.shields.io/badge/UI-Jetpack%20Compose-blue.svg)](https://developer.android.com/jetpack/compose)
[![Min SDK](https://img.shields.io/badge/Min%20SDK-API%2026-orange.svg)](https://developer.android.com/about/versions/oreo)
[![Architecture](https://img.shields.io/badge/Architecture-Feature--based%20MVVM-brown.svg)](CLAUDE.md)
[![Database](https://img.shields.io/badge/Local%20DB-Room-teal.svg)](https://developer.android.com/training/data-storage/room)
[![Sync](https://img.shields.io/badge/Sync-Firebase%20Firestore-yellow.svg)](https://firebase.google.com/docs/firestore)
[![Payments](https://img.shields.io/badge/Payments-M--Pesa%20Daraja-green.svg)](https://developer.safaricom.co.ke)
[![License](https://img.shields.io/badge/License-MIT-red.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](docs/CONTRIBUTING.md)

An offline-first Android farm management app built for Kenyan smallholder poultry farmers. KukuFarm replaces paper notebooks, WhatsApp logs, and broken spreadsheets with a single intelligent companion that works with or without internet.

---

## Screenshots

> Screenshots will be added once Phase 1 (Core Loop) UI is complete.

| Onboarding | Dashboard | Flock Register |
|:---:|:---:|:---:|
| *Coming soon* | *Coming soon* | *Coming soon* |

| Daily Production Log | Vaccination Schedule | Financial P&L |
|:---:|:---:|:---:|
| *Coming soon* | *Coming soon* | *Coming soon* |

---

## What It Does

| Feature | Description |
|---|---|
| Flock Register | Track every batch of birds — Layers, Broilers, Kienyeji |
| Daily Production Log | Fast egg logging with calendar history and production rate trends |
| Vaccination Schedules | Auto-generated on batch creation, with overdue alerts |
| Sales & M-Pesa | Record sales and trigger Daraja STK Push to buyer phones |
| Feed & FCR Tracking | Log feed purchases, auto-calculate Feed Conversion Ratio per batch |
| Financial Dashboard | Real-time P&L — income, expenses, net profit, trend charts |
| Health Events | Log symptoms, mortality, medications, capture photos with CameraX |
| Role-Based Access | Owner sees everything; workers see only production and health |
| Multi-Farm | Switch between farms, view consolidated KPIs across all locations |
| PDF Reports | Branded P&L reports exportable for bank or investor meetings |

---

## Tech Stack

**Language & UI**
- Kotlin 2.0 + Jetpack Compose (Material 3)
- Compose Navigation for type-safe routing

**Data & Sync**
- Room 2.6+ — local source of truth, always
- Firebase Firestore — background sync when connected
- Firebase Auth — phone number OTP
- WorkManager — sync, reminders, mortality alerts

**Payments & Integrations**
- Daraja API (Safaricom M-Pesa STK Push)
- CameraX for health photo capture
- Vico for Compose-native charts
- iText 7 Community for PDF export

**DI & Testing**
- Hilt for dependency injection
- JUnit 5 + MockK + Turbine for unit and Flow testing

---

## Architecture

Feature-based MVVM. Code is organised by feature, not by layer. Each feature owns its full vertical slice:

```
feature/flock/
  data/      ← Room entity, DAO, repository impl
  domain/    ← model, repository interface, use cases (pure Kotlin)
  ui/        ← Screen, ViewModel, UiState
```

Shared infrastructure lives in `core/` — database setup, theme, Hilt modules, WorkManager jobs.

See [`CLAUDE.md`](CLAUDE.md) for the full package map and [`docs/Kukufarm_prd.md`](docs/Kukufarm_prd.md) for feature specs and data models.

---

## Getting Started

**Requirements**
- Android Studio Ladybug or newer
- JDK 11+
- Android SDK API 26–35
- A connected device or emulator running Android 8.0+

**Build & Run**

```bash
# Clone and open in Android Studio, or build from terminal:

./gradlew assembleDebug          # build debug APK
./gradlew installDebug           # install on connected device

./gradlew test                   # local unit tests
./gradlew connectedAndroidTest   # instrumented tests (requires device)
./gradlew lint                   # lint checks
./gradlew check                  # lint + all tests
```

Run a single test class:
```bash
./gradlew test --tests "dev.korryr.kukufarm.ExampleUnitTest"
```

**Secrets setup** — before building with Firebase or Daraja:

1. Place `google-services.json` in `app/` (not committed to VCS)
2. Add Daraja credentials to `keys.properties` (also gitignored):
   ```
   DARAJA_CONSUMER_KEY=...
   DARAJA_CONSUMER_SECRET=...
   ```

---

## Release Phases

| Phase | Goal | Status |
|---|---|---|
| 1 — Core Loop | Onboarding, flock register, daily log, dashboard | In progress |
| 2 — Financial | Sales, M-Pesa, feed tracking, P&L | Planned |
| 3 — Health | Vaccination schedules, health events, mortality alerts | Planned |
| 4 — Team & Scale | Multi-farm, worker accounts, PDF reports, Firestore sync | Planned |
| 5 — Polish | Crashlytics, performance, accessibility, Play Store | Planned |

---

## Brand Palette

| Token | Hex | Use |
|---|---|---|
| PrimaryBrown | `#5C3D2E` | Buttons, headings |
| GoldenYellow | `#F5A623` | CTAs, highlights, badges |
| CreamBackground | `#FFF8F0` | App background |
| HealthGreen | `#4CAF50` | Good production rate, synced |
| AlertOrange | `#FF9800` | Warnings, pending vaccinations |
| DangerRed | `#F44336` | Mortality alerts, overdue events |

---

## Git Workflow

```
main           protected — release tags only
└── dev        base branch for all PRs
    ├── feature/F01-onboarding
    ├── feature/F02-flock-register
    └── fix/*
```

Always branch from `dev`. Commit format: `feat(flock): add mortality logging with photo capture`

---

## Offline-First Guarantee

Every core feature works without internet. Room is always the source of truth. Firestore sync runs silently in the background via WorkManager when connectivity is available. No spinner should block a farmer from logging their eggs.

---

*Built in Kenya, for Kenya.*
