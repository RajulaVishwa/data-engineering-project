# Data Engineering Project

A practical starter project for a modern data engineering workflow using Python, PostgreSQL, Docker, and orchestration patterns.

## Overview

This project demonstrates:

- Extracting sample data from CSV files
- Transforming it using Python and pandas
- Loading it into PostgreSQL
- Running a basic ETL pipeline
- Preparing the repo for extension with dbt, Airflow, or Spark

## Architecture

- Source data: `data/raw/`
- ETL logic: `src/etl.py`
- Warehouse: PostgreSQL via Docker
- Transformations: SQL scripts in `sql/`
- Data modeling: `dbt/`

## Project Structure

```
.
├── .env.example
├── .gitignore
├── README.md
├── docker-compose.yml
├── requirements.txt
├── Makefile
├── data/
│   └── raw/
│       └── sample_orders.csv
├── dbt/
│   ├── dbt_project.yml
│   └── models/
│       └── marts/
│           └── dim_orders.sql
├── scripts/
│   └── run_etl.py
├── sql/
│   └── init.sql
├── src/
│   ├── __init__.py
│   ├── config.py
│   ├── database.py
│   └── etl.py
├── tests/
│   └── test_etl.py
└── .github/workflows/ci.yml
```

## Quick Start

### 1. Clone and setup

```bash
git clone https://github.com/RajulaVishwa/data-engineering-project.git
cd data-engineering-project
cp .env.example .env
```

### 2. Start PostgreSQL

```bash
docker compose up -d
```

### 3. Install Python dependencies

```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### 4. Run the ETL pipeline

```bash
python scripts/run_etl.py
```

### 5. Verify data in the database

```bash
psql postgresql://postgres:postgres@localhost:5432/datawarehouse
```

Once connected:
```sql
SELECT * FROM raw.orders;
SELECT * FROM analytics.daily_orders_summary;
```

## Make Commands

```bash
make up      # Start PostgreSQL
make etl     # Run the ETL pipeline
make test    # Run tests
make clean   # Stop and remove containers
```

## Database Schema

**Raw Layer (raw schema)**
- `orders`: Raw ingested order data

**Analytics Layer (analytics schema)**
- `daily_orders_summary`: Aggregated daily order metrics

## Pipeline Flow

```
CSV File → Extract → Transform → Load → PostgreSQL
                                          ├── raw.orders
                                          └── analytics.daily_orders_summary
```

## What Each Component Does

### src/config.py
Loads environment variables and provides configuration settings for database connections.

### src/database.py
Manages database connections using SQLAlchemy and psycopg2.

### src/etl.py
Contains the core ETL logic:
- `extract_csv()`: Reads CSV files
- `transform_orders()`: Cleans and transforms data
- `load_orders()`: Writes data to PostgreSQL

### scripts/run_etl.py
Entry point for the ETL pipeline.

### sql/init.sql
Database initialization script that creates schemas and tables on container startup.

## Future Extensions

- [ ] Add Apache Airflow for workflow orchestration
- [ ] Add dbt for advanced data modeling
- [ ] Add Great Expectations for data quality checks
- [ ] Add S3/GCS integration for cloud storage
- [ ] Add Spark for distributed processing
- [ ] Add GitHub Actions CI/CD pipeline
- [ ] Add monitoring and alerting

## Development

To add new data sources:
1. Place raw data in `data/raw/`
2. Extend `src/etl.py` with new extraction/transformation logic
3. Update `sql/init.sql` with new table definitions
4. Run tests: `pytest -q`

## Dependencies

- **pandas**: Data manipulation and CSV reading
- **psycopg2-binary**: PostgreSQL adapter
- **SQLAlchemy**: ORM for database operations
- **pytest**: Testing framework

## License

MIT
