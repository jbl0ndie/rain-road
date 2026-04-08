# Local Weather Page — RainRoad

**Date:** 2026-04-08

## Overview

Add a second page to RainRoad that shows the user's local weather: yesterday's historical data, today's forecast, and the delta between them.

## Pages

| File | Purpose |
|------|---------|
| `rain-road.html` | Existing Compare page — unchanged initially |
| `local-weather.html` | New Local Weather page |
| `shared.css` | Shared styles for both pages |

## Navigation

Header links on both pages to switch between:
- **Compare Forecasts** → links to `rain-road.html`
- **Local Weather** → links to `local-weather.html`

## Local Weather Page Layout

```
┌─────────────────────────────────────────────────────┐
│  [RainRoad]  Compare Forecasts  |  Local Weather    │
├─────────────────────────────────────────────────────┤
│                                                     │
│  ┌───────────┐  ┌───────────┐  ┌───────────┐       │
│  │ Yesterday │  │   Today   │  │   Delta   │       │
│  │  18°/12°  │  │  22°/15°  │  │  ↑4°/↑3°  │       │
│  │  💧 5     │  │  💧 2     │  │  ↓3       │       │
│  │  💨 12    │  │  💨 8     │  │  ↓4       │       │
│  └───────────┘  └───────────┘  └───────────┘       │
│                                                     │
│  [Use My Location]                                  │
│  Or search: [________] [Search]                     │
└─────────────────────────────────────────────────────┘
```

### Weather Cards (Yesterday / Today)

Each card displays:
- **Date label** — "Yesterday" or "Today"
- **Temperature** — high°/low° (e.g., "22°/15°")
- **Rainfall** — with rain icon, in mm
- **Wind speed** — with wind icon, in mph

### Delta Card

Shows change from yesterday to today:
- **Temperature** — day high / night low deltas with ↑/↓ (e.g., "↑4°/↑3°")
- **Rain** — delta with ↓/↓ icon
- **Wind** — delta with ↓/↓ icon
- **Color** — green for increase, red for decrease

### Geolocation Flow

1. On page load, request location via `navigator.geolocation.getCurrentPosition()`
2. **On success** → fetch weather data, display cards
3. **On failure/denial** → show error state with manual location fallback

### Error State

- Message: "Couldn't access your location"
- [Use My Location] button to retry
- Manual input: [________] [Search]
- On search: geocode input, fetch weather, hide error state

## API Integration

### Open-Meteo Endpoints

| Data | Endpoint | Parameters |
|------|----------|------------|
| Geocoding | `geocoding-api.open-meteo.com/v1/search` | `name`, `count=1` |
| Today's forecast | `api.open-meteo.com/v1/forecast` | `latitude`, `longitude`, `daily=temperature_2m_max,temperature_2m_min,rain_sum,wind_speed_10m_max` |
| Yesterday's archive | `api.open-meteo.com/v1/archive` | `latitude`, `longitude`, `start_date`, `end_date`, `daily=...` |

### Parameters

- `temperature_unit=celsius`
- `wind_speed_unit=mph`
- `timezone=auto`
- `forecast_days=14` (for Compare page compatibility)

### Units for Archive API

Same daily parameters as forecast endpoint.

## Styling

### shared.css

Extract from `rain-road.html`:
- Body styles (background, font, centering)
- Header/navigation styles
- Card styles (bg-gray-800, rounded-lg, shadow)
- Weather metric display styles
- Custom scrollbar
- Tailwind imports (CDN link stays in HTML)

### Icons

Reuse SVG icons from `rain-road.html`:
- Rain icon (cloud with rain drops)
- Wind icon (wind swirls)

## Implementation Steps

1. **Create `shared.css`** — Extract shared styles
2. **Update `rain-road.html`** — Load shared.css
3. **Create `local-weather.html`** — Three-card layout, geolocation, API calls
4. **Add navigation** — Links to both pages

## Commit Strategy

Commit at each logical step:
1. Add shared.css
2. Refactor rain-road.html to use shared.css
3. Add local-weather.html with basic layout
4. Add geolocation and API integration
5. Add delta calculations and display
