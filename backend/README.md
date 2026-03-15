# VAYU — Pollution Intelligence Platform

Hyper-local AQI monitoring, ML source detection, 6h forecasting, satellite integration, and real-time alerts for Ghaziabad, UP.

---

## Quick Start

### 1. Prerequisites
```bash
Python 3.11+   PostgreSQL 14+   Redis 7+
```

### 2. Install & configure
```bash
cd vayu-backend
python -m venv venv && source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env          # edit DATABASE_URL, REDIS_URL, API keys
```

### 3. Start services (Docker)
```bash
docker compose up db redis -d
```

### 4. Seed database
```bash
python scripts/seed_data.py
```

### 5. Run API
```bash
uvicorn app.main:app --reload --port 8000
```

### 6. Open docs
```
http://localhost:8000/docs       # Swagger UI
http://localhost:8000/redoc      # ReDoc
http://localhost:8000/metrics    # Prometheus metrics
```

---

## Docker (full stack)
```bash
cp .env.example .env
docker compose up --build
python scripts/seed_data.py      # seed from outside, or exec into container
```

---

## Default accounts
| Role     | Email               | Password    |
|----------|---------------------|-------------|
| Admin    | admin@vayu.in       | admin123    |
| Analyst  | analyst@vayu.in     | analyst123  |
| Citizen  | citizen@vayu.in     | citizen123  |

---

## API Reference

### Auth
| Method | Endpoint             | Description        |
|--------|----------------------|--------------------|
| POST   | /api/v1/auth/register | Register user      |
| POST   | /api/v1/auth/login   | Get JWT tokens     |
| POST   | /api/v1/auth/refresh | Refresh token      |
| GET    | /api/v1/auth/me      | Current user       |

### Wards & AQI
| Method | Endpoint                          | Description              |
|--------|-----------------------------------|--------------------------|
| GET    | /api/v1/wards/                    | All wards + current AQI  |
| GET    | /api/v1/wards/{id}                | Single ward              |
| GET    | /api/v1/wards/{id}/aqi/history    | AQI history              |
| GET    | /api/v1/wards/{id}/sources        | ML source detection      |
| GET    | /api/v1/wards/{id}/forecast       | AQI forecast             |
| GET    | /api/v1/wards/{id}/health-advisory| Health advisory          |
| GET    | /api/v1/wards/{id}/risk-index     | Environmental risk score |

### Sensors
| Method | Endpoint                   | Description           |
|--------|----------------------------|-----------------------|
| GET    | /api/v1/sensors/           | List sensors          |
| POST   | /api/v1/sensors/           | Register sensor       |
| POST   | /api/v1/sensors/ingest     | Single reading ingest |
| POST   | /api/v1/sensors/ingest/batch | Batch ingest        |
| GET    | /api/v1/sensors/{id}/health | Sensor health check  |

### Geospatial
| Method | Endpoint                        | Description                      |
|--------|---------------------------------|----------------------------------|
| GET    | /api/v1/geo/locate              | Ward + AQI from lat/lon          |
| GET    | /api/v1/geo/wards-nearby        | Wards within radius              |
| GET    | /api/v1/geo/hotspots            | Top pollution hotspots           |
| GET    | /api/v1/geo/micro-zones/{id}    | Micro-zone grid for ward         |
| GET    | /api/v1/geo/dispersion-map/{id} | Gaussian dispersion map          |
| GET    | /api/v1/geo/city/summary        | City dashboard                   |

### Forecasting
| Method | Endpoint                       | Description          |
|--------|--------------------------------|----------------------|
| GET    | /api/v1/forecast/{ward_id}     | Multi-horizon forecast|
| GET    | /api/v1/forecast/spike-alerts/all | Upcoming spikes   |
| POST   | /api/v1/forecast/run/{ward_id} | Trigger + store forecast|

### Simulation & Policy
| Method | Endpoint                         | Description                |
|--------|----------------------------------|----------------------------|
| POST   | /api/v1/simulation/mitigate      | Simulate AQI reduction     |
| POST   | /api/v1/simulation/policy/{id}   | Generate policy recs       |
| GET    | /api/v1/simulation/scenarios/{id}| Compare all scenarios      |

### Satellite
| Method | Endpoint                        | Description                |
|--------|---------------------------------|----------------------------|
| GET    | /api/v1/satellite/hotspots      | NASA FIRMS fire hotspots   |
| GET    | /api/v1/satellite/fire-signal/{id}| Ward fire signal score   |
| GET    | /api/v1/satellite/events        | Environmental events       |

### Analytics
| Method | Endpoint                               | Description             |
|--------|----------------------------------------|-------------------------|
| GET    | /api/v1/analytics/ward/{id}/monthly-averages | Monthly AQI stats |
| GET    | /api/v1/analytics/ward/{id}/seasonal-patterns | Seasonal analysis|
| GET    | /api/v1/analytics/ward/{id}/peak-events | Worst pollution events|
| GET    | /api/v1/analytics/ward/{id}/diurnal-pattern | Hourly pattern   |
| GET    | /api/v1/analytics/ward/{id}/intelligence-report | Full report |
| GET    | /api/v1/analytics/city/trends          | City-wide trend         |

### Alerts & Reports
| Method | Endpoint                         | Description            |
|--------|----------------------------------|------------------------|
| GET    | /api/v1/alerts/                  | List alerts            |
| POST   | /api/v1/alerts/check-and-create  | Scan & auto-create     |
| POST   | /api/v1/reports/                 | Submit citizen report  |
| GET    | /api/v1/reports/                 | List citizen reports   |

### WebSocket
| Endpoint              | Description                    |
|-----------------------|--------------------------------|
| ws://host/ws/aqi      | All-wards live AQI stream      |
| ws://host/ws/ward/{id}| Single ward live stream        |
| ws://host/ws/alerts   | Real-time alert stream         |

---

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    FastAPI Application                   │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────────┐ │
│  │  Auth    │  │  Wards   │  │ Sensors  │  │  Geo   │ │
│  │  API     │  │  API     │  │  API     │  │  API   │ │
│  └──────────┘  └──────────┘  └──────────┘  └────────┘ │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────────┐ │
│  │Forecast  │  │Satellite │  │Simulation│  │Analytics│ │
│  └──────────┘  └──────────┘  └──────────┘  └────────┘ │
├─────────────────────────────────────────────────────────┤
│                     Services Layer                       │
│  AQI Calculator │ ML Source Detector │ XGBoost Forecaster│
│  Data Pipeline  │ Geospatial Engine  │ Policy Engine      │
│  Satellite Svc  │ Anomaly Detector   │ Risk Calculator    │
├─────────────────────────────────────────────────────────┤
│  PostgreSQL (async)  │  Redis (cache)  │  Celery (tasks)  │
└─────────────────────────────────────────────────────────┘
```

## ML Models

| Model             | Algorithm      | Input Features          | Output               |
|-------------------|----------------|-------------------------|----------------------|
| Source Detector   | Random Forest  | Pollutant ratios, time, weather, satellite | Source probabilities |
| AQI Forecaster    | XGBoost        | 24h lag, rolling stats, weather | Predicted AQI 6–72h |
| Anomaly Detector  | Z-score + IQR  | Rolling sensor window   | Anomaly flag         |
| Risk Calculator   | Weighted score | AQI, forecast, persistence, population | Risk index 0–100 |
