# Brand Guide & Visual Identity - AgroGuard

**Owner:** Ilnur Garifov (UI/UX Designer, QA, Docs)  
**Target User:** Serik Abenov (42 y.o., commercial greenhouse farmer, Atyrau region)  
**Stage:** Week 2 - Design

---

## 1. Original Logo

AgroGuard uses a custom geometric brand mark designed specifically for preventive greenhouse monitoring:

- **Full Logo:** [logo.svg](assets/logo.svg)
- **App Icon / Mark:** [logo_mark.svg](assets/logo_mark.svg)

![AgroGuard Logo](assets/logo.svg)

### Visual Meaning (3 layered elements)
1. **Pitched Shield Silhouette:** combines a protective shield ("Guard") with the geometry of a polycarbonate greenhouse arch.
2. **Botanical Leaf:** represents healthy crops (tomatoes and cucumbers) protected inside the structure.
3. **Central Thermo-Indicator + Apex Thermal Arc:** visually symbolizes **Heat Guard** - predicting rising temperatures 2 hours ahead before heat stress damages flowers and leaves.

### Usage Rules
- Minimum icon size on Android launcher / app header: `32 x 32 dp`.
- Clear space around the mark: at least `15%` of the icon width on every side.
- Do not recolor the thermo-indicator to blue or grey; amber/red contrast communicates heat protection instantly.

---

## 2. Color Palette (5 Core Colors)

Designed specifically for **bright outdoor sunlight in Atyrau** (high luminance contrast, zero dark-mode glare):

| Role | Name | HEX | Contrast vs Background | Why it fits Serik Abenov |
|---|---|---|---|---|
| **Primary Brand / OK** | Deep Agro Green | `#1B5E20` | `8.1:1` (AAA) on `#F6F8F5` | Instant association with healthy crops; readable even under direct summer sun |
| **App Background** | Warm Off-White | `#F6F8F5` | Base surface | Prevents screen mirror-glare outdoors (unlike dark themes) and reduces eye fatigue |
| **Primary Text** | Charcoal Text | `#1A1F1C` | `15.4:1` (AAA) on `#F6F8F5` | Crisp legibility in 5 seconds while glancing at the phone on the road |
| **Warning / Heat Guard** | Warning Amber | `#E67E22` | High visibility accent | Draws immediate attention to early 2-hour Heat Guard warnings before critical damage |
| **Critical / Offline / Error** | Critical Red | `#D32F2F` | `5.3:1` (AA Large / AAA Bold) | Unmistakable emergency signal for >35 C overheating, dry soil zones, M8 offline banner, and invalid input |

### Supporting Surface Tokens
- Card Background: `#FFFFFF`
- Card Border: `#D5E0D6`
- Secondary / Muted Text: `#526056`
- Soft Green Banner / Active Chip Fill: `#E8F1E9`
- Soft Amber Fill: `#FDF2E6`
- Soft Red Fill: `#FDECEC`

---

## 3. Typography & Sizing

- **Primary Font Family:** `Inter` / `Manrope` (fallback on Android / Windows: `Roboto` / `Segoe UI`).
- **Numeric Style:** `tabular-nums` (`font-variant-numeric: tabular-nums`) so live sensor numbers refreshing every 3 seconds never jitter horizontally.

| Element | Font Size | Weight | Usage |
|---|---|---|---|
| **Live Sensor Value** | `28 - 34 sp` | ExtraBold (`800`) | Air temp, humidity, soil moisture, lux (readable in < 5 sec) |
| **5-Second Status Banner Title** | `18 - 20 sp` | ExtraBold (`800`) | "Все в норме" / "Внимание: растет нагрев" / "Критично" |
| **Screen Header / Card Title** | `16 - 18 sp` | Bold (`700 - 800`) | Section headings, device names |
| **Body & Alert Descriptions** | `13 - 15 sp` | SemiBold (`600`) | Alert details, mode explanations |
| **Range Hints & Timestamps** | `11 - 12 sp` | SemiBold (`600`) | "Норма: 18 - 30 C", "Обновлено: 2 с назад" |

---

## 4. Ergonomics & Touch Targets

Serik often checks the app between deliveries or right inside the greenhouse with wet hands or work gloves:

- **Primary CTA buttons ("Войти", "Открыть вентиляцию", "Сохранить"):** minimum height `48 - 52 dp`, full width, rounded corners `14 dp`.
- **Device ON/OFF toggles:** minimum `76 x 42 dp` pill buttons with explicit text labels (`ON` / `OFF`) so state is never ambiguous for color-blind users or in bright sunlight.
- **Bottom Navigation Bar:** `68 dp` height, 4 clear tabs (`Главная`, `Устройства`, `Алерты`, `История`) + red unread badge on `Алерты`.
- **Language Support:** Russian UI by default with one-tap `RU / KZ` switcher in Settings (M7) and `°C / °F` unit toggle.
