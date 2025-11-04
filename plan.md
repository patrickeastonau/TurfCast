# Lawn Irrigation Notification App - Project Plan

## Overview
Mobile-responsive web app that sends weekly lawn irrigation notifications based on rainfall data, grass type, sprinkler type, and weather forecasts.

**Tech Stack**: Reflex (Python web framework), Open-Meteo API (free weather data), modern SaaS UI design

**Color Scheme**: Primary orange, Secondary gray, Font Roboto

---

## Phase 1: Core UI & User Settings ✅
**Goal**: Build the main settings interface where users input their 4 key parameters

- [x] Create base app layout with modern SaaS styling (orange primary, gray secondary, Roboto font)
- [x] Build settings form with 4 inputs:
  - Grass type selector (Kikuyu, Couch/Bermuda, Buffalo, Tall Fescue)
  - Australian postcode input (4-digit validation)
  - Sprinkler type selector (Oscillating, Fixed/Dome, Rotary/Gear-drive, Impact, Dripline)
  - Weekly notification time picker (day + time)
- [x] Implement local storage for settings (no backend/login required)
- [x] Add postcode → lat/long resolution with nearest weather station display
- [x] Create responsive mobile-first design with cards, shadows, gradients

---

## Phase 2: Weather Data Integration & Rainfall Calculation ✅
**Goal**: Connect to Open-Meteo API and compute weekly rainfall + 48h forecast

- [x] Integrate Open-Meteo API (free, no API key needed)
  - Fetch last 7 days daily precipitation (observed rainfall)
  - Fetch next 48h precipitation forecast
- [x] Implement rainfall calculation logic:
  - Weekly total (configurable Mon-Sun or Sun-Sat, default Sun-Sat)
  - Seasonal target determination based on grass type and current season
  - Deficit calculation: max(0, target - observed)
- [x] Build sprinkler flow rate mapping (mm/min for each sprinkler type)
- [x] Calculate watering minutes: deficit × (minutes per 10mm), rounded to nearest 5 min
- [x] Implement emoji status logic (🌧 Heavy rain, 🌦 Light rain, ☀️ Dry, 🔥 Very dry)
- [x] Display weather station info, coordinates, and data sources
- [x] Create comprehensive postcode database with 42 major Australian locations
- [x] Build results card showing all calculation metrics with icons
- [x] Add split watering guidance for sessions >45 minutes

---

## Phase 3: Notification System & Results Display
**Goal**: Create notification scheduling and detailed lawn care recommendations

- [ ] Build results dashboard showing:
  - Current week summary (week ending date, season, target)
  - Observed rainfall vs target with visual progress indicator
  - 48h forecast display
  - Emoji status with recommendation
  - Watering minutes calculation breakdown
- [ ] Create detailed "Lawn Care Guide" section:
  - Watering recommendation with split watering guidance
  - Dry-week watering guide (explain formula)
  - Seasonal reminders (Seasol, fertilizer tips)
- [ ] Implement notification preview (compact one-line format)
- [ ] Add browser notification permission handling
- [ ] Create scheduled notification system (weekly at user-specified time)
- [ ] Handle special cases (Winter watering rules for different grass types)
- [ ] Polish UI with smooth transitions, hover states, and micro-interactions

---

## Acceptance Tests
- [x] Change grass type → target updates and minutes re-compute correctly
- [x] Change sprinkler type → minutes per 10 mm mapping applied
- [x] Different postcodes return different station/coords and totals
- [x] Emoji/status flips correctly across scenarios (wet/dry/very dry)
- [ ] Push fires at configured time and opens full note

---

## Technical Specifications

**Grass Type Targets (mm/week)**:
- Kikuyu: Spring 20, Summer 25, Autumn 15, Winter 0*
- Couch/Bermuda: Spring 15, Summer 25, Autumn 10, Winter 0
- Buffalo: Spring 20, Summer 25, Autumn 15, Winter 0
- Tall Fescue: Spring 20, Summer 30, Autumn 20, Winter 10
(*Winter: alert only if <10mm for 2 consecutive weeks)

**Sprinkler Flow Rates**:
- Oscillating: ~0.5 mm/min (10mm per 20min)
- Fixed/Dome: ~0.5 mm/min (10mm per 20min)
- Rotary/Gear-drive: ~0.33 mm/min (10mm per 30min)
- Impact: ~0.4 mm/min (10mm per 25min)
- Dripline: ~0.25 mm/min (10mm per 40min)

**Emoji Status Logic**:
- 🌧 Heavy rain/Skip: forecast ≥25mm OR observed ≥target
- 🌦 Light rain/Monitor: 5-20mm forecast OR observed within ±3mm of target
- ☀️ Dry/Water needed: forecast <5mm AND observed <target by >3mm
- 🔥 Very dry/Deep watering: deficit >15mm AND forecast <5mm

**Data Source**: Open-Meteo API (Option A - fastest MVP path)
- precipitation_sum daily for last 7 days and next 2 days
- Free, no API key required
- https://open-meteo.com/en/docs

**Timezone**: Australia/Melbourne
**Storage**: Local storage (no account/login)
**UI Style**: Modern SaaS (Linear/Stripe inspired) with orange primary, gray secondary

---

## Current Status
Phase 2 complete. Moving to Phase 3: Notification System & Results Display.
