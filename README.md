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

