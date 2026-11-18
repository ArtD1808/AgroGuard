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
