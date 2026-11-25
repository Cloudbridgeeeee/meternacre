# MeterNacre — proposed AWS architecture

MeterNacre uses AWS to collect meter readings reliably, model every building’s normal and surface anomalies quickly — scaling from one building to a national portfolio.

## Request flow

1. Smart meters
2. AWS IoT Core
3. AWS Lambda
4. Amazon S3
5. SageMaker AI
6. DynamoDB
7. Facility review

## Services

### AWS IoT Core

**Role:** Collect meter readings

Receives 15-minute readings from smart meters, sub-meters and edge gateways over secure, certificate-based MQTT connections, buffering through grid and generator switchovers.

### Amazon SageMaker AI

**Role:** Model expected consumption

Trains per-building forecasting models using history, weather and occupancy, producing expected consumption and confidence bands for anomaly detection.

### Amazon S3

**Role:** Store reading history

Keeps the full history of readings and utility bills, partitioned by building and meter, as the training and audit record.

### Amazon DynamoDB

**Role:** Serve live readings and alerts

Holds the latest readings, forecasts, anomalies and review notes with millisecond reads for live dashboards.

### AWS Lambda

**Role:** Score readings and alert

Compares every new reading with its forecast, flags anomalies by severity and notifies facility teams.

## Principles

- **Read-only metering** — We read meters; we never switch loads.
- **Building-specific normals** — Each building is compared with itself.
- **Verified savings** — Savings are measured against forecasts, not claims.

## Safeguards

- **Facility teams decide** — Alerts suggest what to check; your team decides what to change.
- **No load control** — MeterNacre never switches equipment or controls building systems.
- **Explained alerts** — Every alert shows expected vs actual usage and when it started.
- **Tenant privacy** — Data is aggregated by meter and building, never by individual person.

## MVP scope

We import a year of utility readings and bills for a set of buildings, learn their normal patterns, and deliver consumption dashboards, anomaly flags and a weekly summary your facility team reviews.

- Utility reading and bill import
- Expected consumption models
- Anomaly flags with review
- Building comparison dashboard
- Weekly operational summary
