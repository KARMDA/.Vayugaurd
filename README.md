# VayuGuard

> A hyperlocal air-quality forecasting & health-advisory platform — built from scratch to production by four coordinated engineering tracks.

**Altrodav Industry Immersion Program · 45-Day Capstone · v1.0**

---

## Table of Contents

- [Overview](#overview)
- [The Problem](#the-problem)
- [What VayuGuard Delivers](#what-vayuguard-delivers)
- [System Architecture](#system-architecture)
- [Engineering Tracks](#engineering-tracks)
- [Sprint Roadmap](#sprint-roadmap)
- [Operating Rhythm](#operating-rhythm)
- [Definition of Done](#definition-of-done)
- [Early Decisions to Lock](#early-decisions-to-lock)

---

## Overview

VayuGuard is a full-stack, production-grade platform that ingests real-time air-quality and weather data, forecasts AQI 24–72 hours ahead at a hyperlocal level, and delivers personalised health advisories based on each user's profile.

The project is built end-to-end over 45 working days (nine one-week sprints) by four coordinated engineering roles: AI/ML, Data, DevOps, and MERN. Every role is essential — none is decorative.

| | |
|---|---|
| **Duration** | 45 days · 9 one-week sprints |
| **Tracks** | AI/ML · Data · DevOps · MERN |
| **Outcome** | Live, monitored production URL |

---

## The Problem

Air-quality data in India and across much of the world is:

- **Station-level** — the nearest monitor may be 8 km away
- **Backward-looking** — it tells you what happened, not what will happen
- **Impersonal** — it ignores whether you have asthma, a toddler, or work outdoors

VayuGuard closes that gap with forward-looking, hyperlocal, personalised information.

---

## What VayuGuard Delivers

| # | Capability | Description |
|---|---|---|
| 01 | **Ingest** | Real-time and historical air-quality data (OpenAQ / CPCB) plus weather (Open-Meteo) across selected cities and zones |
| 02 | **Forecast** | Predicts AQI 24–72 hours ahead at a hyperlocal level using a progression of ML models |
| 03 | **Advise** | Generates personalised health advisories from each user's profile (asthma, children, elderly, outdoor workers) |
| 04 | **Visualise** | Interactive map, forecast charts, historical trends, analytics dashboards, and threshold-based alerts |
| 05 | **Analyse** | Admin and analytics layer surfacing pollution patterns, hotspots, and weather–pollution correlations |

---

## System Architecture

```
External Data Sources
  OpenAQ · CPCB · Open-Meteo
          │
          ▼
Ingestion & Cleaning Pipeline  ──────────────────────────────────────────┐
  (Data Analyst)                                                           │
          │                                                                │
          ├──────────────────────────────────────────────────────────┐    │
          ▼                                                           ▼    │
  Analytical Store + Dashboards          Feature Engineering + Models      │
  Postgres · KPIs · Insights              AI/ML · forecast + risk          │
                                                   │                       │
                                                   ▼                       │
                                          ML Service — FastAPI              │
                                          /forecast · /health-risk          │
                                                   │                       │
                                                   ▼                       │
                                         Application Backend ◄─────────────┘
                                         Node.js · Express · MongoDB
                                                   │
                                                   ▼
                                          Web Application — React
                                     Map · Forecast · Alerts · Admin
                                                   │
                                                   ▼
                                              End Users

──────────────────────────────────────────────────────────────────────────
  DEVOPS PLATFORM — operates every service above
  Docker · CI/CD · Cloud Hosting · Scheduled Jobs · Monitoring · Security
──────────────────────────────────────────────────────────────────────────
```

The two shared contracts that must be locked in Week 1:

1. **Data schema** — agreed between the Data Analyst, ML Developer, and MERN Developer
2. **ML API spec** (`/forecast`, `/health-risk`) — agreed between the ML Developer, MERN Developer, and DevOps Engineer

Lock these early and the four tracks rarely block one another.

---

## Engineering Tracks

### AI/ML Developer — *Owns Prediction*

Responsible for the full ML lifecycle: feature engineering, model progression (baseline → classical → deep), evaluation, serving, and retraining.

**Key deliverables**
- Feature pipeline (lag features, rolling means, weather joins, time features)
- Model progression: persistence/moving average → Prophet/ARIMA → XGBoost → LSTM/GRU
- Health-risk scoring model
- Rigorous evaluation: MAE / RMSE, backtesting
- Versioned FastAPI serving layer (`/forecast`, `/health-risk`)
- Automated retraining pipeline with drift monitoring

**Stack:** Python · scikit-learn · XGBoost · PyTorch/TF · FastAPI

---

### Data Analyst — *Owns Data & Insight*

Responsible for the full data lifecycle: ingestion, cleaning, analysis, and the dashboards that power the platform's insights surfaces.

**Key deliverables**
- End-to-end ingestion and cleaning pipeline (OpenAQ, CPCB, Open-Meteo)
- Data-quality checks and automated validation gates
- Exploratory analysis and KPI definitions (unhealthy-air days, exposure scores)
- Health-impact, hotspot, and correlation analysis
- Analytical store schema and BI dashboards
- Reporting exports (CSV / PDF summaries)

**Stack:** SQL · Postgres · Pandas · dbt / BI · Dashboards

---

### DevOps Engineer — *Owns Reliability*

Responsible for everything that keeps the platform running in production: infrastructure, CI/CD, scheduling, monitoring, and security.

**Key deliverables**
- Repo, branching strategy, and environment definitions (dev / staging / prod)
- Dockerised services and docker-compose setup
- CI/CD pipelines (lint, test, deploy on every PR)
- Cloud infrastructure provisioning (Render / Railway / Fly or AWS)
- Scheduled ingestion jobs and retraining pipeline
- Secrets management (no keys committed to code)
- Monitoring, logging, and alerting (Prometheus + Grafana or hosted equivalent)
- Load testing, security hardening, backups, and rollback runbook

**Stack:** Docker · GitHub Actions · Cloud · Nginx · Prometheus / Grafana

---

### MERN Stack Developer — *Owns Experience*

Responsible for the full web application: authentication, user profiles, all user-facing views, the alert system, the admin panel, and the integration glue.

**Key deliverables**
- Authentication (sign-up / login, JWT)
- User health-profile management
- Interactive map with live AQI markers
- Forecast charts and historical-trends views
- Personalised health-advisory UI
- Alerts / subscription system with email/push notifications
- Analytics and insights UI (consuming the analytics APIs)
- Admin panel (user and alert management)
- Responsive, mobile-polished, production-grade frontend

**Stack:** React · Node.js · Express · MongoDB

---

## Sprint Roadmap

### Phase 1 — Foundation & Discovery (Weeks 1–2)

| Week | Theme | Key Milestones |
|---|---|---|
| **Week 1** · Days 1–5 | Foundation, Framing & Setup | Signed-off data schema · ML API contract · Architecture diagram · Working repo with CI placeholder · App shell on staging |
| **Week 2** · Days 6–10 | Data Pipeline, Baseline Model & App Skeleton | Baseline model metrics · Ingestion pipeline live · Three services on staging · Auth, map, and forecast pages running |

### Phase 2 — Core Build (Weeks 3–5)

| Week | Theme | Key Milestones |
|---|---|---|
| **Week 3** · Days 11–14 | Real Integration Begins | Live `/forecast` returning real predictions · KPI and hotspot dashboards · Scheduled ingestion · App pulling real forecasts end to end |
| **Week 4** · Days 16–20 | Advanced Models & Richer Product | Champion model + `/health-risk` endpoint · Advisory rules and admin views · Automated deploys and backups · Personalised advisories and alerts |
| **Week 5** · Days 21–25 | Feature Completion | Retraining and drift report · Accuracy and cohort reports · API gateway live · Feature-complete app on staging |

### Phase 3 — Integration (Week 6)

| Week | Theme | Key Milestones |
|---|---|---|
| **Week 6** · Days 26–30 | Full Integration Sprint | All services behind one staging URL · No mocks remaining · Load tested · Full integrated demo across all tracks |

### Phase 4 — Testing & Hardening (Weeks 7–8)

| Week | Theme | Key Milestones |
|---|---|---|
| **Week 7** · Days 31–35 | Testing & Reliability | E2E tests green · Stable model with monitoring · Chaos/resilience tests passing · Verified data accuracy |
| **Week 8** · Days 36–40 | Hardening, Security & UAT | Release candidates tagged for all tracks · Security review complete · Production dry-run passed |

### Phase 5 — Launch & Handover (Week 9)

| Week | Theme | Key Milestones |
|---|---|---|
| **Week 9** · Days 41–45 | Production Launch & Handover | All services deployed to production · Live monitoring and alerting active · Documentation complete · Final demo presented |

---

## Operating Rhythm

Every candidate submits, every day. This rhythm is what turns 45 days of effort into a production system rather than a pile of notebooks.

**Daily**
1. Push work to your branch with a clear commit message; open a PR when a feature is complete.
2. Submit a three-line end-of-day log in the shared tracker — **Done / Blocked / Next**.
3. Attach any artefact — notebook, screenshot, dashboard link, or deployed URL.

**Weekly (every Friday)**
- Each track shows a working increment.
- Followed by a 30-minute cross-track integration check.

> **The one rule that keeps it production-ready:** nothing is "done" until it is committed, reviewed, and running in at least the staging environment. Local-only work does not count.

---

## Definition of Done

The project is considered production-ready only when **every** item below is true:

- [ ] A live, public production URL serving real users
- [ ] Every service containerised and deployed through CI/CD
- [ ] Real, scheduled data ingestion and automated model retraining
- [ ] Monitoring, logging, and alerting active across all services
- [ ] Secrets managed centrally — no keys committed to code
- [ ] Authentication and baseline security hardening complete
- [ ] Automated tests passing in the pipeline
- [ ] Rollback procedure and on-call runbook documented
- [ ] A complete README / handover document per role

---

## Early Decisions to Lock

Two choices must be made deliberately in Week 1 — revisiting them mid-project is expensive.

### Target Cities & Zones
Start with **two or three cities**, not twenty. Narrow scope keeps the data pipeline, models, and dashboards tractable within 45 days. Breadth can come after launch.

### Cloud Host
| Option | Trade-off |
|---|---|
| **Render / Railway / Fly** | Simpler setup, faster to deploy, lower schedule risk |
| **AWS** | Genuine DevOps depth (IAM, ECS, RDS, etc.) but adds schedule risk — choose deliberately |

---

*Altrodav · Industry Immersion Program — Capstone Project Brief*
