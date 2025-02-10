## Setup

```
CREATE OR REPLACE EXTERNAL TABLE taxi-rides-ny.nytaxi.external_yellow_trips
OPTIONS (
    format = 'PARQUET',
    uris = ['gs://de_zoomcamp_week3_hw/yellow_tripdata_2024-*.parquet']
)
```