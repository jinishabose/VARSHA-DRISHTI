# VARSHA-Drishti

## Tagline
From Monsoon Signals to Smarter Sowing Decisions

## Project Overview
VARSHA-Drishti is a probabilistic 7–30 day monsoon intelligence system designed to support farmers and agricultural extension officers in making better sowing decisions at Block and Panchayat scale. The system combines large-scale climate signals, regional rainfall indicators, satellite information, terrain context, and agronomic contingency knowledge to predict monsoon onset, break spells, revival phases, and heavy-rainfall risk.

The primary objective is not to predict weather with false certainty, but to quantify risk and convert it into crop-stage-specific, actionable advice. Farmers are the primary users. Extension officers are secondary users who validate and support field deployment.

---

## Problem We Are Solving
Farmers often decide when to sow based on incomplete or delayed climate information. In rainfed agriculture, a wrong sowing decision can lead to:

- delayed sowing when monsoon onset is late
- crop stress during mid-season dry spells or breaks
- loss of seed, labour, and input costs from mistimed planting
- poor resilience when rainfall resumes unexpectedly after a short dry phase

The real problem is not only rainfall prediction; it is the inability to connect climate signals to a farmer's practical action: when to sow, when to wait, and what to do when the monsoon breaks.

---

## Pain Points Addressed
### 1. Lack of local, reliable short-term monsoon intelligence
Most systems provide broad weather trends or general rainfall information, but farmers need decision support that is meaningful at the Block/Panchayat level.

### 2. Uncertainty is not communicated clearly
A forecast without probability is often misleading. Farmers need to understand likelihoods, not just a yes/no statement.

### 3. Farmers need action, not just information
A forecast becomes useful only when it is translated into practical decisions such as:
- delay sowing by N days
- prepare irrigation backup
- switch to a short-duration or drought-tolerant variety
- alter crop choice during a high-risk interval

### 4. Extension officers need better prioritization
Officers need risk maps and localized alerts to decide which villages or blocks need urgent visits and advisories.

### 5. Existing weather systems are not fully integrated for sowing decisions
Current weather and advisory systems often cover one layer of the problem, such as delivery, forecast generation, or district-level contingency planning, but not the full chain from prediction to farmer action.

---

## Why We Are Building This
Indian agriculture is deeply sensitive to monsoon timing. In many rainfed systems, sowing decisions are made under uncertainty and are highly vulnerable to onset delay, dry spells, and revival cycles. Farmers need short-range, probabilistic, locally relevant guidance that is easy to understand and act on.

Our goal is to build a system that:

- understands the monsoon as a probabilistic risk process
- downscales national and regional signals to Block/Panchayat scale
- converts risk into agronomic advice
- delivers that advice through mobile and messaging channels farmers already use

---

## Solution Summary
VARSHA-Drishti is a hybrid forecasting and advisory pipeline that:

1. ingests climate, rainfall, satellite, and terrain data
2. aligns and processes these signals for Block/Panchayat-level analysis
3. applies a hybrid statistical-dynamical + ML forecast model
4. generates onset, break, revival, and heavy-rainfall probabilities
5. converts forecast risk into crop-specific action advice
6. delivers that advice to farmers via mobile web and SMS/WhatsApp, in regional languages

---

## High-Level Architecture

```mermaid
flowchart LR
    A[Data Sources<br/>IMD / IITM / NOAA / BOM / Satellite / Terrain / ICAR] --> B[Data Processing<br/>Cleaning + Alignment + Spatial Downscaling]
    B --> C[AI / ML Forecast Engine<br/>Probabilistic Onset / Break / Revival Risk]
    C --> D[Risk & Probability Layer]
    D --> E[Crop Advisory Engine<br/>Sowing + Irrigation + Variety Guidance]
    E --> F[Delivery Layer<br/>Mobile Web + WhatsApp / SMS + Officer Dashboard]
```

---

## Tech Stack
### Data and Forecasting
- Python
- FastAPI
- PostgreSQL + PostGIS
- Redis
- Celery / scheduled pipelines
- Cloud deployment (AWS/GCP based on pilot needs)
- IMD gridded rainfall products
- IITM Extended Range Prediction System outputs
- NOAA / BOM / JAMSTEC climate index data

### Frontend and Delivery
- React / Next.js
- Progressive Web App (PWA)
- Leaflet / Mapbox style geospatial UI
- WhatsApp Business API / SMS gateway
- Bhashini for regional-language translation and voice support

### AI / ML Layer
- statistical-dynamical forecasting
- gradient-boosted trees / quantile regression style probabilistic calibration
- spatial downscaling using terrain-aware and historical analog-based methods
- rule-based agronomic advisory engine with human review

### DevOps / Deployment
- Docker
- CI/CD pipeline
- object storage for forecast layers and raster datasets

---

## Data Used to Train and Operate the Model
The system is designed to use real, verifiable, operational data sources, not synthetic or imagined data.

