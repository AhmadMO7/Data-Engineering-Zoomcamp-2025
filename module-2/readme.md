# Module 2 Homework

This document contains the answers and SQL queries for the Module 2 Homework of the Data Engineering Zoomcamp.

---

## Question 1

### Answer:
**128.3 MB**

---

## Question 2

### Answer:
**green_tripdata_2020-04.csv**

---

## Question 3

### Query:
```sql
SELECT COUNT(*) AS Number_2020_rows
FROM `kestra-sandbox-449519.zoomcamp.yellow_tripdata` 
WHERE DATE(tpep_pickup_datetime) BETWEEN '2020-01-01' AND '2020-12-31';
```

### Query Output:
**24,648,663**

### Answer (Nearest Value):
**24,648,499**

---

## Question 4

### Query:
```sql
SELECT COUNT(*) AS Number_2020_rows
FROM `kestra-sandbox-449519.zoomcamp.green_tripdata` 
WHERE DATE(lpep_pickup_datetime) BETWEEN '2020-01-01' AND '2020-12-31';
```

### Query Output:
**1,734,039**

### Answer (Nearest Value):
**1,734,051**

---

## Question 5

### Approach 1: SQL Query
```sql
SELECT COUNT(*) AS Number_2021_3_rows
FROM `kestra-sandbox-449519.zoomcamp.yellow_tripdata` 
WHERE DATE(tpep_pickup_datetime) BETWEEN '2021-03-01' AND '2021-03-31';
```

### Query Output:
**1,925,130**

### Answer (Nearest Value):
**1,925,152**

### Approach 2: BigQuery Interface
If opened directly from BigQuery details, the number of rows displayed is **1,925,152**.

---

## Question 6

### Answer:
**Add a timezone property set to America/New_York in the Schedule trigger configuration.**
