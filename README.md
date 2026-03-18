# Real-Time Customer Intelligence Platform

> End-to-end ML system for customer churn prediction and retention analytics

![Python](https://img.shields.io/badge/Python-3.10+-blue) ![FastAPI](https://img.shields.io/badge/FastAPI-0.110-green) ![XGBoost](https://img.shields.io/badge/XGBoost-2.0-orange) ![Docker](https://img.shields.io/badge/Docker-Compose-blue) ![Airflow](https://img.shields.io/badge/Airflow-2.8-red) ![AWS](https://img.shields.io/badge/AWS-EC2%20%2B%20S3-yellow)

---

## Overview

A production-grade customer intelligence platform that ingests 500K+ retail transactions, engineers RFM features into a PostgreSQL feature store, trains an XGBoost churn model, and serves real-time predictions via a FastAPI REST API — all orchestrated by Apache Airflow and deployed on AWS EC2 with Docker.

**Live demo:** `http://<ec2-public-ip>:8501` _(available after Week 6)_

---

## Architecture

```
UCI Online Retail II (500K+ rows)
         │
         ▼
 Airflow DAG (ETL)
  /pipeline/etl_dag.py
         │
         ▼
 PostgreSQL Feature Store
  user_features table
  (recency, frequency, monetary, churn_score)
         │
         ▼
 XGBoost Model Training
  /models/train.py  →  xgboost_churn.joblib (S3)
         │
         ▼
 FastAPI REST API          Streamlit Dashboard
  POST /predict        ←→   /dashboard/app.py
  GET  /metrics             Plotly charts
  GET  /customer/{id}/history
  POST /ingest (simulated)
  GET  /health
         │
         ▼
 Docker + AWS EC2
 GitHub Actions CI/CD
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| Orchestration | Apache Airflow 2.8 |
| Data Store | PostgreSQL 15 |
| ML Modeling | XGBoost, Scikit-learn |
| API | FastAPI + Pydantic |
| Dashboard | Streamlit + Plotly |
| Containerization | Docker + docker-compose |
| Cloud | AWS EC2 + S3 |
| CI/CD | GitHub Actions |

---

## Project Structure

```
customer-intelligence-platform/
├── data/
│   ├── raw/                  # Raw UCI Online Retail II dataset
│   └── processed/            # Cleaned + transformed data
├── pipeline/
│   ├── etl_dag.py            # Airflow DAG — raw → clean → PostgreSQL
│   └── features.py           # RFM feature engineering
├── models/
│   ├── train.py              # Model training + evaluation
│   └── evaluate.py           # ROC-AUC, precision, recall reporting
├── api/
│   ├── main.py               # FastAPI app entry point
│   ├── routes/               # Endpoint definitions
│   └── schemas.py            # Pydantic request/response models
├── dashboard/
│   └── app.py                # Streamlit dashboard
├── docker/
│   └── Dockerfile            # API container
├── config/
│   └── settings.py           # Config loader (reads from .env)
├── tests/
│   └── test_api.py           # pytest — API endpoint tests
├── docker-compose.yml
├── .env.example
└── README.md
```

---

## Quickstart (Local)

**1. Clone and set up environment**
```bash
git clone https://github.com/<your-username>/customer-intelligence-platform.git
cd customer-intelligence-platform
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

**2. Configure environment**
```bash
cp .env.example .env
# Edit .env with your local PostgreSQL credentials
```

**3. Start all services**
```bash
docker-compose up --build
```

**4. Access**
- API docs: `http://localhost:8000/docs`
- Dashboard: `http://localhost:8501`
- Airflow UI: `http://localhost:8080`

---

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| POST | `/predict` | Returns churn probability for a customer |
| GET | `/metrics` | Model performance — ROC-AUC, precision, recall |
| GET | `/customer/{id}/history` | Prediction history for a customer |
| POST | `/ingest` | Simulated event ingestion (not Kafka-grade) |
| GET | `/health` | Health check for Docker + AWS |

**Example prediction request:**
```bash
curl -X POST http://localhost:8000/predict \
  -H "Content-Type: application/json" \
  -d '{"customer_id": "U001", "recency": 30, "frequency": 12, "monetary": 1420.0}'
```

**Example response:**
```json
{
  "customer_id": "U001",
  "churn_probability": 0.73,
  "risk_level": "high",
  "model_version": "xgboost_v1"
}
```

---

## Dataset

**UCI Online Retail II** — 500K+ retail transactions from a UK-based online retailer (2009–2011).

- Source: [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/502/online+retail+ii)
- Records: ~1 million rows across 2 sheets
- Features used: CustomerID, InvoiceDate, Quantity, Price → RFM features

> Dataset not included in repo. Download and place at `data/raw/online_retail_ii.xlsx`

---

xxxx Team xxxx

---

## Build Progress

- [x] Week 1 — Repo setup + dataset
- [ ] Week 2 — Airflow ETL pipeline
- [ ] Week 3 — Feature engineering + feature store
- [ ] Week 4 — ML modeling
- [ ] Week 5 — FastAPI + Docker
- [ ] Week 6 — AWS deployment + dashboard
- [ ] Week 7 — Integration + testing
- [ ] Week 8 — Polish + documentation

---

## License

MIT
