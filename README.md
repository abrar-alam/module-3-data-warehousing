# module-3-data-warehousing


# Creating External table:
CREATE OR REPLACE EXTERNAL TABLE `udemy-compute-engine-abrar.nyc_taxi.external_yellow_tripdata_2024`
OPTIONS (
  format = 'PARQUET',
  uris = ['gs://abrar-alam-dezoomcamp-hw3-2026/yellow_tripdata_2024-*.parquet']
);

# Creating BQ Native Non Partitioned, Non Clustered table:
CREATE OR REPLACE TABLE `udemy-compute-engine-abrar.nyc_taxi.yellow_tripdata_2024_non_partioned_non_clustered` AS
SELECT * FROM `udemy-compute-engine-abrar.nyc_taxi.external_yellow_tripdata_2024`;

#  Q. 1 Total number of records:
SELECT COUNT(*) FROM `udemy-compute-engine-abrar.nyc_taxi.yellow_tripdata_2024_non_partioned_non_clustered` ;

The query gave us 20,332,093

# Q. 2 COUNT DISTINCT PLULocation IDS and MB processed
SELECT COUNT( DISTINCT PULocationID ) FROM `udemy-compute-engine-abrar.nyc_taxi.yellow_tripdata_2024_non_partioned_non_clustered` ;

This gave us 155.12 MB

SELECT COUNT( DISTINCT PULocationID ) FROM `udemy-compute-engine-abrar.nyc_taxi.external_yellow_tripdata_2024` ;

This gave us 0 B (GCP can't approximate MB processed on external table)

# Question 3. Understanding columnar storage

BigQuery is a columnar database, and it only scans the specific columns requested in the query. Querying two columns (PULocationID, DOLocationID) requires reading more data than querying one column (PULocationID), leading to a higher estimated number of bytes processed.

# Question 4. Counting zero fare trips
 SELECT COUNT(*) FROM `udemy-compute-engine-abrar.nyc_taxi.yellow_tripdata_2024_non_partioned_non_clustered`
WHERE fare_amount = 0;

This gave us 8333

# Question 5: Partitioning and clustering

CREATE OR REPLACE TABLE `udemy-compute-engine-abrar.nyc_taxi.yellow_tripdata_2024_partioned_clustered`
PARTITION BY DATE(tpep_dropoff_datetime)
CLUSTER BY VendorID AS
SELECT * FROM `udemy-compute-engine-abrar.nyc_taxi.external_yellow_tripdata_2024`;

So, Partition by tpep_dropoff_datetime and Cluster on VendorID

# Question 6:

SELECT DISTINCT VendorID from `udemy-compute-engine-abrar.nyc_taxi.yellow_tripdata_2024_non_partioned_non_clustered`
WHERE DATE(tpep_dropoff_datetime) >= '2024-03-01' AND DATE(tpep_dropoff_datetime) <= '2024-03-15'

This estimates 310.24 MB

SELECT DISTINCT VendorID from `udemy-compute-engine-abrar.nyc_taxi.yellow_tripdata_2024_partioned_clustered`
WHERE DATE(tpep_dropoff_datetime) >= '2024-03-01' AND DATE(tpep_dropoff_datetime) <= '2024-03-15'

This estimates 26.84 MB

# Question 7. External table storage

Ans: GCP Bucket

# Question 8. Clustering best practices

False


