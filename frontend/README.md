# Pollution Intelligence Platform Frontend

Frontend for the Pollution Intelligence Platform, a ward-level air quality monitoring and mitigation system designed for municipal administrators, environmental agencies, citizens, and researchers.

## Product Overview

The platform is intended to support real-time and predictive air quality intelligence at a hyper-local level. It combines environmental sensor data, weather inputs, and machine learning outputs to help users monitor pollution, understand likely source contributors, receive health guidance, and take operational action.

Core goals:

- Detect localized pollution hotspots.
- Identify probable pollution sources.
- Provide automated policy recommendations for administrators.
- Deliver personalized health advisories to citizens.
- Enable data-driven environmental governance.

## Target Users

| User | Need |
| --- | --- |
| Municipal administrators | Identify pollution sources and enforce actions |
| Environmental agencies | Data analytics and trend monitoring |
| Citizens | Local air quality updates and health guidance |
| Researchers | Access to granular environmental datasets |

## Objectives

- Provide hyper-local AQI monitoring at ward or block level.
- Detect pollution sources using machine learning models.
- Predict AQI trends and pollution spikes.
- Offer automated mitigation and policy recommendations.
- Deliver personalized health advisories.
- Support faster, data-driven decision making.

## Planned Frontend Modules

### 1. Home Dashboard

- City AQI summary
- AQI status card with color-coded severity
- Pollution statistics overview
- Daily AQI trend charts

### 2. Hyper-Local AQI Monitoring

- Interactive heatmap at ward or block level
- Ward-specific pollution metrics
- Time-series visualizations
- Pollution index and hotspot summaries

Displayed metrics:

- PM2.5
- PM10
- NO2
- SO2
- CO
- O3
- Temperature
- Humidity

### 3. Pollution Source Detection

Expected source categories:

- Construction dust
- Biomass burning
- Traffic congestion
- Industrial emissions
- Waste burning

Expected model outputs:

- Predicted source label
- Confidence score
- Ward-level source attribution summaries
- Visual breakdowns using charts

### 4. Predictive AQI Forecasting

Forecast horizons:

- Next 6 hours
- Next 24 hours
- Next 3 days

Frontend deliverables:

- Forecast charts
- Early warning banners
- Explanations for forecast trends

### 5. Automated Policy Recommendation Engine

The admin-facing interface should translate detected issues into recommended actions.

Examples:

| Detected Issue | Recommended Action |
| --- | --- |
| Construction dust | Enforce dust barriers and water spraying |
| Biomass burning | Deploy enforcement teams |
| Traffic congestion | Apply temporary vehicle restrictions |
| Industrial emissions | Issue compliance alerts |
| Waste burning | Send municipal inspection team |

### 6. Citizen Health Advisory System

| AQI Level | Health Advisory |
| --- | --- |
| 0-50 | Safe outdoor activities |
| 51-100 | Sensitive groups should limit exposure |
| 101-200 | Reduce outdoor exercise |
| 201-300 | Wear masks outdoors |
| 301+ | Stay indoors |

Interface goals:

- Location-aware AQI visibility
- Health recommendations by severity
- Alert and notification surfaces
- Support for future SMS and push notification integration

### 7. Admin Dashboard

Expected sections:

- Pollution map
- Source detection analytics
- Forecast analytics
- Mitigation recommendation panel
- Citizen alert management
- Trend monitoring and incident review

## Machine Learning Context

The frontend will consume outputs from backend and ML services. Likely models and analytics pipelines include:

- Random Forest Classifier
- Gradient Boosting
- CNN for satellite imagery interpretation
- LSTM for time-series prediction
- ARIMA for forecasting baselines
- DBSCAN, K-Means, and spatial clustering for hotspot detection

Example source detection output:

| Ward | AQI | Predicted Source | Confidence |
| --- | --- | --- | --- |
| Ward 21 | 182 | Construction Dust | 0.86 |
| Ward 15 | 210 | Biomass Burning | 0.79 |

## System Architecture

Conceptual flow:

1. Sensors, satellites, and external APIs feed raw environmental data.
2. A data ingestion layer receives and normalizes incoming streams.
3. Data processing handles cleaning and feature engineering.
4. ML services perform source detection, AQI forecasting, and hotspot detection.
5. A recommendation engine generates suggested actions.
6. The dashboard and citizen-facing application present insights and alerts.

## Technology Stack

