# Modern Data Pipeline with dbt & Airflow

An automated ELT pipeline using dbt, Snowflake, and Airflow to transform order data into analytics-ready fact tables with data quality checks and daily scheduling.

## Project Overview
This pipeline processes order and line item data from the TPC-H dataset through several stages of transformation, implementing data quality checks and automated scheduling.

## Technologies Used
- **dbt**: Data transformation
- **Snowflake**: Data warehouse
- **Apache Airflow**: Orchestration
- **Docker**: Containerization

## Project Structure
```
dbt_snowflake_pipeline/
├── dbt/               # dbt transformation logic
├── airflow/           # Airflow DAGs and config
└── profiles/          # Configuration profiles
```

## Setup Instructions

### Prerequisites
- Snowflake account
- Python 3.8+
- Docker
- dbt
- Apache Airflow

### Snowflake Setup
1. Create warehouse, database, and role:
```sql
use role accountadmin;
create warehouse dbt_wh with warehouse_size='x-small';
create database if not exists dbt_db;
create role if not exists dbt_role;
```

2. Configure permissions:
```sql
grant role dbt_role to user [username];
grant usage on warehouse dbt_wh to role dbt_role;
grant all on database dbt_db to role dbt_role;
```

### dbt Setup
1. Install dbt with Snowflake adapter:
```bash
pip install dbt-snowflake
```

2. Configure profiles.yml with your Snowflake credentials

### Airflow Setup
1. Build Docker image:
```bash
docker build -t airflow-dbt ./airflow
```

2. Configure Snowflake connection in Airflow UI

## Data Models

### Staging Models
- `stg_tpch_orders`: Raw order data
- `stg_tpch_line_items`: Raw line item data

### Intermediate Models
- `int_order_items`: Joined orders and line items
- `int_order_items_summary`: Aggregated order metrics

### Final Models
- `fct_orders`: Final fact table with complete order analysis

## Testing
The project includes both generic and singular tests:
- Primary key validation
- Foreign key relationships
- Custom business logic validation
- Date range validation

## Running the Pipeline

### Local Development
```bash
# Install dependencies
pip install -r requirements.txt

# Run dbt models
dbt run

# Run tests
dbt test
```

### Production Deployment
The Airflow DAG will automatically run the pipeline daily.

## Contributing
1. Fork the repository
2. Create a feature branch
3. Commit changes
4. Push to the branch
5. Create a Pull Request
