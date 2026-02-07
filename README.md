# module-3-data-warehousing


# Creating External table:
CREATE OR REPLACE EXTERNAL TABLE `udemy-compute-engine-abrar.nyc_taxi.external_yellow_tripdata_2024`
OPTIONS (
  format = 'PARQUET',
  uris = ['gs://abrar-alam-dezoomcamp-hw3-2026/yellow_tripdata_2024-*.parquet']
);

