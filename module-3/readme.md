# Module 3 Homework

This document contains the answers and SQL queries for the Module 3 Homework of the Data Engineering Zoomcamp.

---

## **Setup**

### **To Create an External Table:**
```sql
CREATE OR REPLACE EXTERNAL TABLE kestra-sandbox-449519.HW3.external_yellow_tripdata
OPTIONS(
    format = 'PARQUET',
    uris = ['gs://de-zoomcamp-week3/yellow_tripdata_2024-*.parquet']
);
```

### **To Create a Regular Table:**
```sql
CREATE OR REPLACE TABLE HW3.yellow_tripdata_2024 AS
SELECT * 
FROM `HW3.external_yellow_tripdata`;
```

---

## **Question 1**

### **Query:**
```sql
SELECT COUNT(*) 
FROM `HW3.external_yellow_tripdata`;
```

### **Answer:**
**20,332,093**

---

## **Question 2**

### **Queries and Answers:**

#### **External Table:**
```sql
SELECT DISTINCT COUNT(PULocationID)
FROM `HW3.external_yellow_tripdata`;
```
**Scanned Data: 0 MB**

#### **Materialized Table:**
```sql
SELECT DISTINCT COUNT(PULocationID)
FROM `HW3.yellow_tripdata_2024`;
```
**Scanned Data: 155.12 MB**

### **Answer:**
**0 MB for the External Table and 155.12 MB for the Materialized Table**

---

## **Question 3**

### **Queries and Scanned Data:**

#### **Single Column Query:**
```sql
SELECT PULocationID
FROM `HW3.yellow_tripdata_2024`;
```
**Scanned Data: 155.12 MB**

#### **Two Column Query:**
```sql
SELECT PULocationID, DOLocationID
FROM `HW3.yellow_tripdata_2024`;
```
**Scanned Data: 310.24 MB**

### **Answer:**
BigQuery is a columnar database, and it only scans the specific columns requested in the query. Querying two columns (**PULocationID, DOLocationID**) requires reading more data than querying one column (**PULocationID**), leading to a higher estimated number of bytes processed.

---

## **Question 4**

### **Query:**
```sql
SELECT COUNT(*)
FROM `HW3.yellow_tripdata_2024`
WHERE fare_amount = 0;
```

### **Answer:**
**8,333**

---

## **Question 5**

### **Query to Create a Partitioned and Clustered Table:**
```sql
CREATE TABLE kestra-sandbox-449519.HW3.yellow_tripdata_2024_partitoned_clustered
PARTITION BY DATE(tpep_dropoff_datetime)
CLUSTER BY VendorID AS
SELECT * FROM `HW3.external_yellow_tripdata`;
```

### **Answer:**
**Partitioned by `tpep_dropoff_datetime` and Clustered on `VendorID`**

---

## **Question 6**

### **Queries and Scanned Data:**

#### **Non-Partitioned Table:**
```sql
SELECT DISTINCT VendorID
FROM `HW3.yellow_tripdata_2024`
WHERE DATE(tpep_dropoff_datetime) BETWEEN '2024-03-01' AND '2024-03-15';
```
**Scanned Data: 310.24 MB**

#### **Partitioned Table:**
```sql
SELECT DISTINCT VendorID
FROM `HW3.yellow_tripdata_2024_partitoned_clustered`
WHERE DATE(tpep_dropoff_datetime) BETWEEN '2024-03-01' AND '2024-03-15';
```
**Scanned Data: 26.84 MB**

### **Answer:**
**310.24 MB for the non-partitioned table and 26.84 MB for the partitioned table**

---

## **Question 7**

### **Answer:**
**GCP Bucket**

---

## **Question 8**

### **Answer:**
**False**

---

## **Question 9**

### **Answer:**
**0 MB because it's already stored in the table's storage info, which doesn't require the table to be scanned again.**
