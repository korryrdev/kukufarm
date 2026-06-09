# KukuFarm — Product Requirements Document

**Version:** 1.0.0  
**Status:** Active  
**Lead Engineer:** DevKorrir (korryrdev)  
**Last Updated:** June 2026  
**Classification:** Internal — Engineering + Product

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Problem Statement](#2-problem-statement)
3. [Vision & Mission](#3-vision--mission)
4. [Target Users & Personas](#4-target-users--personas)
5. [Goals & Success Metrics](#5-goals--success-metrics)
6. [Scope & Out of Scope](#6-scope--out-of-scope)
7. [Feature Specifications](#7-feature-specifications)
8. [Screen Inventory & Navigation](#8-screen-inventory--navigation)
9. [Technical Architecture](#9-technical-architecture)
10. [Data Models](#10-data-models)
11. [Non-Functional Requirements](#11-non-functional-requirements)
12. [Release Plan & Milestones](#12-release-plan--milestones)
13. [Risks & Mitigations](#13-risks--mitigations)
14. [Open Questions](#14-open-questions)
15. [Appendix](#15-appendix)

---

## 1. Executive Summary

**KukuFarm** is an offline-first Android application built for Kenyan smallholder poultry farmers. It replaces fragmented paper records, WhatsApp notes, and broken Excel sheets with a single, intelligent farm management tool that speaks the farmer's language — in KES, kilograms, trays, and Kienyeji.

The app automates what spreadsheets cannot: vaccination schedules, mortality alerts, Feed Conversion Ratio (FCR) tracking per batch, M-Pesa payment capture, and daily production reminders — all working without reliable internet.

KukuFarm is not a data entry tool. It is a **farm intelligence companion**.

---

## 2. Problem Statement

### 2.1 The Farmer's Reality

Smallholder poultry farmers in Kenya manage Layers, Broilers, and Kienyeji birds across one or multiple small farms. Their current tools are:

- **Paper notebooks** — lost, damaged, no aggregation
- **WhatsApp groups** — workers send "100 eggs today" messages with zero structure
- **Excel spreadsheets** — purchased templates that require manual data bridges, have zero reminders, break on mobile, and show KPI dashboards stuck at 0 because Farm Setup was never filled

This creates real business pain:

| Pain Point | Real Consequence |
|---|---|
| No vaccination reminders | Birds die from preventable diseases |
| Manual FCR calculation | Farmer doesn't know which batch is profitable |
| No mortality spike detection | Disease spreads to entire flock before action |
| No income tracking vs feed spend | Farmer operates blind on profit/loss |
| Worker logs nothing consistently | Month-end reconciliation is guesswork |
| No M-Pesa record linking to sales | Revenue leakage and disputes |

### 2.2 The Excel Gap Analysis

The purchased **Poultry_Farm_Management_2026** spreadsheet was analysed. It has 19 sheets including Dashboard, Flock Register, Production Log, Sales Tracker, Feed Expenses, Vet & Health, Labour, Batch Performance, and Annual P&L. Critical gaps found:

- Farm Setup flock counts are **manually maintained** — not linked to Flock Register
- Dashboard KPIs pull from Farm Setup, so show **all zeros** when Farm Setup is empty
- No automated vaccination schedule generation on batch creation
- No mortality anomaly detection
- No reminder system of any kind
- No M-Pesa integration
- No role-based access (owner vs worker)
- No offline capability
- No camera integration for health evidence
- Static charts — no real-time reactivity

KukuFarm solves every one of these gaps.

---

## 3. Vision & Mission

### Vision
> *Every Kenyan poultry farmer — from a backyard flock of 50 to a commercial operation of 10,000 — runs their farm with the same intelligence as an enterprise, from their phone.*

### Mission
> Build the simplest, most reliable poultry farm management tool for the Kenyan market: offline-first, Swahili-aware, M-Pesa-integrated, and smart enough to alert farmers before problems become losses.

### App Tagline
> **"Simamia Kuku Wako. Kwa Akili."**  
> *(Manage Your Flock. Intelligently.)*

---

## 4. Target Users & Personas

### Persona 1 — Mama Wanjiku (Primary User)
- **Age:** 38 | Kiambu County
- **Flock:** 300 ISA Brown layers, sells eggs to local shops and market
- **Device:** Tecno Spark 10, Android 12, 3GB RAM
- **Literacy:** Basic English, comfortable in Kiswahili
- **Pain:** Writes egg counts in a notebook. Forgets vaccination dates. Doesn't know if she's profitable after buying feeds.
- **Needs:** Simple daily logging, vaccine alerts, monthly profit view, M-Pesa record of sales

### Persona 2 — James Kipchoge (Secondary User — Farm Worker)
- **Age:** 24 | Works on a 1,000-bird farm in Nakuru
- **Role:** Feeds birds, collects eggs, logs daily counts
- **Device:** Samsung A05, Android 13
- **Pain:** Boss calls every evening asking for numbers. WhatsApp messages get lost.
- **Needs:** Minimal-friction daily log screen, no access to financial data

### Persona 3 — David Ochieng (Power User)
- **Age:** 45 | Kisumu, runs 3 farms
- **Flock:** 800 Broilers + 500 Layers + 200 Kienyeji across 3 locations
- **Device:** Samsung A54, Android 14
- **Pain:** Manages everything mentally. Can't compare farm performance. Buys feeds in bulk but doesn't track FCR.
- **Needs:** Multi-farm support, batch-level FCR analytics, PDF reports for bank/investor

---

## 5. Goals & Success Metrics

### 5.1 Product Goals

| Goal | Metric | Target |
|---|---|---|
| Daily active logging | % of registered farms that log production daily | > 70% after onboarding week |
| Reduce zero-dashboard problem | Farms with complete flock setup | 100% via guided onboarding |
| Vaccination compliance | Vaccination events marked done on time | > 80% |
| Mortality alerts acted on | Alert → health log created within 24h | > 60% |
| Retention | 30-day retention rate | > 65% |
| Offline reliability | App functional with zero network | 100% core features |
| Crash rate | Firebase Crashlytics | < 0.5% |

### 5.2 Business Goals (Post-MVP)

- 1,000 active farms by end of Phase 4
- Partnership discussions with KARI, KALRO, or feed companies for data insights
- Premium tier revenue through batch analytics and PDF export

---

## 6. Scope & Out of Scope

### In Scope (v1.0 — Phases 1–4)

- Flock management (Layers, Broilers, Kienyeji)
- Daily production logging
- Vaccination & health schedule auto-generation
- Feed & expense tracking
- Sales tracking with M-Pesa STK Push
- Financial P&L dashboard
- Mortality spike detection
- Role-based access (Owner / Worker)
- Offline-first with Firebase Firestore sync
- WorkManager-based daily reminders
- CameraX health photo evidence
- PDF report export
- Multi-farm support

### Out of Scope (v1.0)

- Web dashboard / admin panel
- SMS-based logging (feature phone support)
- IoT sensor integration (automatic egg counters)
- Feed supplier marketplace
- Livestock insurance integration (future Shamba Guard crossover)
- AI/ML disease prediction (Phase 6 consideration)
- Multi-language (Kiswahili Phase 2, English only at launch)

---

## 7. Feature Specifications

---

### F-01 — Onboarding & Farm Setup

**Priority:** P0 — Must Have  
**Phase:** 1

**Description:**  
Guided onboarding that eliminates the "zero dashboard" problem by walking the farmer through farm creation before the app is usable. No farm = no access to features.

**User Stories:**
- As a new user, I want to create my farm profile in under 3 minutes so I can start logging immediately.
- As a farmer with multiple farms, I want to add each farm separately and switch between them.

**Acceptance Criteria:**
- Farmer enters: Farm Name, Location (county selector), Primary Bird Type, Farm Owner name, phone number
- At least one flock batch must be added before the dashboard unlocks
- Onboarding progress indicator (Step 1 of 3)
- Farm data persists in Room DB immediately; no network required
- Returning user skips onboarding and lands on Dashboard

**UI Notes:**
- Full-screen onboarding cards, not a settings screen
- Warm earthy palette: soil brown (#5C3D2E), golden yellow (#F5A623), cream (#FFF8F0)
- Large tappable buttons (min 56dp height) for low-literacy accessibility

---

### F-02 — Flock Register

**Priority:** P0 — Must Have  
**Phase:** 1

**Description:**  
Single source of truth for all bird batches. Adding a batch here automatically triggers vaccination schedule generation and feeds all dashboard aggregations with zero manual bridging.

**User Stories:**
- As a farmer, I want to add a new batch so the app automatically knows my flock size without me filling a separate setup form.
- As a farmer, I want to log bird deaths with a cause so I can track mortality history per batch.

**Acceptance Criteria:**
- Batch fields: Breed, Bird Type (Layers / Broilers / Kienyeji), Date Acquired, Initial Count, Age at Acquisition (weeks), Source (supplier name optional), Purchase Cost
- On batch save: auto-generate vaccination schedule (see F-06)
- Dashboard flock count updates reactively (no manual refresh)
- Mortality logging: date, count, cause (dropdown: Disease / Predator / Unknown / Other), optional photo (CameraX)
- Batch status: Active / Sold Out / Culled
- Age-in-weeks auto-calculated from date acquired, displayed on batch card
- Broiler batch: auto-flag at week 6–8 "Ready for Market" banner

**Business Logic:**
```
currentBirdCount = initialCount - cumulativeMortality - soldBirds
ageInWeeks = (today - dateAcquired).days / 7
productionRate = (eggsToday / currentBirdCount) * 100  [Layers only]
```

---

### F-03 — Daily Production Log

**Priority:** P0 — Must Have  
**Phase:** 1

**Description:**  
The most-used screen in the app. Workers open this every morning and every evening. Optimised for speed — under 30 seconds to log a full day.

**User Stories:**
- As a farm worker, I want to log today's egg count in as few taps as possible.
- As a farm owner, I want to see production history across all batches.

**Acceptance Criteria:**
- Log per batch: eggs collected (AM + PM), cracked/soiled eggs (waste), date (defaults to today)
- Total trays auto-calculated (÷30), displayed live as user types
- Production rate % shown against current bird count
- Calendar view showing 30-day production history with colour coding:
    - Green: ≥ 75% production rate
    - Yellow: 50–74%
    - Red: < 50%
- WorkManager notification fires at 07:00 AM daily: *"Habari za asubuhi! Rekodi mayai ya leo 🥚"*
- If no log by 10:00 PM, second notification fires
- Log entry is editable within 24 hours, locked after that (owner can override)

---

### F-04 — Sales Tracker

**Priority:** P0 — Must Have  
**Phase:** 1

**Description:**  
Records egg sales and bird sales. Optionally triggers M-Pesa STK Push to collect payment from buyer.

**User Stories:**
- As a farmer, I want to record an egg sale and immediately send an M-Pesa payment request to the buyer.
- As a farmer, I want to see which buyers buy most frequently and the total owed.

**Acceptance Criteria:**
- Sale types: Egg Sale, Bird Sale (live weight / per head), Manure Sale
- Fields: Date, Buyer Name, Buyer Phone, Product, Quantity, Unit Price, Payment Method (M-Pesa / Cash / Credit)
- M-Pesa STK Push: if method = M-Pesa, trigger Daraja STK Push to buyer phone
- Payment status: Paid / Pending / Partial
- Running total: Total Sales This Month, Unpaid Balance
- Quick-repeat: "Sell to [last buyer]" shortcut on FAB
- Income flows automatically to Financial Dashboard

---

### F-05 — Feed & Expense Tracker

**Priority:** P0 — Must Have  
**Phase:** 1

**Description:**  
Track every feed purchase and farm expense. Auto-calculate FCR per batch. Alert when feed stock is running low.

**User Stories:**
- As a farmer, I want to know my Feed Conversion Ratio for each batch so I know which batch is actually profitable.
- As a farmer, I want a low-feed-stock alert before I run out.

**Acceptance Criteria:**
- Feed entry: date, feed type (Layers Mash / Broiler Starter / Broiler Finisher / Kienyeji Grower / Custom), brand, quantity (kg), cost (KES), supplier
- Expense entry: category (Medication / Labour / Equipment / Utilities / Transport / Other), date, amount, notes
- FCR per batch = totalFeedKg / totalBirdWeightGained [Broilers] or totalFeedKg / (totalEggsKg) [Layers]
- Running feed stock tracker: current stock kg per feed type
- Low stock alert threshold configurable per farm (default: 7 days of estimated consumption)
- Cost per egg auto-calculated = totalFeedCost / totalEggsProduced

---

### F-06 — Vet & Health Module

**Priority:** P1 — Should Have  
**Phase:** 2

**Description:**  
Auto-generates vaccination schedules on batch creation based on bird type and age. Tracks medications. Enables CameraX health photo capture. Alerts on overdue vaccinations.

**User Stories:**
- As a farmer, I want the app to tell me when to vaccinate my new chicks without me having to look it up.
- As a farmer, I want to take a photo of a sick bird and attach it to a health event.

**Acceptance Criteria:**
- On Batch Creation → auto-generate vaccination schedule:

  **Layers/Kienyeji Schedule (from day of acquisition if day-old chicks):**
  | Day | Vaccine |
  |---|---|
  | Day 7 | Newcastle Disease (eye drop) |
  | Day 14 | Gumboro IBD |
  | Day 21 | Newcastle Booster |
  | Day 28 | Fowl Typhoid |
  | Week 8 | Marek's (if not done at hatchery) |
  | Week 16 | Newcastle + IB (pre-lay) |

  **Broiler Schedule:**
  | Day | Vaccine |
  |---|---|
  | Day 1 | Marek's (hatchery — mark as done) |
  | Day 7 | Newcastle + IB |
  | Day 14 | Gumboro |
  | Day 21 | Newcastle Booster |

- Each event: status (Pending / Done / Skipped), date, vet name (optional), cost, notes
- Overdue vaccination → red badge on Health tab + push notification
- Medication log: drug name, dose, duration, withdrawal period, cost
- Health event log: date, symptoms (checklist), affected count, CameraX photo, outcome
- Mortality spike detection: if daily mortality > 3% of batch → immediate push alert: *"⚠️ Mortality spike detected in [Batch Name]. Check your flock now."*

---

### F-07 — Financial Dashboard & P&L

**Priority:** P0 — Must Have  
**Phase:** 2

**Description:**  
Fully reactive financial overview. All numbers flow from actual entries — zero manual bridging. Owner-only screen.

**User Stories:**
- As a farm owner, I want to see this month's net profit without doing any calculations.
- As a farmer applying for a loan, I want to export a 3-month P&L report as PDF.

**Acceptance Criteria:**
- Summary cards (real-time): Total Income, Total Expenses, Net Profit, Profit Margin %
- Income breakdown: Egg Sales, Bird Sales, Manure Sales
- Expense breakdown: Feed, Medication, Labour, Equipment, Other
- Time filters: This Week / This Month / This Quarter / Custom Range
- Batch P&L: per-batch income vs cost for completed batches
- Trend chart: 6-month income vs expense line chart (Vico library)
- Break-even calculator: *"You need to sell X more trays this month to break even"*
- PDF export: branded KukuFarm report, farm name, date range, all figures

---

### F-08 — Role-Based Access Control

**Priority:** P1 — Should Have  
**Phase:** 3

**Description:**  
Farm owners can invite workers. Workers get a restricted view: only Production Log, Feed Log, and Health Events. No financial data.

**User Stories:**
- As a farm owner, I want my worker to log daily production without seeing my sales or profits.
- As a worker, I want a simplified app that shows me only what I need to do today.

**Acceptance Criteria:**
- Roles: **Owner** (full access) | **Worker** (restricted access)
- Owner invites worker via phone number → Firebase Auth link
- Worker sees: Today's Tasks, Production Log, Feed Log, Health Events, Notifications
- Worker does NOT see: Sales Tracker, Financial Dashboard, Expense breakdown, P&L
- Owner can revoke worker access at any time
- Offline: permissions cached in Room, re-validated on next sync

---

### F-09 — Multi-Farm Support

**Priority:** P1 — Should Have  
**Phase:** 3

**Description:**  
A single account can manage multiple farms. Farms are independent but the owner gets a consolidated cross-farm view.

**User Stories:**
- As a farmer with 3 farms, I want to switch between farms in 2 taps.
- As a farmer, I want to see total income across all my farms in one view.

**Acceptance Criteria:**
- Farm switcher accessible from top navigation bar (farm name + dropdown arrow)
- Each farm has isolated data (Room DB per farmId)
- Consolidated Dashboard: sum of KPIs across all farms with per-farm breakdown collapsible
- Max farms per account: 5 (v1.0 limit, extendable)

---

### F-10 — Offline-First Sync Engine

**Priority:** P0 — Must Have  
**Phase:** 3 (foundation from Phase 1)

**Description:**  
100% of core features work without network. When network is available, data syncs to Firebase Firestore silently.

**Acceptance Criteria:**
- Room DB is the primary data store for all operations
- Firebase Firestore is the sync target, not the source of truth during offline periods
- Conflict resolution strategy: last-write-wins with timestamp comparison
- Sync status indicator: subtle icon in top bar (synced ✓ / pending ⟳ / offline ✗)
- Background sync via WorkManager with exponential backoff
- User never sees a loading spinner for local data operations

---

## 8. Screen Inventory & Navigation

```
Bottom Navigation (Owner):
├── 🏠 Dashboard
├── 🐔 Flock
├── 🥚 Production
├── 💰 Finance
└── ⚙️  More

Bottom Navigation (Worker):
├── 📋 Today
├── 🥚 Log Production
├── 🌾 Log Feed
└── 💉 Health

Screen Tree:
Dashboard
├── [tap KPI card] → Detail screen
├── [tap alert banner] → Alert detail + action

Flock
├── Flock List
│   └── [tap batch] → Batch Detail
│       ├── Mortality Log
│       └── Vaccination Status
└── Add Batch (bottom sheet)

Production
├── Production Log (calendar view)
├── Add Daily Log (bottom sheet)
└── Batch Production History

Finance
├── P&L Dashboard
├── Sales List → Add Sale
├── Expense List → Add Expense
└── Export PDF

More
├── Health & Vet
│   ├── Vaccination Schedule
│   ├── Health Events
│   └── Medication Log
├── Feed Tracker
├── Workers (owner only)
├── Reports
├── Farm Settings
└── App Settings
```

---

## 9. Technical Architecture

### 9.1 Tech Stack

| Layer | Technology | Rationale |
|---|---|---|
| Language | Kotlin 2.0 | Type safety, coroutines, modern Android |
| UI | Jetpack Compose (Material 3) | Declarative, modern, animation support |
| Architecture | MVVM + Clean Architecture | Testable, scalable, separation of concerns |
| DI | Hilt | First-class Android DI, less boilerplate than Koin |
| Local DB | Room 2.6+ | Offline-first, coroutine support, migrations |
| Remote DB | Firebase Firestore | Real-time sync, offline persistence layer |
| Auth | Firebase Auth | Phone number auth (no email needed for farmers) |
| Storage | Firebase Storage | Health event photos |
| Background | WorkManager | Reliable scheduled reminders, sync jobs |
| Camera | CameraX | Health photo capture |
| Charts | Vico | Compose-native charting |
| Payments | Daraja API (M-Pesa) | Kenyan market payment standard |
| PDF | iText 7 Community | Report generation |
| Navigation | Compose Navigation 2.7+ | Type-safe navigation |
| Async | Kotlin Coroutines + Flow | Reactive, lifecycle-aware |
| Testing | JUnit 5 + MockK + Turbine | Unit, integration, Flow testing |

### 9.2 Architecture Layers

```
┌─────────────────────────────────────────┐
│           Presentation Layer            │
│  Compose Screens → ViewModels → UiState │
├─────────────────────────────────────────┤
│             Domain Layer                │
│      UseCases → Domain Models           │
│   (Pure Kotlin, zero Android imports)   │
├─────────────────────────────────────────┤
│              Data Layer                 │
│  Repository → Room DAO + Firestore      │
│         SyncManager (WorkManager)       │
└─────────────────────────────────────────┘
```

### 9.3 Project Structure

```
kukufarm/
├── app/
│   ├── src/main/
│   │   ├── data/
│   │   │   ├── local/
│   │   │   │   ├── db/           ← KukuFarmDatabase.kt
│   │   │   │   ├── dao/          ← FlockDao, ProductionDao, SalesDao...
│   │   │   │   ├── entity/       ← Room entities
│   │   │   │   └── mapper/       ← Entity ↔ Domain mappers
│   │   │   ├── remote/
│   │   │   │   ├── firestore/    ← FirestoreService.kt
│   │   │   │   └── daraja/       ← DarajaService.kt, DarajaApi.kt
│   │   │   ├── repository/       ← Implementations
│   │   │   └── sync/
│   │   │       └── SyncWorker.kt
│   │   ├── domain/
│   │   │   ├── model/            ← Farm, Batch, EggLog, Sale, Feed, VetEvent
│   │   │   ├── repository/       ← Interfaces
│   │   │   └── usecase/
│   │   │       ├── flock/
│   │   │       ├── production/
│   │   │       ├── sales/
│   │   │       ├── finance/
│   │   │       └── health/
│   │   ├── presentation/
│   │   │   ├── dashboard/
│   │   │   ├── flock/
│   │   │   ├── production/
│   │   │   ├── sales/
│   │   │   ├── finance/
│   │   │   ├── health/
│   │   │   ├── feed/
│   │   │   ├── workers/
│   │   │   ├── onboarding/
│   │   │   └── settings/
│   │   ├── workers/
│   │   │   ├── DailyReminderWorker.kt
│   │   │   ├── SyncWorker.kt
│   │   │   └── AlertWorker.kt
│   │   ├── ui/
│   │   │   ├── theme/            ← KukuFarmTheme, Color, Type, Shape
│   │   │   └── components/       ← Shared composables
│   │   └── di/                   ← Hilt modules
├── build-logic/                  ← Convention plugins
└── gradle/
    └── libs.versions.toml        ← Version catalog
```

### 9.4 Key Design Patterns

**UiState Pattern (per screen):**
```kotlin
data class ProductionUiState(
    val isLoading: Boolean = false,
    val todayLogs: List<DailyLogUi> = emptyList(),
    val weeklyTotal: Int = 0,
    val productionRate: Float = 0f,
    val error: String? = null
)
```

**ViewModel Event Pattern:**
```kotlin
sealed interface ProductionEvent {
    data class LogAdded(val batchId: String, val eggs: Int) : ProductionEvent
    data class ValidationError(val message: String) : ProductionEvent
    object SaveSuccess : ProductionEvent
}
```

**Repository Offline-First Pattern:**
```kotlin
override fun getProductionLogs(farmId: String): Flow<List<DailyLog>> =
    productionDao.observeAll(farmId)        // Room emits immediately
        .onStart { syncManager.requestSync() } // trigger background sync
```

---

## 10. Data Models

### Core Entities

```kotlin
// Farm
data class Farm(
    val id: String,           // UUID
    val name: String,
    val county: String,
    val ownerPhone: String,
    val createdAt: LocalDate,
    val isActive: Boolean = true
)

// FlockBatch
data class FlockBatch(
    val id: String,
    val farmId: String,
    val birdType: BirdType,   // LAYERS | BROILERS | KIENYEJI
    val breed: String,
    val initialCount: Int,
    val currentCount: Int,    // derived: initialCount - mortality - sold
    val dateAcquired: LocalDate,
    val ageAtAcquisitionWeeks: Int,
    val purchaseCost: Double,
    val status: BatchStatus,  // ACTIVE | SOLD_OUT | CULLED
    val sourceName: String?
)

// DailyProductionLog
data class DailyProductionLog(
    val id: String,
    val batchId: String,
    val farmId: String,
    val date: LocalDate,
    val eggsAM: Int,
    val eggsPM: Int,
    val crackedEggs: Int,
    val totalEggs: Int,       // derived: eggsAM + eggsPM - crackedEggs
    val productionRate: Float // derived: totalEggs / currentBirdCount
)

// Sale
data class Sale(
    val id: String,
    val farmId: String,
    val saleType: SaleType,   // EGGS | BIRDS | MANURE
    val buyerName: String,
    val buyerPhone: String,
    val quantity: Double,
    val unitPrice: Double,
    val totalAmount: Double,  // derived
    val paymentMethod: PaymentMethod, // MPESA | CASH | CREDIT
    val paymentStatus: PaymentStatus, // PAID | PENDING | PARTIAL
    val mpesaRef: String?,
    val date: LocalDate
)

// FeedExpense
data class FeedExpense(
    val id: String,
    val farmId: String,
    val batchId: String?,
    val feedType: FeedType,
    val brand: String?,
    val quantityKg: Double,
    val costKES: Double,
    val supplier: String?,
    val date: LocalDate
)

// VaccinationEvent
data class VaccinationEvent(
    val id: String,
    val batchId: String,
    val vaccineName: String,
    val scheduledDate: LocalDate,
    val completedDate: LocalDate?,
    val status: VaccineStatus, // PENDING | DONE | OVERDUE | SKIPPED
    val vetName: String?,
    val costKES: Double?,
    val notes: String?
)

// HealthEvent
data class HealthEvent(
    val id: String,
    val batchId: String,
    val farmId: String,
    val date: LocalDate,
    val symptoms: List<String>,
    val affectedCount: Int,
    val photoUrl: String?,
    val outcome: String?,
    val vetConsulted: Boolean
)

// MortalityLog
data class MortalityLog(
    val id: String,
    val batchId: String,
    val date: LocalDate,
    val count: Int,
    val cause: MortalityCause, // DISEASE | PREDATOR | UNKNOWN | OTHER
    val photoUrl: String?,
    val notes: String?
)
```

---

## 11. Non-Functional Requirements

### 11.1 Performance
- App cold start < 2 seconds on a 3GB RAM Android Go device
- Dashboard KPI render < 100ms (from Room cache)
- Log entry save < 50ms (Room write)
- Smooth 60fps scrolling on all list screens

### 11.2 Offline Reliability
- 100% of data entry features work without network
- Sync queue survives app kill and device restart (WorkManager persistence)
- Maximum sync lag: 30 seconds after network restoration

### 11.3 Security

> ⚠️ **Security Review Required — All of the below must be verified before each release.**

| Risk | Mitigation |
|---|---|
| Daraja API credentials exposed | Stored in `keys.properties`, excluded from VCS; fetched via BuildConfig only |
| Firebase config in repo | `google-services.json` in `.gitignore`; env-specific configs per build variant |
| User PII (phone numbers, sales data) | Encrypted at rest via Android Keystore + SQLCipher for Room |
| Insecure HTTP to Daraja | `network_security_config.xml` — HTTPS enforced, certificate pinning on Daraja endpoint |
| Role bypass (worker sees finance) | Role checked server-side in Firestore Security Rules, not only in UI |
| Photo upload without validation | MIME type + file size validation before Firebase Storage upload |
| WorkManager data payloads | No sensitive data in WorkManager `Data` objects — use IDs only |

### 11.4 Accessibility
- Minimum touch target: 48dp × 48dp (Material 3 standard)
- All interactive elements have content descriptions (TalkBack support)
- Colour is never the only differentiator (icons + labels always paired)
- Support for system font scaling up to 1.5×

### 11.5 Device Support
- Minimum SDK: API 26 (Android 8.0) — covers ~95% of Kenyan Android market
- Target SDK: API 35 (Android 15)
- Screen sizes: 5" to 6.7" — no tablet optimisation in v1.0
- Memory: tested on 2GB RAM devices

### 11.6 Localisation
- v1.0: English
- v1.1 target: Kiswahili (string resources externalised from day 1 — no hardcoded strings)

---

## 12. Release Plan & Milestones

### Phase 1 — Core Loop *(Target: 6 weeks)*
> Goal: A farmer can add a flock, log daily production, and see a live dashboard.

- [ ] Project scaffolding (MVVM + Clean Architecture + Hilt + Room)
- [ ] KukuFarm design system (theme, typography, colour tokens, shared components)
- [ ] Onboarding flow (Farm Setup + first batch creation)
- [ ] Flock Register (CRUD + mortality logging)
- [ ] Daily Production Log (log, calendar view, production rate)
- [ ] Dashboard (reactive KPIs — zero manual bridging)
- [ ] WorkManager daily reminder notifications
- [ ] Unit tests for all domain UseCases

**Definition of Done:** A farmer can install the app, add a farm, add a batch of 100 chickens, log today's 80 eggs, and see the Dashboard show "80 eggs today, 80% production rate" — all offline.

---

### Phase 2 — Financial Intelligence *(Target: 4 weeks)*
> Goal: A farmer knows exactly how much they earned and spent this month.

- [ ] Sales Tracker (egg sales, bird sales)
- [ ] M-Pesa Daraja STK Push integration
- [ ] Feed & Expense Tracker
- [ ] FCR calculation per batch
- [ ] Financial Dashboard (P&L, income breakdown, expense breakdown)
- [ ] Vico charts (6-month trend)
- [ ] Low feed stock alerts
- [ ] Integration tests for financial flows

---

### Phase 3 — Health & Vet Intelligence *(Target: 4 weeks)*
> Goal: A farmer never misses a vaccination. Disease is detected early.

- [ ] Auto-vaccination schedule generation on batch creation
- [ ] Vaccination status UI + overdue alerts
- [ ] Health event log + CameraX photo capture
- [ ] Firebase Storage integration (photos)
- [ ] Mortality spike detection algorithm + push notifications
- [ ] Medication log
- [ ] Broiler market-weight auto-flag

---

### Phase 4 — Team & Scale *(Target: 4 weeks)*
> Goal: Multi-farm support. Workers can log without seeing financials.

- [ ] Firebase Auth (phone number OTP)
- [ ] Role-based access (Owner / Worker)
- [ ] Worker invitation flow
- [ ] Multi-farm support + consolidated dashboard
- [ ] Firebase Firestore sync engine
- [ ] Offline conflict resolution
- [ ] PDF report export (iText 7)

---

### Phase 5 — Polish & Release *(Target: 2 weeks)*
> Goal: Production-ready. Play Store ready.

- [ ] Firebase Crashlytics integration
- [ ] Analytics events (Firebase Analytics — privacy-first, no PII)
- [ ] UI polish pass (animations, transitions, loading states)
- [ ] Performance profiling (Macrobenchmark)
- [ ] Accessibility audit
- [ ] Play Store listing (screenshots, description, privacy policy)
- [ ] Beta testing with 10 real farmers (Kiambu / Nakuru)
- [ ] Final security audit

---

## 13. Risks & Mitigations

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| Daraja sandbox ≠ production behaviour | High | High | Test with real Daraja production credentials early in Phase 2 |
| Farmers don't log consistently | High | High | WorkManager reminders + ultra-simple worker log screen (< 3 taps) |
| Room migration breakage on update | Medium | High | Strict migration scripts + AutoMigration annotations from v1 |
| Firebase costs spike with photo uploads | Medium | Medium | Compress photos to max 500KB before upload, set Storage rules |
| Device storage full (photos) | Low | Medium | Store photos in external cache dir, offer cloud-only option |
| Firestore Security Rules misconfiguration | Medium | Critical | Dedicated security rule unit tests (firebase-functions-test) |
| Play Store rejection | Low | Medium | Review policy compliance checklist before submission |

---

## 14. Open Questions

| # | Question | Owner | Priority |
|---|---|---|---|
| OQ-01 | Should vaccination schedules be fully configurable by the farmer (custom breeds, custom schedules) or should v1.0 only ship with ISA Brown / Cobb 500 / Kienyeji presets? | Product | High |
| OQ-02 | What is the M-Pesa Daraja paybill / till number strategy for Phase 2? Will the app use the farmer's own Daraja credentials or a KukuFarm business till? | Engineering | High |
| OQ-03 | Should the worker role require the worker to have a KukuFarm account, or can they access a farm with just a PIN sent by the owner? | Product | Medium |
| OQ-04 | Should PDF reports be branded with KukuFarm logo and farm logo, or plain text only for v1.0? | Design | Low |
| OQ-05 | Is Kiswahili localisation in scope for Phase 5 (before Play Store launch) or Phase 6? | Product | Medium |
| OQ-06 | Should multi-farm consolidated dashboard be part of Phase 4 free tier or a premium feature? | Business | Medium |

---

## 15. Appendix

### A. KukuFarm Colour System

| Token | Hex | Usage |
|---|---|---|
| `PrimaryBrown` | `#5C3D2E` | Primary buttons, headings |
| `GoldenYellow` | `#F5A623` | CTAs, highlights, badges |
| `CreamBackground` | `#FFF8F0` | App background |
| `EggWhite` | `#FEFEFE` | Card surfaces |
| `HealthGreen` | `#4CAF50` | Good production rate, synced status |
| `AlertOrange` | `#FF9800` | Warnings, pending vaccinations |
| `DangerRed` | `#F44336` | Mortality alerts, overdue events |
| `TextPrimary` | `#1A1A1A` | Body text |
| `TextSecondary` | `#757575` | Supporting text, labels |

### B. Vaccination Schedule Reference

**Day-Old Chick Layers / Kienyeji:**

| Age | Vaccine | Route |
|---|---|---|
| Day 7 | Newcastle Disease (Clone 30 / Hitchner B1) | Eye drop |
| Day 14 | Gumboro IBD (Intermediate) | Drinking water |
| Day 21 | Newcastle Booster (La Sota) | Drinking water |
| Day 28 | Fowl Typhoid (ADE 40) | Injection |
| Week 6 | Infectious Bronchitis | Drinking water |
| Week 8 | Fowl Pox | Wing web stab |
| Week 16 | Newcastle + IB combo (pre-lay) | Drinking water |

**Day-Old Broilers:**

| Age | Vaccine | Route |
|---|---|---|
| Day 1 | Marek's | Hatchery injection |
| Day 7 | Newcastle + IB | Eye drop |
| Day 14 | Gumboro | Drinking water |
| Day 21 | Newcastle Booster | Drinking water |

### C. FCR Reference Values

| Bird Type | Good FCR | Acceptable FCR | Poor FCR |
|---|---|---|---|
| Broilers (Cobb 500) | < 1.8 | 1.8 – 2.2 | > 2.2 |
| Layers (ISA Brown) | < 2.0 kg/kg egg | 2.0 – 2.5 | > 2.5 |
| Kienyeji | < 3.5 | 3.5 – 4.5 | > 4.5 |

### D. Git Workflow (Team Standard)

```
main            ← protected, release-only
└── dev         ← base branch for all PRs
    ├── feature/F01-onboarding
    ├── feature/F02-flock-register
    ├── feature/F03-production-log
    └── fix/dashboard-zero-kpi
```

Branch rules:
- All feature branches cut from `dev`
- PRs require 1 reviewer approval minimum
- No direct push to `main` or `dev`
- Commit format: `feat(flock): add mortality logging with photo capture`

---

*Document owned by korryrdev. Review cycle: every sprint (2 weeks). Major changes require version bump.*

*KukuFarm — Built in Kenya, for Kenya. 🇰🇪*