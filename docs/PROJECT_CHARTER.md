# Project Charter - AgroGuard (Week 1: Idea and Team)

**College:** TOO "Apec PetroTechnical Higher College"  
**Subject:** Using basic software applications and information and communication networks to perform work tasks  
**Group:** SW 1-24  
**Team:** Artur Kurmashev, Maxim Li, Nurdaulet Zhumabay, Ilnur Garifov  
**Repository:** https://github.com/ArtD1808/AgroGuard

---

## 1. App Name & One-Sentence Description

**App Name:** AgroGuard

> **AgroGuard helps commercial greenhouse farmers to monitor and control the microclimate of all their greenhouses remotely and to prevent crop-destroying overheating 2 hours before it happens.**

**Platform:** .NET MAUI mobile app (Android) + FastAPI backend + PostgreSQL database.

---

## 2. Target User (Persona)

**Serik Abenov, 42 years old, commercial farmer, Atyrau region**

Serik Abenov is a 42-year-old commercial farmer who runs three polycarbonate greenhouses (about 1,500 m² in total) near Atyrau, growing tomatoes and cucumbers for local supermarkets and the city bazaar. In spring and summer the air inside a closed greenhouse can jump from 28 °C to over 40 °C in less than two hours, and above 35 °C tomato flowers drop and cucumber leaves burn, so a single heat spike can destroy up to 30% of a harvest worth several million tenge. Because he spends most of the day on the road delivering produce and meeting buyers, he physically cannot watch his greenhouses 24/7, and his simple thermometer alarms react only when the damage is already happening. He needs one app that shows all three greenhouses at a glance, warns him before the temperature becomes dangerous and acts automatically when he cannot.

**Key pains:** sudden temperature spikes · risk of losing a harvest worth millions of tenge · impossible to watch three greenhouses 24/7.

---

## 3. Own Extra Feature (Signature Feature) - Heat Guard

> **Our app can predict greenhouse overheating up to 2 hours in advance and open ventilation automatically before the temperature becomes dangerous because our user Serik manages three greenhouses while constantly on the road and cannot physically react in time when a sudden Atyrau heat spike begins.**

### How it works (linear regression, 2 hours ahead)
1. **Data window:** every 5 minutes the server takes the last 2 hours of air-temperature readings of a greenhouse from PostgreSQL (about 24 points).
2. **Linear regression:** it fits the line T(t) = a + b·t with the least-squares method; b is the heating rate in °C per minute.
3. **2-hour forecast:** the line is extrapolated 120 minutes ahead, and if b > 0 the time to danger is ETA = (T_max - T_now) / b.
4. **Confidence check:** the forecast is used only when there are enough points and the fit is good (R² >= 0.6), so noisy data never triggers false actions.
5. **Preventive action:** Warning if T_max is reached within 2 h; Critical if ETA < 30 min - in Auto mode AgroGuard opens vents and the fan itself, in Manual mode Serik gets a one-tap “Open ventilation” alert.

**Why it is preventive:** a normal alarm answers *“Is it hot now?”*, Heat Guard answers *“Will it be too hot in the next 2 hours?”* - ventilation starts 60-90 minutes earlier and the crop never enters the damaging zone.

### Cold start solved with Crop Profiles
- **Problem:** a newly added greenhouse has no history, so regression is impossible, and the farmer may not know the right limits for a crop.
- **Crop Profiles:** ready presets - Tomatoes, Cucumbers, Strawberries, Greens - store optimal min/max air temperature, air humidity, soil moisture and light hours (e.g. Tomatoes: T_max = 30 °C).
- **One tap:** choosing a crop when creating a greenhouse applies all thresholds instantly, including Heat Guard’s T_max.
- **Learning mode:** until 2 hours of data are collected, Heat Guard uses the profile limit with a safety margin (warning at T_max - 2°C) and shows “Heat Guard is learning...”, then switches to full trend forecasting automatically.

---

## 4. Direction (Track)

**Chosen Track: Agro-intelligence**

