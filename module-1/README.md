1) docker run -it --entrypoint bash python:3.12.8
    pip --version

    answer :24.3.1

2)
answer: postgres:5432 or db:5432



3)
select 
	SUM(CASE WHEN trip_distance <= 1 THEN 1 ELSE 0 END) AS "Up to 1 mile",
    SUM(CASE WHEN trip_distance > 1 AND trip_distance <= 3 THEN 1 ELSE 0 END) AS "between 1 (exclusive) and 3 miles (inclusive)",
    SUM(CASE WHEN trip_distance > 3 AND trip_distance <= 7 THEN 1 ELSE 0 END) AS "In between 3 (exclusive) and 7 miles (inclusive)",
    SUM(CASE WHEN trip_distance > 7 AND trip_distance <= 10 THEN 1 ELSE 0 END) AS "In between 7 (exclusive) and 10 miles (inclusive)",
    SUM(CASE WHEN trip_distance > 10 THEN 1 ELSE 0 END) AS "Over 10 miles"
from green_taxi_data
where lpep_dropoff_datetime >= '2019-10-01' 
and lpep_dropoff_datetime < '2019-11-01'

answer : 104,802; 198,924; 109,603; 27,678; 35,189


4)
select lpep_pickup_datetime
from green_taxi_data
where trip_distance = (select max(trip_distance) from green_taxi_data)

answer = 2019-10-31 23:23:41

5)
SELECT gtz."Zone",SUM(total_amount)
from green_taxi_data gtd
left join green_taxi_zones gtz
ON (gtd."PULocationID" = gtz."LocationID")
where DATE(gtd.lpep_pickup_datetime) = '2019-10-18'
GROUP BY gtz."Zone"
HAVING SUM(total_amount)>13000
ORDER By SUM(total_amount) DESC

answer : East Harlem North, East Harlem South, Morningside Heights

6)
select gtz2."Zone",Max(tip_amount)
from green_taxi_data gtd
left join green_taxi_zones gtz1
ON (gtd."PULocationID" = gtz1."LocationID")
left join green_taxi_zones gtz2
ON (gtd."DOLocationID" = gtz2."LocationID")
where DATE(gtd.lpep_pickup_datetime) between '2019-10-01' and '2019-10-31'
and gtz1."Zone" = 'East Harlem North'
Group by gtz2."Zone"
ORDER BY Max(tip_amount) DESC
LIMIT 1

answer:JFK Airport


7)terraform init, terraform apply -auto-approve, terraform destroy