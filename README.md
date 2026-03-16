# Taxi-demand-predictor
An end-to-end pipeline of a machine-learning based product that predicts taxi demand in New York.  
The model will be adapted for the resources of a local machine.

## Contents
[About problem](#about-problem)  
[Exploratory data analysis](#eda)

## About problem
The task is to predict the approximate time and place of a next taxi order, using data from the past taxi demand.  
Data source: https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page  
(information about 'yellow taxi' trips is used)  
Data dictionary from the official [website](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page):

| Field Name | Description |
|------------|-------------|
| **VendorID** | A code indicating the TPEP provider that provided the record.<br>1 = Creative Mobile Technologies, LLC<br>2 = Curb Mobility, LLC<br>6 = Myle Technologies Inc<br>7 = Helix |
| **tpep_pickup_datetime** | The date and time when the meter was engaged. |
| **tpep_dropoff_datetime** | The date and time when the meter was disengaged. |
| **passenger_count** | The number of passengers in the vehicle. |
| **trip_distance** | The elapsed trip distance in miles reported by the taximeter. |
| **RatecodeID** | The final rate code in effect at the end of the trip.<br>1 = Standard rate<br>2 = JFK<br>3 = Newark<br>4 = Nassau or Westchester<br>5 = Negotiated fare<br>6 = Group ride<br>99 = Null/unknown |
| **store_and_fwd_flag** | This flag indicates whether the trip record was held in vehicle memory before sending to the vendor, aka "store and forward," because the vehicle did not have a connection to the server.<br>Y = store and forward trip<br>N = not a store and forward trip |
| **PULocationID** | TLC Taxi Zone in which the taximeter was engaged. |
| **DOLocationID** | TLC Taxi Zone in which the taximeter was disengaged. |
| **payment_type** | A numeric code signifying how the passenger paid for the trip.<br>0 = Flex Fare trip<br>1 = Credit card<br>2 = Cash<br>3 = No charge<br>4 = Dispute<br>5 = Unknown<br>6 = Voided trip |
| **fare_amount** | The time-and-distance fare calculated by the meter. For additional information on the following columns, see https://www.nyc.gov/site/tlc/passengers/taxi-fare.page |
| **extra** | Miscellaneous extras and surcharges. |
| **mta_tax** | Tax that is automatically triggered based on the metered rate in use. |
| **tip_amount** | Tip amount – This field is automatically populated for credit card tips. Cash tips are not included. |
| **tolls_amount** | Total amount of all tolls paid in trip. |
| **improvement_surcharge** | Improvement surcharge assessed trips at the flag drop. The improvement surcharge began being levied in 2015. |
| **total_amount** | The total amount charged to passengers. Does not include cash tips. |
| **congestion_surcharge** | Total amount collected in trip for NYS congestion surcharge. |
| **airport_fee** | For pick up only at LaGuardia and John F. Kennedy Airports. |
| **cbd_congestion_fee** | Per-trip charge for MTA's Congestion Relief Zone starting Jan. 5, 2025. |


## EDA
The data is extremely dirty, has a lot of missing and nonsensical values, such as negative ride costs and 0 passengers.  
The [notebook](https://github.com/Tukk0/Taxi-demand-predictor/blob/main/notebooks/EDA.ipynb) with explanations.  

The interesting problems with data include pairs of rows, in which trip data is the same, but the monetary values are opposite:  
| # | VendorID | tpep_pickup_datetime | tpep_dropoff_datetime | passenger_count | trip_distance | RatecodeID | store_and_fwd_flag | PULocationID | DOLocationID | payment_type | fare_amount | extra | mta_tax | tip_amount | tolls_amount | improvement_surcharge | total_amount | congestion_surcharge | Airport_fee | cbd_congestion_fee |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 23 | 2 | 2025-07-01 00:10:28 | 2025-07-01 00:28:12 | 1.0 | 10.00 | 1.0 | N | 138 | 114 | 4 | -39.4 | -6.00 | -0.5 | 0.00 | 0.00 | -1.0 | -51.90 | -2.5 | -1.75 | -0.75 |
| 24 | 2 | 2025-07-01 00:10:28 | 2025-07-01 00:28:12 | 1.0 | 10.00 | 1.0 | N | 138 | 114 | 4 | 39.4 | 6.00 | 0.5 | 0.00 | 0.00 | 1.0 | 51.90 | 2.5 | 1.75 | 0.75 |



After rigorous data cleaning process every feature was inspected by itself and cleaned of outliers.  
Kolmogorov-Smirnov and Anderson-Darling tests were used to come to the conclusion that the distribution of ride durations (in hours) is lognormal.  
![Duration hours density](pictures/duration_hours_density.png)

In progress...
