# Show Location Name in Local Weather Page

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Display the location name (city name or coordinates) in the weather cards after successfully fetching weather data.

**Architecture:** Add a location display header above the weather cards. For geolocation, show "Your Location" with coordinates. For manual search, show the city name returned from geocoding API.

**Tech Stack:** JavaScript (vanilla), HTML, CSS (shared.css)

---

## Tasks

### Task 1: Add location header HTML element

**Files:**
- Modify: `local-weather.html:57-63`

- [ ] **Step 1: Add location header element**

Find this code:
```html
        <div id="weather-cards" class="hidden">
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                <div id="yesterday-card" class="card"></div>
                <div id="today-card" class="card"></div>
                <div id="delta-card" class="card"></div>
            </div>
        </div>
```

Replace with:
```html
        <div id="weather-cards" class="hidden">
            <div id="location-header" class="text-center mb-4">
                <h2 id="location-name" class="text-xl font-semibold text-white"></h2>
                <p id="location-coords" class="text-sm text-gray-400"></p>
            </div>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                <div id="yesterday-card" class="card"></div>
                <div id="today-card" class="card"></div>
                <div id="delta-card" class="card"></div>
            </div>
        </div>
```

- [ ] **Step 2: Commit**

```bash
git add local-weather.html
git commit -m "Add location header element for displaying location name"
```

---

### Task 2: Update renderWeatherCards to accept and display location

**Files:**
- Modify: `local-weather.html:144` (fetchWeatherData function)
- Modify: `local-weather.html:169` (renderWeatherCards function)

- [ ] **Step 1: Update fetchWeatherByCoords to pass coordinates as location name**

Find:
```javascript
        async function fetchWeatherByCoords(lat, lon) {
            await fetchWeatherData(lat, lon, 'Your Location');
        }
```

Replace with:
```javascript
        async function fetchWeatherByCoords(lat, lon) {
            const coordsStr = `${lat.toFixed(4)}, ${lon.toFixed(4)}`;
            await fetchWeatherData(lat, lon, 'Your Location', coordsStr);
        }
```

- [ ] **Step 2: Update fetchWeatherByCity to pass city name and coordinates**

Find:
```javascript
                const { latitude, longitude, name } = geoData.results[0];
                console.log('Found location:', name, latitude, longitude);
                await fetchWeatherData(latitude, longitude, name);
```

Replace with:
```javascript
                const { latitude, longitude, name, country } = geoData.results[0];
                console.log('Found location:', name, latitude, longitude);
                const displayName = country ? `${name}, ${country}` : name;
                const coordsStr = `${latitude.toFixed(4)}, ${longitude.toFixed(4)}`;
                await fetchWeatherData(latitude, longitude, displayName, coordsStr);
```

- [ ] **Step 3: Update fetchWeatherData function signature**

Find:
```javascript
        async function fetchWeatherData(lat, lon, locationName) {
```

Replace with:
```javascript
        async function fetchWeatherData(lat, lon, locationName, coordsStr = '') {
```

- [ ] **Step 4: Update renderWeatherCards function call**

Find:
```javascript
                console.log('Weather data fetched successfully');
                renderWeatherCards(yesterdayData.daily, todayData.daily);
```

Replace with:
```javascript
                console.log('Weather data fetched successfully');
                renderWeatherCards(yesterdayData.daily, todayData.daily, locationName, coordsStr);
```

- [ ] **Step 5: Update renderWeatherCards function signature and add location display**

Find:
```javascript
        function renderWeatherCards(yesterday, today) {
```

Replace with:
```javascript
        function renderWeatherCards(yesterday, today, locationName, coordsStr) {
            document.getElementById('location-name').textContent = locationName;
            document.getElementById('location-coords').textContent = coordsStr;
```

- [ ] **Step 6: Commit**

```bash
git add local-weather.html
git commit -m "Add location name display to weather cards"
```

---

## Verification

After implementation:
1. Open `local-weather.html` in browser
2. Click "Use My Location" or search for a city
3. Verify location name and coordinates appear above weather cards