**Why we chose this track:**
- Serik’s biggest financial risk is agronomic: heat stress on specific crops, not a lack of buttons.
- Agro-intelligence lets us add real crop knowledge (Crop Profiles) and predictive analytics (Heat Guard) on top of the 8 required modules.
- It works purely with data from PostgreSQL, without extra hardware, so it can be fully demonstrated and tested.
- It turns AgroGuard from a passive remote control into an assistant that makes decisions for a busy farmer.

---

## 5. Feature List (M1-M8)

| # | Module | How it works in AgroGuard |
|---|---|---|
| **M1** | **Authentication** | Serik registers and logs in with username + password (bcrypt hash, JWT token); every greenhouse has an owner_id, so a second test farmer account sees only its own greenhouses and gets 404 for Serik’s. |
| **M2** | **Dashboard** | For each of Serik’s 3 greenhouses it shows 4 parameters - air temperature, air humidity, soil moisture, light - plus last update time, a status banner (OK / Attention / Critical) and the Heat Guard forecast, refreshed every 3 s. |
| **M3** | **Remote Control** | 4 devices per greenhouse - irrigation, ventilation (vents + fan), phyto-lamps, heating - are switched from the app; the new state is saved in PostgreSQL in < 1 s, confirmed in the UI and logged in device_events. |
| **M4** | **Operating Modes** | Auto / Manual switch per greenhouse; in Auto the FastAPI background loop applies Crop Profile rules (soil moisture < 40% → irrigation for 10 min, Heat Guard critical → open vents), tested by UPDATE-ing values in the PostgreSQL console instead of a simulator. |
| **M5** | **Notifications** | When a value changed in the PostgreSQL console leaves the profile range (e.g. air temperature = 45 °C) or Heat Guard predicts overheating, a warning / critical alert appears, closes itself when values normalise, and a tap opens device control. |
| **M6** | **History and Charts** | The sensor_readings table stores 8+ days of seeded history for all 4 parameters; the app draws temperature and soil-moisture charts for Day / Week / Month with threshold lines and highlighted too-hot / too-dry periods. |
| **M7** | **Settings** | Serik selects a Crop Profile per greenhouse and edits min/max for all 4 parameters, irrigation duration, light hours and °C / °F; client and server validation reject impossible values such as -50% humidity or min > max. |
| **M8** | **Failure Handling** | If the internet drops or the FastAPI server is stopped, the app shows a red “No connection to greenhouse” banner with the last known values, does not crash, blocks blind commands and reconnects automatically. |

> No external simulator is used: sensor values live in PostgreSQL and are changed directly in the database console (psql / DBeaver) to demonstrate automation rules and alerts.

---

## 6. Team Setup & Roles

| Member | Role | Design responsibility | Modules | Architecture part |
|---|---|---|---|---|
| **Artur Kurmashev** | Team Lead · Backend Developer | REST API contract, system architecture diagram | M1 (backend: JWT, bcrypt, owner isolation), Heat Guard regression engine | FastAPI service layer; GitHub repository, code review, merging PRs |
| **Maxim Li** | Mobile Developer (.NET MAUI) | Navigation flow; Login, Dashboard and Devices screens | M1 (UI), M2, M3, M8 | .NET MAUI app (MVVM), API client data layer |
| **Nurdaulet Zhumabay** | Database & Automation Engineer | ER diagram / PostgreSQL schema | M4, M5, Crop Profiles data | PostgreSQL database, automation loop, seed data, DB-console demo queries |
| **Ilnur Garifov** | UI/UX Designer · QA · Docs | Visual identity (colours, fonts); History, Alerts and Settings screens | M6, M7 | Chart component; test scenarios, presentations, documentation |

**Repository:** https://github.com/ArtD1808/AgroGuard  
**Workflow:** every member works in a feature branch and opens a Pull Request; the Team Lead reviews and merges into `main`.

---

## 7. Week 1 Deliverables

- [x] App name and one-sentence description
- [x] Target user persona and problem
- [x] Signature feature (Heat Guard) with Crop Profiles for cold start
- [x] Chosen track: Agro-intelligence
- [x] Feature list M1-M8 adapted to AgroGuard
- [x] Team roles and GitHub repository with commits from all four members
