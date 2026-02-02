# Data Engineering Zoomcamp 2026 – Week 2  
## Workflow Orchestration with Kestra

This repository contains my solutions and learning artifacts for **Week 2 (Workflow Orchestration)** of the **Data Engineering Zoomcamp** by **DataTalksClub**.

In this module, I built end-to-end ETL pipelines using **Kestra** to orchestrate, schedule, and backfill data workflows using real-world datasets.

---

## 📌 What I Learned

During this week, I focused on understanding **workflow orchestration concepts** and implementing them in practice using Kestra.

Key topics covered:

- Designing workflows using **Kestra Flows**
- Defining **tasks, inputs, outputs, and variables**
- Using **expressions & templating** for dynamic pipelines
- Implementing **conditional logic** with `If` tasks
- Scheduling workflows with **timezone support**
- Running **backfills** for historical data
- Orchestrating multi-month and multi-year ETL jobs
- Building idempotent pipelines using **staging tables & MERGE**

---

## 🏗️ Project Overview

The main project in this module is an **ETL pipeline for NYC Taxi Trip Data**, including:

- **Yellow Taxi**
- **Green Taxi**

### Data Source
NYC TLC Trip Records:  
https://github.com/DataTalksClub/nyc-tlc-data/releases

---

## 🔄 ETL Pipeline Design

Each pipeline follows a production-style pattern:

1. **Extract**
   - Download compressed CSV files
   - Decompress and store them as execution outputs

2. **Transform**
   - Generate deterministic `unique_row_id`
   - Add metadata such as `filename`

3. **Load**
   - Load data into PostgreSQL using staging tables
   - Merge into final tables to avoid duplicates

4. **Cleanup**
   - Remove temporary execution files from Kestra storage

---

## 📁 Repository Structure

```text
.
├── docker-compose.yaml
├── flows/
│   ├── 01_hello_world.yaml
│   ├── 02_python.yaml
│   ├── 03_getting_started_data_pipeline.yaml
│   ├── 04_postgres_taxi.yaml
│   ├── 05_postgres_taxi_scheduled.yaml
│   └── 06_postgres_taxi_year_loop.yaml
└── README.md
````

### Flow Descriptions

* **01_hello_world**
  Introduction to Kestra concepts: inputs, variables, outputs, triggers, concurrency.

* **02_python**
  Running Python scripts in Kestra using Docker-based task runners.

* **03_getting_started_data_pipeline**
  Basic data pipeline example.

* **04_postgres_taxi**
  Core ETL pipeline for loading Yellow & Green Taxi data by month.

* **05_postgres_taxi_scheduled**
  Scheduled orchestration flow with backfill support.

* **06_postgres_taxi_year_loop**
  Controller flow using `ForEach` and `Subflow` to process a full year automatically.

---

## 🕒 Backfill & Scheduling

* Monthly schedules are configured using **Schedule triggers**
* Timezone is explicitly set (e.g. `America/New_York`)
* Historical data is processed using **Kestra Backfill**
* Large ranges (e.g. full year) are handled programmatically via looping flows

---

## 🗄️ Database

* **PostgreSQL** is used for:

  * Kestra metadata (flows, executions, logs)
  * NYC Taxi data warehouse tables
* Data is loaded using:

  * CSV `COPY`
  * Staging tables
  * `MERGE` for idempotent loads

---

## 📊 Validation Queries

Example validation query (Yellow Taxi, 2020):

```sql
SELECT COUNT(*)
FROM public.yellow_tripdata
WHERE filename LIKE 'yellow_tripdata_2020-%';
```

This approach avoids manual per-file checks and mirrors real-world data validation practices.

---

## 🚀 How to Run

1. Start services:

   ```bash
   docker compose up -d
   ```

2. Open Kestra UI:

   ```
   http://localhost:8080
   ```

3. Import flows (via UI or API)

4. Execute flows manually or using schedules/backfill

---

## 📚 Course Reference

This work is part of the **Data Engineering Zoomcamp** by **DataTalksClub**.

Course repository:
[https://github.com/DataTalksClub/data-engineering-zoomcamp](https://github.com/DataTalksClub/data-engineering-zoomcamp)

---

## 🙌 Acknowledgements

* **DataTalksClub** for the open and high-quality course
* **Kestra team** for an excellent workflow orchestration platform
* The Zoomcamp community for shared learning and discussions

---

## 📬 Notes

This repository is for **learning and portfolio purposes**.
Feedback and suggestions are always welcome.

```
