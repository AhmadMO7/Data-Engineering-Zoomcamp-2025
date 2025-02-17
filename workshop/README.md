# Workshop Homework

This document contains the answers for the Workshop Homework of the Data Engineering Zoomcamp.

---

## **Question 1**
```
import dlt
print("dlt version:", dlt.__version__)
```

### **Answer:**
**dlt 1.6.1**

---

## **Question 2**
```
import dlt
from dlt.sources.helpers.rest_client import RESTClient
from dlt.sources.helpers.rest_client.paginators import PageNumberPaginator


@dlt.resource(name="rides")
def ny_taxi():
    """Fetch paginated data from the NYC Taxi API."""
    client = RESTClient(
        base_url="https://us-central1-dlthub-analytics.cloudfunctions.net/data_engineering_zoomcamp_api",
        paginator=PageNumberPaginator(
            base_page=1,  # Start from page 1
            total_path=None  # API doesn't return total pages explicitly
        )
    )

    for page in client.paginate("data_engineering_zoomcamp_api"):
        yield page  # Yield each page's data



pipeline = dlt.pipeline(
    pipeline_name="ny_taxi_pipeline",
    destination="duckdb",
    dataset_name="ny_taxi_data"
)

load_info = pipeline.run(ny_taxi)
print(load_info)

import duckdb
from google.colab import data_table
data_table.enable_dataframe_formatter()

# A database '<pipeline_name>.duckdb' was created in working directory so just connect to it

# Connect to the DuckDB database
conn = duckdb.connect(f"{pipeline.pipeline_name}.duckdb")

# Set search path to the dataset
conn.sql(f"SET search_path = '{pipeline.dataset_name}'")

# Describe the dataset
conn.sql("DESCRIBE").df()
```

### **Answer:**

**4**

---

## **Question 3**

```
df = pipeline.dataset(dataset_type="default").rides.df()
df
len(df)
```

### **Answer:**
**10,000**
---

## **Question 4**
```
with pipeline.sql_client() as client:
    res = client.execute_sql(
            """
            SELECT
            AVG(date_diff('minute', trip_pickup_date_time, trip_dropoff_date_time))
            FROM rides;
            """
        )
    # Prints column values of the first row
    print(res)
```
### **Answer:**
**12.3049**
