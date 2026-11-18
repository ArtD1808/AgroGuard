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

_To be added by Ilnur Garifov._

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

_To be added by Nurdaulet Zhumabay._

---

## 5. Feature List (M1-M8)

_To be added by Nurdaulet Zhumabay._

---

## 6. Team Setup & Roles

_To be added by Maxim Li._

---

## 7. Week 1 Deliverables

_To be added by Artur Kurmashev after review._
