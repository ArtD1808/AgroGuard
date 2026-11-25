# REST API Contract - AgroGuard

**Owner:** Artur Kurmashev (Team Lead / Backend Developer)  
**Base URL:** `/api` (local dev: `http://localhost:8000/api`, Android emulator: `http://10.0.2.2:8000/api`)  
**Auth Scheme:** `Authorization: Bearer <access_token>` (JWT HS256, 24h expiry)  
**Content Type:** `application/json; charset=utf-8`

---

## 1. Endpoint Summary (M1 - M8 + Heat Guard)

| Module | Method | Endpoint | Auth | Purpose |
|---|---|---|---|---|
| **M1** | `POST` | `/api/auth/register` | Public | Register a new farmer account (bcrypt hash) |
| **M1** | `POST` | `/api/auth/login` | Public | Authenticate and issue JWT access token |
| **M1** | `GET` | `/api/auth/me` | JWT | Return current farmer profile (`temp_unit`, `language`) |
| **M2** | `GET` | `/api/greenhouses` | JWT | List only greenhouses where `owner_id == current_user.id` |
| **M2** | `GET` | `/api/greenhouses/{id}/dashboard` | JWT + Owner | Live 4 readings, status banner, mode, last updated, compact Heat Guard |
| **M3** | `GET` | `/api/greenhouses/{id}/devices` | JWT + Owner | Current state of 4 devices + recent `device_events` log |
| **M3** | `POST` | `/api/greenhouses/{id}/devices/{type}` | JWT + Owner | Toggle device (`irrigation`, `ventilation`, `lamps`, `heating`) ON/OFF |
| **M4** | `PATCH` | `/api/greenhouses/{id}/mode` | JWT + Owner | Switch greenhouse operating mode (`auto` / `manual`) |
| **M5** | `GET` | `/api/greenhouses/{id}/alerts` | JWT + Owner | List active and resolved alerts (`?status=active\|resolved\|all`) |
| **M5** | `POST` | `/api/greenhouses/{id}/alerts/{alert_id}/resolve` | JWT + Owner | Manually acknowledge/resolve an alert |
| **M6** | `GET` | `/api/greenhouses/{id}/history` | JWT + Owner | Time-series telemetry (`?period=day\|week\|month`) + dry-soil intervals |
| **M7** | `GET` | `/api/crop-profiles` | JWT | List 4 built-in Crop Profiles (Tomatoes, Cucumbers, Strawberries, Greens) |
| **M7** | `PUT` | `/api/greenhouses/{id}/settings` | JWT + Owner | Update Crop Profile, min/max thresholds, watering duration, lighting hours |
| **M7** | `PATCH` | `/api/auth/preferences` | JWT | Update user `temp_unit` (`C` / `F`) and `language` (`ru` / `kz`) |
| **Heat Guard** | `GET` | `/api/greenhouses/{id}/heat-guard` | JWT + Owner | Full 2h OLS forecast (`slope`, `r_squared`, `eta_minutes`, `risk_level`, points, learning progress) |
| **M8** | `GET` | `/api/health` | Public | Lightweight ping (`{"status":"ok"}`) used by `ApiClient` connectivity check |

---

## 2. Key JSON Payloads

### M1: `POST /api/auth/login`
Request:
```json
{
  "username": "serik.abenov",
  "password": "StrongPassword123"
}
```
Response `200 OK`:
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIs...",
  "token_type": "bearer",
  "user": {
    "id": 1,
    "username": "serik.abenov",
    "full_name": "Серик Абенов",
    "temp_unit": "C",
    "language": "ru"
  }
}
```

### M2: `GET /api/greenhouses/1/dashboard`
Response `200 OK`:
```json
{
  "greenhouse_id": 1,
  "name": "Теплица 1",
  "crop_profile": "tomatoes",
  "mode": "auto",
  "status": "warning",
  "status_message_ru": "Внимание: перегрев через 24 мин",
  "recorded_at": "2026-11-19T14:32:10+05:00",
  "readings": {
    "air_temp_c": 27.6,
    "air_humidity_pct": 62.0,
    "soil_moisture_pct": 54.0,
    "light_lux": 14200
  },
  "thresholds": {
    "air_temp_min": 18.0,
    "air_temp_max": 30.0,
    "air_humidity_min": 55.0,
    "air_humidity_max": 75.0,
    "soil_moisture_min": 45.0,
    "soil_moisture_max": 70.0,
    "light_lux_min": 8000
  },
  "heat_guard": {
    "risk_level": "critical",
    "eta_minutes": 24,
    "slope_c_per_min": 0.10,
    "r_squared": 0.91,
    "is_learning": false
  }
}
```

### M3: `POST /api/greenhouses/1/devices/irrigation`
Request:
```json
{
  "is_on": true,
  "duration_minutes": 10
}
```
Response `200 OK`:
```json
{
  "device_id": 1,
  "device_type": "irrigation",
  "is_on": true,
  "auto_off_at": "2026-11-19T14:42:10+05:00",
  "updated_at": "2026-11-19T14:32:10+05:00",
  "event_id": 412,
  "source": "manual"
}
```

### Heat Guard: `GET /api/greenhouses/1/heat-guard`
Response `200 OK` (active forecast or cold-start learning):
```json
{
  "greenhouse_id": 1,
  "t_max_c": 30.0,
  "t_now_c": 27.6,
  "predicted_temp_2h_c": 39.6,
  "slope_c_per_min": 0.10,
  "r_squared": 0.91,
  "eta_minutes": 24,
  "risk_level": "critical",
  "is_learning": false,
  "learning_minutes_collected": 120,
  "learning_minutes_required": 120,
  "fallback_warn_temp_c": 28.0,
  "history_points": [
    {"minute_offset": -120, "temp_c": 15.6},
    {"minute_offset": 0, "temp_c": 27.6}
  ],
  "forecast_points": [
    {"minute_offset": 24, "temp_c": 30.0},
    {"minute_offset": 120, "temp_c": 39.6}
  ]
}
```

### M7: `PUT /api/greenhouses/1/settings`
Request:
```json
{
  "crop_profile_code": "tomatoes",
  "min_air_temp": 18.0,
  "max_air_temp": 30.0,
  "min_air_humidity": 55.0,
  "max_air_humidity": 75.0,
  "min_soil_moisture": 45.0,
  "max_soil_moisture": 70.0,
  "min_light_lux": 8000,
  "watering_duration_min": 10,
  "light_start_hour": 6,
  "light_end_hour": 20
}
```

---

## 3. Standard Error Codes & Security Rules

| HTTP Status | Code | When Returned | Example JSON |
|---|---|---|---|
| `401 Unauthorized` | `INVALID_CREDENTIALS` / `TOKEN_EXPIRED` | Wrong login/password or missing/expired JWT | `{"detail": "Неверный логин или пароль"}` |
| `403 Forbidden` / `404 Not Found` | `GREENHOUSE_NOT_FOUND` | Authenticated farmer requests a `greenhouse_id` belonging to another `owner_id` (404 prevents ID enumeration) | `{"detail": "Теплица не найдена"}` |
| `422 Unprocessable Entity` | `VALIDATION_ERROR` | Invalid settings in M7 (e.g., `min_soil_moisture = -50`, `min_air_temp >= max_air_temp`, `watering_duration_min > 60`) | `{"detail": "Влажность почвы должна быть от 0% до 100%"}` |
| `503 Service Unavailable` | `DB_UNAVAILABLE` | PostgreSQL unreachable; triggers M8 offline banner on client | `{"detail": "Сервер временно недоступен"}` |
