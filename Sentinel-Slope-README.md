# Sentinel Slope

**Uncertainty-Aware Landslide Risk Decision Support System for the North-Eastern Region of India**

Sentinel Slope is a software-first landslide risk monitoring and decision-support prototype designed for **Smart India Hackathon 2026 — SIH26001**.

The system combines geospatial data, rainfall information, satellite observations, risk scoring, uncertainty assessment, targeted verification, and resilient communication into one operational workflow.

> **Risk tells us where to look. Uncertainty tells us where to measure. Verified evidence tells us when to act.**

---

## 1. Problem Statement

### SIH26001
**AI-Based early warning and landslide Risk Monitoring System in NER**

**Theme:** Disaster Management  
**Category:** Software  
**Team:** Tech Titans

The North-Eastern Region has difficult mountainous terrain, intense monsoon conditions, scattered settlements, infrastructure corridors, sparse monitoring coverage, and connectivity constraints.

A practical system must therefore answer more than:

> “What is the landslide risk?”

It should also answer:

1. Which locations need attention first?
2. How trustworthy is the available evidence?
3. Should the authority monitor, verify, alert, or act?
4. What additional observation would reduce uncertainty the most?
5. Can critical information remain available when connectivity is lost?

---

## 2. Sentinel Slope Approach

Sentinel Slope follows a **screen → prioritize → verify → act** model.

### Wide-area screening

Use broad-area data sources to identify potentially vulnerable locations:

- Sentinel-1 SAR observations
- Rainfall information
- Terrain and elevation
- Geological and land-cover information
- Soil-moisture proxies
- Historical landslide information
- Road, settlement, and infrastructure context

### Risk prioritization

A machine-learning risk layer combines available features to estimate the relative risk of each monitored slope or location.

### Uncertainty assessment

The system evaluates:

- Data freshness
- Missing data
- Signal disagreement
- Evidence quality
- Model confidence

Instead of forcing a high-confidence answer from weak evidence, the system can explicitly mark:

**DATA CONFLICT / LOW CONFIDENCE**

### Targeted verification

Additional evidence is requested only where it has high value.

Examples:

- Field verification
- Additional observations
- Targeted low-cost sensing
- New satellite observation when available

### Decision support

The system converts the analysis into four operational states:

**MONITOR → VERIFY → ALERT → ACT**

This makes the output useful to an authority instead of presenting only a raw probability.

---

## 3. Core Innovation

### 3.1 Slope State Engine

Determines the current state of a slope using multiple environmental and geospatial signals.

Example state progression:

`STABLE → WETTING → WATCH → UNSTABLE → CRITICAL`

The state is based on changing evidence rather than a single rainfall threshold.

### 3.2 Uncertainty Engine

Answers:

> **“How much should we trust this prediction?”**

It considers:

- Source freshness
- Signal agreement
- Data completeness
- Evidence quality
- Model confidence

A high-risk location with weak evidence can therefore remain in **VERIFY** instead of being treated as a confirmed emergency.

### 3.3 Next-Best-Observation Engine

Answers:

> **“What should we measure next?”**

Instead of deploying monitoring infrastructure everywhere, the system prioritizes locations where additional observations are likely to provide the greatest reduction in uncertainty.

This supports a **software-first, targeted-sensing strategy**.

---

## 4. Resilient Communication

Disaster conditions can also damage communication infrastructure.

Sentinel Slope is therefore designed around multiple communication paths:

```text
                SENTINEL SLOPE
                     |
          +----------+----------+
          |                     |
    HAZARD DETECTION       COMMUNICATION
          |                     |
 Satellite / Terrain        Internet
 Rainfall / Soil               |
 Local Observations            SMS
 AI Analysis                   |
          |                Local P2P
          +----------+----------+
                     |
                RISK DECISION
                     |
                 LOCAL ALERT
                     |
            +--------+--------+
            |                 |
        Authority         Community
            |                 |
            +--------+--------+
                     |
                  Cloud Sync
              when connection returns
```

### Offline-first principle

The field application can retain critical information locally, including:

- Last verified risk state
- Assigned monitoring locations
- Recent alerts
- Field observations
- Queued reports

When connectivity returns, stored information can synchronize with the central system.

### Local peer-to-peer concept

For extreme outages, participating devices can use local store-and-forward communication so an emergency message can move through nearby devices without requiring continuous internet access.

**Important limitation:** local P2P improves communication resilience; it does not independently detect a hazard without an available sensing or reporting source.

---

## 5. Prototype Dashboard

The web prototype acts as a central command interface.

### Dashboard components