### Primary Data Sources
- IMD high-resolution gridded rainfall dataset
- IMD AWS / station observations and block-level weather inputs
- IITM Extended Range Prediction System outputs
- NOAA ENSO indices (Niño 3.4 SST anomalies)
- BOM / JAMSTEC Indian Ocean Dipole information
- NOAA Real-time Multivariate MJO (RMM) index
- Satellite rainfall and gap-filling signals
- SRTM / DEM terrain layers
- ICAR-CRIDA district crop contingency plans

### User and Operational Inputs
- farmer crop type and sowing window
- block or panchayat location
- selected crop stage
- local weather context from mobile or officer review
- officer validation feedback for advisory tuning

### What the model does with this data
- learns historical relationships between large-scale climate signals and local rainfall behavior
- calibrates forecast risk using historical weather patterns and region-specific features
- maps risk to agronomic decision rules based on crop stage and local contingency guidance

---

## How Prediction Works - and Why It Is Not AI Hallucination
This product is not a free-form generative AI system producing random answers. It is an operational forecasting and decision-support system built on observable climate and agronomic data.

### Prediction Logic
- Model inputs are actual meteorological and geospatial datasets
- Forecast outputs are probabilistic, not absolute certainty
- Predictions are generated for 7–30 day windows and framed as likelihoods
- Risk classes are explained in terms of onset, break, revival, or heavy-rainfall probability
- Agronomic advice is generated from explicit rule logic tied to crop stage and contingency guidance

### Why this is reliable by design
- it depends on real historical and operational data
- it surfaces uncertainty instead of hiding it
- it explains the reasoning pathway via forecast and agronomic rules
- it is designed with human-in-the-loop review for officer validation

### Important honesty statement
This system predicts probability, not exact village-level observation. The output is a model-informed estimate for Block/Panchayat scale decision support, not a guarantee of exact rainfall at a field level.

---

## How User Inputs Are Used
The system is designed to use a minimal but relevant set of user inputs.

### Farmer-side inputs
- district / block / panchayat
- crop type
- sowing window / crop stage
- preferred language
- mobile number or WhatsApp access

### Officer-side inputs
- block risk review
- advisory approval or editing
- local field observations
- seasonal feedback to improve future advisories

These inputs are used to contextualize the forecast and generate a useful recommendation, not to fabricate new facts.

---

## Why This Is Better Than Existing Systems
| Area | Existing systems | VARSHA-Drishti |
|---|---|---|
| Forecast horizon | Weather apps focus on short-term, while ERPS covers extended-range forecasting | Combines 7–30 day probabilistic forecasting with local agronomic decision support |
| Spatial resolution | Many systems are district or broad regional level | Downscales to Block/Panchayat granularity |
| Farmer action | Weather alerts often remain informational | Converts risk into crop-stage-specific sowing advice |
| User focus | General weather delivery | Farmer-focused sowing decisions and officer support |
| Language access | Some services are in English or limited regional content | Delivery in regional languages for mobile/web and WhatsApp/SMS |
| Decision support | Mostly information delivery | Action-oriented, probabilistic recommendation engine |

### Key Differentiator
Existing systems solve pieces of the problem, but not the complete decision loop:
- forecast generation
- downscaling to local precision
- crop-specific action recommendation
- multilingual delivery to farmers

VARSHA-Drishti connects these into one workflow.

---

## Impact and Benefits
### For Farmers
- reduced uncertainty in sowing timing
- improved preparation for onset delay and dry spells
- lower risk of crop loss due to mistimed planting
- action suggestions when conditions are unfavourable

### For Extension Officers
- block-level risk prioritization
- faster response to affected villages
- better support for field-level decisions

### For Agriculture Ecosystem
- better resource planning
- smoother crop planning for rainfed regions
- more resilient, timely farm decisions under uncertain monsoon conditions

---

## Future Scope
### Phase 1: Pilot deployment
- selected districts or blocks
- focused crop advisory for major Kharif crops
- farmer and officer feedback loop

### Phase 2: Expansion across states
- state-specific crop contingency rules
- local calibration for regional rainfall behavior
- improved farmer language coverage

### Phase 3: Advanced decision intelligence
- integration with crop insurance and risk records
- field-level validation and historical model improvement
- seasonal learning from officer corrections and observed farmer outcomes

### Phase 4: National-scale readiness
- integration with broader agromet advisory infrastructure
- scalable multilingual alerting and dashboarding
- broader climate adaptation support for vulnerable farming communities

---

## Project Vision
Our mission is to turn monsoon uncertainty into actionable farmer intelligence. We want every farmer to receive not just a weather alert, but a clear answer to a practical question:

“Should I sow now, wait, or change the crop choice?”

---

## Final Statement
VARSHA-Drishti is designed to be useful, explainable, farmer-first, and operationally grounded. It uses real climate and agronomic data to create a probabilistic monsoon advisory system that helps farmers make better sowing decisions in a highly uncertain environment.

It is not a guess engine. It is a risk-aware, data-driven decision-support system for rainfed agriculture.
