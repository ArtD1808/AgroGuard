# 🌱 AgroGuard

> Smart greenhouse monitoring and control that **predicts overheating 2 hours before it happens**.

**Group:** SW 1-24 · TOO "Apec PetroTechnical Higher College"  
**Team:** Artur Kurmashev · Maxim Li · Nurdaulet Zhumabay · Ilnur Garifov  
**Repository:** https://github.com/ArtD1808/AgroGuard  
**Current stage:** Week 1 - Idea and Team

---

## 📖 About

AgroGuard helps commercial greenhouse farmers to monitor and control the microclimate of all their greenhouses remotely and to prevent crop-destroying overheating 2 hours before it happens.

Most greenhouse apps are **reactive** - they raise an alarm when it is already too hot. AgroGuard is **preventive**:
its signature feature **Heat Guard** runs a linear regression over recent temperature history, forecasts the next
2 hours and opens ventilation *before* the crop is damaged. Ready-made **Crop Profiles** give a brand-new
greenhouse correct thresholds from the first minute.

📄 Full details: [Project Charter](docs/PROJECT_CHARTER.md)

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Mobile frontend** | .NET MAUI (C#, XAML, MVVM) |
| **Backend API** | FastAPI (Python), Pydantic, SQLAlchemy |
| **Database** | PostgreSQL |
| **Security** | JWT + bcrypt |
| **Analytics** | Linear regression (least squares) - Heat Guard |
| **Collaboration** | Git + GitHub (feature branches, pull requests) |

---

## ✨ Key Features

| Feature | Description |
|---|---|
| 🔥 **Heat Guard** | 2-hour overheating forecast (linear regression) + preventive ventilation |
| 🌿 **Crop Profiles** | One-tap optimal thresholds for Tomatoes, Cucumbers, Strawberries, Greens - no cold start |
| 📊 **Multi-greenhouse dashboard** | 4 live parameters for every greenhouse on one screen |
| 🔔 **Smart alerts** | Warnings before and during critical situations |

---

## 🧩 Required Modules

| # | Module |
|---|---|
| **M1** | Authentication |
| **M2** | Dashboard |
| **M3** | Remote Control |
| **M4** | Operating Modes |
| **M5** | Notifications |
| **M6** | History and Charts |
| **M7** | Settings |
| **M8** | Failure Handling |

**Chosen Track: Agro-intelligence** - crop knowledge + predictive analytics on top of the required modules.

Detailed description of every module: [Project Charter → Feature List](docs/PROJECT_CHARTER.md#5-feature-list-m1-m8)

---

## 👥 Team & Roles

| Member | Role | Modules |
|---|---|---|
| **Artur Kurmashev** | Team Lead · Backend Developer | M1 (backend: JWT, bcrypt, owner isolation), Heat Guard regression engine |
| **Maxim Li** | Mobile Developer (.NET MAUI) | M1 (UI), M2, M3, M8 |
| **Nurdaulet Zhumabay** | Database & Automation Engineer | M4, M5, Crop Profiles data |
| **Ilnur Garifov** | UI/UX Designer · QA · Docs | M6, M7 |

## 📂 Repository Structure (Week 1)

```text
AgroGuard/
├── docs/
│   └── PROJECT_CHARTER.md
├── .gitignore
└── README.md
```

> Source code folders will be added in the next stages of the project.
<<<<<<< HEAD

---

## 🗓️ Progress

| Stage | Status |
|---|---|
| Week 1 - Idea and Team | ✅ Done |

---

*Educational project - Group SW 1-24, Atyrau 2026.*
=======
