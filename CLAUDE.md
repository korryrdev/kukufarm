# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

KukuFarm is an **offline-first Android app** for Kenyan smallholder poultry farmers. Built with Kotlin + Jetpack Compose, targeting API 26 (Android 8.0) minimum and API 35 (Android 15).

The full product specification is in `docs/Kukufarm_prd.md` — consult it for feature requirements, data models, and release phases before implementing anything.

## Build & Development Commands

```bash
# Build
./gradlew assembleDebug
./gradlew assembleRelease

# Install on connected device
./gradlew installDebug

# Tests
./gradlew test                    # Local JVM unit tests
./gradlew connectedAndroidTest    # Instrumented tests (requires device/emulator)

# Lint & checks
./gradlew lint
./gradlew check                   # Lint + all tests

# Clean
./gradlew clean
```

Run a single test class:
```bash
./gradlew test --tests "dev.korryr.kukufarm.ExampleUnitTest"
```

## Architecture

The architecture is **feature-based MVVM**. Code is organised by feature first; each feature owns its full vertical slice rather than splitting across global `data/`, `domain/`, and `presentation/` directories.

```
app/src/main/java/dev/korryr/kukufarm/
├── core/                        ← Shared infrastructure, not a feature
│   ├── database/                  KukuFarmDatabase, Room setup
│   ├── network/firestore/         FirestoreService
│   ├── network/daraja/            DarajaService, DarajaApi
│   ├── sync/                      SyncWorker
│   ├── workers/                   DailyReminderWorker, AlertWorker
│   ├── ui/theme/                  KukuFarmTheme, Color, Type, Shape
│   ├── ui/components/             Shared composables
│   └── di/                        Hilt modules
├── feature/
│   ├── onboarding/
│   │   ├── data/                  FarmEntity, FarmDao, FarmRepositoryImpl
│   │   ├── domain/                Farm model, FarmRepository interface, SetupFarmUseCase
│   │   └── ui/                    OnboardingScreen, OnboardingViewModel, OnboardingUiState
│   ├── flock/                     FlockBatch, MortalityLog — same data/domain/ui split
│   ├── production/                DailyProductionLog
│   ├── dashboard/                 Aggregation usecases + DashboardScreen/ViewModel
│   ├── sales/                     Sale + DarajaUseCase
│   ├── finance/                   P&L aggregation usecases + screen
│   ├── feed/
│   ├── health/                    VaccinationEvent, HealthEvent, MedicationLog
│   ├── team/                      Role-based access, worker invitation
│   └── settings/
└── navigation/                  Top-level NavGraph, route destinations
```

Within each feature the layering is: `data/` (Room entities, DAOs, repo impl) → `domain/` (models, repo interface, UseCases — pure Kotlin) → `ui/` (`*Screen.kt`, `*ViewModel.kt`, `*UiState`).

## Key Tech Stack

| Concern | Library |
|---------|---------|
| DI | Hilt |
| Local DB | Room 2.6+ |
| Remote DB | Firebase Firestore + Firebase Auth |
| Payments | Daraja API (M-Pesa) |
| Background | WorkManager |
| Camera | CameraX |
| Charts | Vico |
| PDF export | iText 7 Community |
| Testing | JUnit 5 + MockK + Turbine |

Hilt, Room, Firebase, and others are **not yet added** to `gradle/libs.versions.toml` — add them there (not directly in `app/build.gradle.kts`) before use.

## Dependency Management

All dependency versions are centralized in `gradle/libs.versions.toml` (version catalog). Add new dependencies there first, then reference via `libs.*` in `app/build.gradle.kts`.

## Offline-First Constraint

The app must work without internet. Room is the single source of truth. Firestore sync runs via WorkManager when connectivity is available. Never make a feature depend on network availability for its core function.

## Feature Flags / Release Phases

Development is organized into 5 phases per the PRD:
1. **Core Loop** — Onboarding, flock register, daily production log, basic dashboard
2. **Financial** — Sales, feed expenses, profit/loss
3. **Health** — Vaccination schedule, health events, alerts
4. **Team & Scale** — Multi-farm, worker accounts, reports/PDF export
5. **Polish & Release** — Offline sync, accessibility, Kiswahili localisation

v1.0 is English only. Do not add localisation scaffolding until Phase 5.

## Git Branching

```
main       ← protected, release-only
dev        ← base branch for all PRs
feature/F01-onboarding
feature/F02-flock-register
fix/*
```

Always branch from `dev`, not `main`.

## Brand Colors

The `Color.kt` file currently has placeholder Material 3 defaults. The actual brand palette (from PRD Appendix A):

| Token | Hex |
|-------|-----|
| PrimaryBrown | `#5C3D2E` |
| GoldenYellow | `#F5A623` |
| CreamBackground | `#FFF8F0` |
| HealthGreen | `#4CAF50` |
| AlertOrange | `#FF9800` |
| DangerRed | `#F44336` |

Apply these when building any UI screen — do not use the placeholder purple palette.