- Regional risk map
- Risk summary cards
- Seven-day risk trend
- Monitor / Verify / Alert / Act recommendations
- Data-source status
- Confidence overview
- Recent alerts
- Recent field reports
- System health
- District and risk-level filters

### Map

The prototype uses:

- **Leaflet**
- **OpenStreetMap**

The map displays representative risk markers for demonstration.

### Live weather data

The prototype reads precipitation from the public **Open-Meteo API**.

The current implementation uses this as a live demonstration input and is not a certified operational warning system.

---

## 6. Technology Stack

### Frontend

- HTML5
- CSS3
- JavaScript
- Responsive CSS
- Leaflet.js

### Mapping

- Leaflet
- OpenStreetMap

### Live data

- Open-Meteo precipitation API

### Planned production backend

- Python
- FastAPI
- PostgreSQL
- PostGIS
- GeoAlchemy2

### AI / ML

Planned production pipeline:

- XGBoost
- LightGBM
- SHAP
- Python geospatial and machine-learning tooling

### Planned mobile layer

- React Native
- Expo
- SQLite / WatermelonDB
- Offline-first synchronization

---

## 7. Data Architecture

A production Sentinel Slope deployment can organize information into five layers.

### Layer 1 — Static context

- DEM
- Slope
- Aspect
- Curvature
- Geological information
- Land cover
- Historical landslide inventory
- Roads and settlements

### Layer 2 — Dynamic observations

- Rainfall
- Forecast information
- Soil-moisture observations or proxies
- SAR deformation indicators
- Field observations

### Layer 3 — Data quality

- Timestamp
- Freshness
- Completeness
- Source status
- Confidence

### Layer 4 — Intelligence

- Risk estimation
- Slope-state classification
- Uncertainty estimation
- Next-best-observation recommendation

### Layer 5 — Action

- Dashboard
- Decision cards
- Notifications
- Authority workflow
- Community warning channels
- Audit history

---

## 8. Decision Logic

A simple operational model is:

### MONITOR

**Lower risk or stable evidence**

Continue monitoring.

### VERIFY

**Meaningful risk + insufficient confidence**

Request field evidence or additional observation.

### ALERT

**High risk + sufficiently strong evidence**

Prepare and issue an authorized warning.

### ACT

**Critical risk + high confidence**

Trigger the appropriate emergency response workflow through authorized personnel.

The exact thresholds must be calibrated with real historical and operational data before deployment.

---

## 9. Dashboard Risk Model

The prototype demonstrates four broad risk categories:

| Level | Meaning |
|---|---|
| Very High | Priority threat requiring immediate review |
| High | Significant risk requiring close monitoring or verification |
| Moderate | Elevated condition requiring continued observation |
| Low | Lower current risk |

The dashboard also includes a separate:

**DATA CONFLICT**

state for contradictory or unreliable evidence.

---

## 10. Why the Approach Is Different

Sentinel Slope does not attempt to replace existing national or regional disaster-monitoring systems.

Instead, it acts as a **decision-support layer** that connects different evidence sources.

### Traditional pattern

```text
Data → Prediction → Alert
```

### Sentinel Slope pattern

```text
Data
 ↓
Risk
 ↓
Confidence
 ↓
Evidence conflict?
 ↓
Next-best observation
 ↓
Verification
 ↓
Decision
 ↓
Action
```

The focus is therefore **decision-making under uncertainty**, not just prediction accuracy.

---

## 11. Key Benefits

### Social

- Earlier access to useful risk information
- More actionable warnings
- Multilingual communication potential
- Better support for remote communities

### Economic

- Avoids unnecessary large-scale sensor deployment
- Prioritizes field resources
- Supports protection of roads and infrastructure
- Reduces duplicated monitoring effort

### Environmental

- Uses wide-area remote sensing
- Limits physical deployment to priority areas
- Supports risk-informed planning

### Operational

- Converts complex evidence into clear actions
- Makes uncertainty visible
- Supports field verification
- Provides an auditable decision process

### Scalability

The architecture is designed so that expanding to another district primarily requires:

- New geographic data
- Local calibration
- Additional monitoring inputs

rather than deploying physical infrastructure everywhere.

---

## 12. Challenges and Mitigations

| Challenge | Mitigation |
|---|---|
| SAR observation latency | Combine SAR with rainfall, terrain and other available evidence |
| Missing or stale data | Track source freshness and reduce confidence |
| Conflicting signals | Explicit DATA CONFLICT state |
| Model generalization | Regional validation and calibration |
| Network failure | Offline-first local storage and synchronization |
| Field-report quality | GPS, timestamps, evidence and verification workflows |
| False alarms | Risk + confidence based decision gating |
| Excessive sensor cost | Targeted observation instead of sensor-everywhere deployment |