| Layer | Technology |
| --- | --- |
| Frontend | React or Next.js |
| Backend | Python with FastAPI or Flask |
| ML Models | Scikit-learn, TensorFlow |
| Database | PostgreSQL or MongoDB |
| Real-time Data | Kafka or WebSockets |
| Cloud | AWS or GCP |
| Visualization | Mapbox or Leaflet |

## Data Requirements

Training and inference features expected from backend services:

| Feature | Description |
| --- | --- |
| PM2.5 | Particulate matter concentration |
| PM10 | Coarse particle concentration |
| NO2 | Traffic emission indicator |
| SO2 | Industrial emission indicator |
| CO | Combustion indicator |
| Temperature | Weather impact |
| Humidity | Pollution dispersion factor |
| Wind speed | Pollution movement |
| Traffic density | Vehicles per hour |
| Construction activity | Binary or scored activity indicator |
| Biomass burning index | Satellite-detected anomaly score |

## KPIs

| KPI | Target |
| --- | --- |
| Source detection accuracy | More than 85% |
| Prediction accuracy | More than 80% |
| Dashboard latency | Less than 3 seconds |
| Coverage | Ward-level monitoring |
| Alert delivery time | Less than 1 minute |

## Security and Privacy

- Secure API authentication
- Encrypted data transmission
- Role-based access control
- Compliance with environmental data standards

## Roadmap

### Phase 1 - MVP

- AQI dashboard
- Ward-level heatmap
- Basic pollution forecasting

### Phase 2

- Source detection ML integration
- Admin policy recommendation workflows

### Phase 3

- Citizen mobile alerts
- Predictive mitigation system

## Expected Impact

- Faster pollution source identification
- Better municipal decision making
- Reduced public health risk exposure
- Higher environmental transparency

## Example User Flows

### Administrator

1. Login.
2. View AQI heatmap.
3. Detect a pollution hotspot.
4. Review system-identified likely source.
5. Receive mitigation recommendation.
6. Deploy enforcement action.

### Citizen

1. Open the app.
2. View local AQI.
3. Receive health advisory.
4. Get alerts for pollution spikes.

## Possible Extensions

- Drone-based pollution sensing
- Computer vision for construction dust detection
- Smart traffic control integration
- Pollution simulation modeling

## Current Frontend Status

The current frontend in this folder is still at a starter stage and includes a basic Vite React app. The full dashboard, charts, map views, admin workflows, and reporting flows described above still need to be implemented.

Current entry points:

- [src/App.jsx](src/App.jsx)
- [src/main.jsx](src/main.jsx)
- [src/index.css](src/index.css)

## Recommended Frontend Dependencies

To implement the planned dashboard, the frontend will need additional packages beyond the current starter setup.

Suggested packages:

- Tailwind CSS
- react-router-dom
- chart.js
- react-chartjs-2
- mapbox-gl
- lucide-react or a similar icon library

Example install commands:

```bash
npm install react-router-dom chart.js react-chartjs-2 mapbox-gl lucide-react
npm install -D tailwindcss postcss autoprefixer
```

## Local Development

Install dependencies:

```bash
npm install
```

Start the development server:

```bash
npm run dev
```

Create a production build:

```bash
npm run build
```

Run preview mode:

```bash
npm run preview
```

## Suggested Frontend Structure

```text
src/
  components/
    AQICard.jsx
    PollutionChart.jsx
    PredictionChart.jsx
    MapView.jsx
    Navbar.jsx
    Sidebar.jsx
    PollutionReportForm.jsx
    AlertNotificationCard.jsx
  pages/
    Dashboard.jsx
    AQIMap.jsx
    SourceDetection.jsx
    Prediction.jsx
    HealthAdvisory.jsx
    CitizenReport.jsx
    AdminDashboard.jsx
  data/
    mockData.js
  utils/
    aqi.js
```

## Backend Integration Notes

The frontend should be designed so mock data can be replaced later with API responses from backend services for:

- Real-time ward AQI feeds
- Forecast outputs
- Pollution source classification results
- Hotspot clustering summaries
- Citizen incident reports
- Admin recommendations and alerts

Recommended API contract areas:

- `/api/aqi/current`
- `/api/aqi/forecast`
- `/api/sources/detection`
- `/api/hotspots`
- `/api/recommendations`
- `/api/reports`

## Notes

- Visualization can use Mapbox or Leaflet depending on licensing and deployment needs.
- Real-time streaming can be integrated later using WebSockets.
- Backend APIs should enforce authentication and role-based access for administrator features.
