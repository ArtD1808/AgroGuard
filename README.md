# 🌱 AgroGuard

> Smart greenhouse monitoring and control that **predicts overheating 2 hours before it happens**.

**Group:** SW 1-24 · TOO "Apec PetroTechnical Higher College" (Atyrau)  
**Team:** Artur Kurmashev · Maxim Li · Nurdaulet Zhumabay · Ilnur Garifov  
**Repository:** https://github.com/ArtD1808/AgroGuard  
**Figma Prototype:** https://www.figma.com/design/9NNmGFH6OFR9xb7BcEw9Jh/AgroGuard-%E2%80%94-Interactive-UI-Prototype--Maxim-n-Ilnur-?t=3QxbebN2A01O0MHn-0  
**Current stage:** Week 2 - Design (Brand, Clickable Prototype, Architecture, API & DB Schema)

---

## 📖 About

AgroGuard helps commercial greenhouse farmers to monitor and control the microclimate of all their greenhouses remotely and to prevent crop-destroying overheating 2 hours before it happens.

Most greenhouse apps are **reactive** - they raise an alarm when it is already too hot. AgroGuard is **preventive**:
its signature feature **Heat Guard** runs OLS linear regression `T(t) = a + b*t` over a 2-hour temperature window every 5 minutes (`R^2 >= 0.6`), calculates `ETA = (T_max - T_now) / b`, and opens ventilation *before* the crop is damaged. Ready-made **Crop Profiles** (Tomatoes, Cucumbers, Strawberries, Greens) + **Learning mode** (`T_max - 2 C`) solve the cold-start problem from minute one.

### Key Documents
- 📄 [Project Charter (Week 1)](docs/PROJECT_CHARTER.md)
- 📘 [Week 2 Design Summary PDF](docs/AgroGuard_Week2_Design.pdf)
- 📱 [Interactive Clickable Prototype (390x844, Russian UI)](prototype/index.html)
- 🎨 [Brand Guide & Logo](docs/design/BRAND_GUIDE.md)
- 🗺️ [Navigation Flow & Figma Guide](docs/design/FIGMA_FLOW.md)
- 🏗️ [System Architecture & Stack Justification](docs/design/ARCHITECTURE.md)
- 🔌 [REST API Contract (M1-M8 + Heat Guard)](docs/design/API_CONTRACT.md)
- 🗄️ [PostgreSQL Database Schema & SQL Demo Queries](docs/design/DATABASE_SCHEMA.md)
- 🎤 [Week 2 Defense Guide & Q&A](docs/design/DEFENSE_GUIDE.md)

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Mobile frontend** | .NET MAUI (C#, XAML, MVVM, Android) |
| **Backend API** | FastAPI (Python 3.12), Pydantic v2, SQLAlchemy 2.0 (async) |
| **Database** | PostgreSQL 16 (no external simulator; SQL testing via `psql` / `DBeaver`) |
| **Security** | JWT + bcrypt (strict `owner_id` isolation) |
| **Analytics** | Linear regression (least squares) - Heat Guard |
| **Collaboration** | Git + GitHub (feature branches, pull requests) |

---

## ✨ Key Features

| Feature | Description |
|---|---|
| 🔥 **Heat Guard** | 2-hour overheating forecast (linear regression, `R^2 >= 0.6`) + preventive ventilation |
| 🌿 **Crop Profiles** | One-tap optimal thresholds for Tomatoes, Cucumbers, Strawberries, Greens + Learning mode |
| 📊 **Multi-greenhouse dashboard** | 3 greenhouses of Serik, 5-sec status banner, 4 live parameters |
| 🔔 **Smart alerts & offline resilience** | Actionable alerts (tap -> Devices) + M8 offline banner without crashing |

---

## 🧩 Required Modules (M1-M8)

| # | Module |
|---|---|
| **M1** | Authentication (`JWT + bcrypt`, `owner_id` isolation) |
| **M2** | Dashboard (4 parameters, 3 greenhouses, update timestamp) |
| **M3** | Remote Control (4 devices: irrigation, ventilation, lamps, heating) |
| **M4** | Operating Modes (Auto / Manual) |
| **M5** | Notifications (Warning / Critical alerts) |
| **M6** | History and Charts (8+ days seeded history, 2 charts, Day/Week/Month) |
| **M7** | Settings (Crop Profiles, thresholds, schedules, C/F, RU/KZ, validation) |
| **M8** | Failure Handling (Offline banner, locked commands, auto-recovery) |

**Chosen Track: Agro-intelligence** - crop knowledge + predictive analytics on top of the required modules.

---

## 👥 Team & Roles

| Member | Role | Week 2 Design Ownership | Core Modules |
|---|---|---|---|
| **Artur Kurmashev** | Team Lead · Backend Developer | Architecture diagram, API Contract, Stack justification, PDF, README | M1 (backend), Heat Guard engine |
| **Maxim Li** | Mobile Developer (.NET MAUI) | Navigation flow, Figma guide, Prototype shell & screens 01, 02, 03, 05 | M1 (UI), M2, M3, M8 |
| **Nurdaulet Zhumabay** | Database & Automation Engineer | PostgreSQL ER diagram (9 tables), DB Schema & SQL demo queries | M4, M5, Crop Profiles data |
| **Ilnur Garifov** | UI/UX Designer · QA · Docs | Original logo, Brand Guide, Defense Guide, screens 04, 06, 07, 08 | M6, M7 |

---

## 📂 Repository Structure (Week 2)

```text
AgroGuard/
├── docs/
│   ├── PROJECT_CHARTER.md
│   ├── AgroGuard_Week2_Design.pdf
│   └── design/
│       ├── BRAND_GUIDE.md
│       ├── FIGMA_FLOW.md
│       ├── ARCHITECTURE.md
│       ├── API_CONTRACT.md
│       ├── DATABASE_SCHEMA.md
│       ├── DEFENSE_GUIDE.md
│       └── assets/
│           ├── logo.svg
│           ├── logo_mark.svg
│           ├── architecture.svg / architecture.png
│           ├── er_diagram.svg / er_diagram.png
│           └── navigation_flow.svg / navigation_flow.png
├── prototype/
│   ├── index.html
│   └── screens/
│       ├── 01_login.html
│       ├── 02_dashboard.html
│       ├── 03_dashboard_offline.html
│       ├── 04_heat_guard.html
│       ├── 05_devices_modes.html
│       ├── 06_alerts.html
│       ├── 07_history_charts.html
│       └── 08_settings_profiles.html
├── .gitignore
└── README.md
```

> Working mobile and backend source code will be added starting in Week 3.

---

## 🗓️ Progress

| Stage | Status | Tag |
|---|---|---|
| Week 1 - Idea and Team | ✅ Done | `v0.1-week1` |
| Week 2 - Design & Architecture | ✅ Ready for Review | `v0.2-week2` (planned) |

---

*Educational project - Group SW 1-24, Atyrau 2026.*