---

## 13. Project Structure

The current prototype is intentionally lightweight:

```text
sentinel-slope-live/
├── index.html
└── README.md
```

The current implementation is a single-page static web application.

A production architecture can be separated into:

```text
sentinel-slope/
├── frontend/
│   ├── dashboard/
│   └── mobile/
├── backend/
│   ├── api/
│   ├── models/
│   └── services/
├── ml/
│   ├── training/
│   ├── inference/
│   └── explainability/
├── geospatial/
├── database/
└── docs/
```

---

## 14. Running the Current Prototype

No build tool is required.

### Option 1 — Direct

Open:

```text
index.html
```

in a modern browser.

### Option 2 — Local HTTP server

With Python:

```bash
python -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

The web version uses public CDN resources for Leaflet and a public OpenStreetMap tile service, so internet access is required for those live external resources.

---

## 15. Deployment

The current frontend can be deployed as a static website.

Compatible platforms include:

- GitHub Pages
- Cloudflare Pages
- Netlify
- Vercel
- Any conventional static web host

### Vercel

The project can be deployed directly as a static site.

Typical flow:

```bash
git clone <repository-url>
cd sentinel-slope-live
```

Push the repository to GitHub and import it into Vercel.

No frontend build command is required for the current single-file version.

---

## 16. Production Roadmap

### Phase 1 — Prototype

- Interactive dashboard
- Risk map
- Demonstration risk states
- Live precipitation input
- Alert and field-report interface

### Phase 2 — Data integration

- Real government-approved data feeds
- Sentinel-1 processing pipeline
- Historical landslide inventory
- Regional terrain and geological layers
- PostGIS spatial storage

### Phase 3 — AI validation

- Train and evaluate regional models
- Calibrate predicted probabilities
- Add uncertainty estimates
- Validate against historical events

### Phase 4 — Offline field application

- React Native application
- Offline maps
- Local data storage
- Store-and-forward reports
- Local alert caching
- Synchronization

### Phase 5 — Operational pilot

- Selected high-risk corridors
- Authority review workflow
- Field verification
- Alert governance
- Continuous monitoring and model recalibration

---

## 17. Important Safety Note

Sentinel Slope is currently a **prototype and decision-support demonstration**.

The risk values, locations, alert counts, confidence values and other dashboard numbers shown in the prototype are demonstration data unless explicitly connected to a validated production data source.

The system must not be used for real evacuation, road-closure, emergency-response, or life-safety decisions without:

- Validated data feeds
- Regional model calibration
- Independent technical validation
- Operational testing
- Appropriate government authority approval
- Defined alert governance and human oversight

---

## 18. Research Direction

The project is based on research areas including:

- Landslide susceptibility mapping
- Rainfall-triggered landslide analysis
- Sentinel-1 SAR deformation monitoring
- Soil-moisture influence on slope stability
- Machine-learning risk prediction
- Explainable AI
- Spatial-temporal analysis
- Uncertainty-aware decision systems
- Offline-first disaster applications
- Resilient communication networks

The project should maintain a clearly documented distinction between:

**published evidence → prototype assumptions → measured prototype results → future targets**

This prevents projected performance from being presented as field-validated performance.

---

## 19. References and Data Sources

The current project research references include:

### Sentinel-1 / Copernicus

Used for radar-based earth observation and deformation-related analysis.

https://dataspace.copernicus.eu/

### India Meteorological Department

Used for rainfall and weather information.

https://mausam.imd.gov.in/

### Geological Survey of India

Used for geological and landslide-related information.

https://www.gsi.gov.in/

### ISRO / NRSC

Used for Indian remote-sensing and geospatial resources.

https://www.nrsc.gov.in/

### SMAP / NASA

Used as a source for soil-moisture information.

https://smap.jpl.nasa.gov/

### OpenStreetMap

Used for roads, settlements and infrastructure context.

https://www.openstreetmap.org/

### Open-Meteo

Used by the current prototype for live precipitation demonstration.

https://open-meteo.com/

### Leaflet

Used for the interactive map interface.

https://leafletjs.com/

---

## 20. SIH Positioning

Sentinel Slope should be presented as:

> **A software-first, uncertainty-aware landslide decision-support network that uses wide-area intelligence to prioritize where additional observation is needed and converts uncertain environmental signals into actionable decisions.**

The strongest engineering message is:

> **Monitor broadly. Measure intelligently. Verify uncertainty. Act decisively.**

---

## 21. Team

**Tech Titans**

**Project:** Sentinel Slope  
**Problem Statement:** SIH26001  
**Theme:** Disaster Management  
**Category:** Software
