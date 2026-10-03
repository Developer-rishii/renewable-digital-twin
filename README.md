# 🌿 EcoWatch — Renewable Digital Twin

> **A real-time atmospheric simulation engine and renewable energy digital twin for Indian cities.**

EcoWatch is an interactive, browser-based digital twin that models air quality, pollutant dispersion, carbon sinks, and the impact of renewable energy transitions across major Indian cities — all powered by live meteorological and AQI data.

[![React](https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=white)](https://react.dev/)
[![Vite](https://img.shields.io/badge/Vite-5-646CFF?logo=vite&logoColor=white)](https://vitejs.dev/)
[![Leaflet](https://img.shields.io/badge/Leaflet-1.9-199900?logo=leaflet&logoColor=white)](https://leafletjs.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## ✨ Features

- 🗺️ **Interactive City Map** — Leaflet-based map with real-time AQI heatmap and wind vector overlays for 8 Indian cities
- 💨 **Gaussian Plume Dispersion** — Physics-based pollutant dispersion using Pasquill-Gifford stability classes and Briggs sigma coefficients
- 🏭 **Emission Sources** — Place coal/gas power plants, heavy/light/cement factories on the map and watch pollution spread in real-time
- 🌳 **Carbon Sinks** — Add tree clusters, forests, and wetlands to absorb CO₂ and PM₂.₅
- ⚡ **Energy Transition Sliders** — Model the impact of rooftop solar, solar farms, wind farms, and coal retirement on city-wide emissions
- 🚗 **Transport Scenarios** — Adjust EV adoption %, public transit share, and cycling rates to reduce NOₓ and PM
- 🌱 **Green Roof Coverage** — Simulate urban green infrastructure and its CO₂ absorption effect
- 📊 **24-Hour Trend Charts** — Recharts-powered timeline for AQI, PM₂.₅, NO₂, SO₂, and CO₂
- 🌬️ **Wind Rose Visualization** — Real-time directional wind speed display
- 🔴 **Live / Demo Mode** — Fetches real AQI from WAQI and weather from OpenWeatherMap; falls back gracefully to realistic mock data
- 🕐 **Time Simulation** — Advances one simulated hour per real minute; manual step control available
- 🏙️ **Multi-City Support** — Pune, Delhi, Mumbai, Bengaluru, Chennai, Kolkata, Hyderabad, Ahmedabad

---

## 🏗️ Architecture

```
src/
├── engine/                     # Core simulation logic
│   ├── SimulationCore.js       # Master orchestrator — tick loop, state management
│   ├── AtmosphericFetcher.js   # Fetches live AQI (WAQI) & weather (OWM) data
│   ├── GaussianPlume.js        # Pasquill-Gifford plume dispersion model
│   ├── EmissionSources.js      # Power plant & factory emission definitions
│   ├── CarbonSinks.js          # Tree / forest / wetland / green-roof absorption
│   ├── TransportModel.js       # Vehicle fleet emission calculations
│   ├── EnergyModel.js          # Grid energy mix & emission factors
│   └── WindModel.js            # Diurnal wind adjustment & wind field generation
│
├── components/                 # React UI components
│   ├── MapView.jsx             # Leaflet map, heatmap layer, wind vectors
│   ├── ControlPanel.jsx        # Left sidebar: source/sink placement & scenario sliders
│   ├── Dashboard.jsx           # Right sidebar: charts, pollutant cards, point inspector
│   ├── TimelineChart.jsx       # 24-hour Recharts line chart
│   └── WindRose.jsx            # SVG wind rose diagram
│
├── utils/
│   ├── constants.js            # City configs, AQI breakpoints, emission factors, sink rates
│   ├── gridUtils.js            # Grid construction, PM2.5→AQI conversion, spatial averaging
│   ├── colorScale.js           # AQI → colour / label / emoji mapping
│   └── cn.js                  # clsx/tailwind-merge utility
│
├── App.jsx                     # Root component, engine lifecycle, event wiring
├── main.jsx                    # React DOM entry point
└── index.css                   # Global design tokens and component styles
```

### Simulation Tick Cycle (per simulated hour)

```
Rebuild baseline grid from AQI data
        │
        ▼
Apply Gaussian plume per emission source  ←── wind speed/dir + stability class
        │
        ▼
Distribute transport emissions across grid cells
        │
        ▼
Spread energy-mix background emissions city-wide
        │
        ▼
Apply carbon sink absorption (trees, forest, wetland, green roofs)
        │
        ▼
Recompute AQI per cell  →  Record 24-h history  →  Notify React
```

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** >= 18
- **npm** >= 9
- API keys (see [API Keys](#-api-keys) below)

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/Developer-rishii/renewable-digital-twin.git
cd renewable-digital-twin

# 2. Install dependencies
npm install

# 3. Configure environment variables
cp .env.example .env
# Edit .env with your API keys (see below)

# 4. Start the development server
npm run dev
```

Open http://localhost:5173 in your browser.

---

## 🔑 API Keys

Copy `.env.example` to `.env` and fill in your keys:

```env
# World Air Quality Index — https://aqicn.org/api/
VITE_WAQI_TOKEN=your_waqi_token_here

# OpenWeatherMap — https://openweathermap.org/api
VITE_OWM_KEY=your_owm_key_here
```

| Key | Source | Free Tier |
|-----|--------|-----------|
| `VITE_WAQI_TOKEN` | [aqicn.org/api](https://aqicn.org/api/) | Free |
| `VITE_OWM_KEY` | [openweathermap.org](https://home.openweathermap.org/users/sign_up) | Free (1000 calls/day) |

> **Note:** If either key is missing or the API is unreachable, the app automatically falls back to realistic mock data and shows a **Demo Data** badge in the header.

---

## 🛠️ Available Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start Vite dev server with hot-reload at http://localhost:5173 |
| `npm run build` | Build optimised production bundle to `dist/` |
| `npm run preview` | Preview the production build locally |

---

## 🌍 Supported Cities

| City | Grid Coverage |
|------|--------------|
| Pune | 18.30-18.75N, 73.65-74.10E |
| Delhi | 28.40-28.85N, 76.90-77.40E |
| Mumbai | 18.85-19.30N, 72.70-73.10E |
| Bengaluru | 12.75-13.20N, 77.35-77.85E |
| Chennai | 12.85-13.30N, 80.10-80.40E |
| Kolkata | 22.35-22.80N, 88.15-88.55E |
| Hyderabad | 17.15-17.65N, 78.25-78.75E |
| Ahmedabad | 22.80-23.25N, 72.35-72.80E |

Each city uses a **50x50 simulation grid** (~1 km resolution).

---

## 🔬 Simulation Models

### Gaussian Plume Dispersion
Implements the Pasquill-Gifford stability class framework with Briggs urban sigma coefficients for σy and σz. Stack downwash and buoyancy rise corrections are applied per source.

### Atmospheric Stability Classes

| Class | Condition | Description |
|-------|-----------|-------------|
| A / B | High insolation + low wind | Very Unstable |
| C | Moderate insolation | Slightly Unstable |
| D | Overcast / neutral | Neutral |
| E | Night + low wind | Stable |
| F | Night + calm | Very Stable |

### Emission Factors
- **Power plants**: kg/MWh (coal, gas, solar, wind)
- **Factories**: kg/hr (heavy, light, cement)
- **Vehicles**: g/km/vehicle (petrol car, diesel car/bus, 2-wheeler, EVs)

### Carbon Sink Rates

| Sink Type | CO2 Absorption |
|-----------|----------------|
| Single Tree | 21 kg/year |
| Forest (per ha) | 6,000 kg/year |
| Wetland (per ha) | 2,000 kg/year |
| Green Roof (per m2) | 0.2 kg/year |

---

## 🧰 Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | React 18 |
| Build Tool | Vite 5 |
| Map | Leaflet 1.9 |
| Charts | Recharts 2 |
| Styling | TailwindCSS v4 + Vanilla CSS |
| Icons | Lucide React |
| AQI Data | WAQI API |
| Weather | OpenWeatherMap API |

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

1. Fork the repository
2. Create your feature branch: `git checkout -b feature/my-feature`
3. Commit your changes: `git commit -m "feat: add my feature"`
4. Push to the branch: `git push origin feature/my-feature`
5. Open a Pull Request

Please follow [Conventional Commits](https://www.conventionalcommits.org/) for commit messages.

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## 👤 Author

**Developer-rishii**
GitHub: [@Developer-rishii](https://github.com/Developer-rishii)

---

> *"The best time to plant a tree was 20 years ago. The second best time is now."* 🌱
