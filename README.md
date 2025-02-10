### Setup
```
CREATE OR REPLACE EXTERNAL TABLE taxi-rides-ny.nytaxi.external_yellow_trips
OPTIONS (
    format = 'PARQUET',
    uris = ['gs://de_zoomcamp_week3_hw/yellow_tripdata_2024-*.parquet']
)
```
```
CREATE OR REPLACE TABLE taxi_rides_ny.yellow_trips AS

SELECT * FROM `de-zoomcamp-03-warehousing.taxi_rides_ny.external_yellow_trips`
```

### Question 1
```
SELECT COUNT(*) FROM `de-zoomcamp-03-warehousing.taxi_rides_ny.yellow_trips`
```
Answer: 20332093

### Question 2
```
SELECT COUNT(DISTINCT PULocationID) FROM `de-zoomcamp-03-warehousing.taxi_rides_ny.yellow_trips`
```
155.12 MB
```
SELECT COUNT(DISTINCT PULocationID) FROM `de-zoomcamp-03-warehousing.taxi_rides_ny.external_yellow_trips`
```
0 B

Answer: 0 MB for the External Table and 155.12 MB for the Materialized Table

### Question 3
```
SELECT PULocationID FROM `de-zoomcamp-03-warehousing.taxi_rides_ny.yellow_trips`
SELECT PULocationID, DOLocationID FROM `de-zoomcamp-03-warehousing.taxi_rides_ny.yellow_trips`
```
Answer: BigQuery is a columnar database, and it only scans the specific columns requested in the query. Querying two columns (PULocationID, DOLocationID) requires reading more data than querying one column (PULocationID), leading to a higher estimated number of bytes processed.

### Question 4
```
SELECT 
  COUNT(*)
FROM 
  `de-zoomcamp-03-warehousing.taxi_rides_ny.yellow_trips`
WHERE 
  fare_amount = 0
```
Answer: 8333

### Question 5
Answer: Partition by tpep_dropoff_datetime and Cluster on VendorID
```
CREATE OR REPLACE TABLE taxi-rides-ny.nytaxi.yellow_tripdata_partitoned_clustered
PARTITION BY DATE(tpep_pickup_datetime)
CLUSTER BY VendorID AS
SELECT * FROM taxi-rides-ny.nytaxi.external_yellow_tripdata;
```

### Question 6
```
SELECT DISTINCT VendorID
FROM `de-zoomcamp-03-warehousing.taxi_rides_ny.yellow_trips`
WHERE tpep_dropoff_datetime >= '2024-03-01' and tpep_dropoff_datetime <= '2024-03-15'
```
310.24
```
SELECT DISTINCT VendorID
FROM `de-zoomcamp-03-warehousing.taxi_rides_ny.yellow_trips_partitoned_clustered`
WHERE tpep_dropoff_datetime >= '2024-03-01' and tpep_dropoff_datetime <= '2024-03-15'
```
26.84

Answer: 310.24 MB for non-partitioned table and 26.84 MB for the partitioned table

### Question 7
Answer: GCP Bucket

### Question 8
False
