# This is the solution for the first week

## I have used **Anaconda**
The commands I used for install anaconda are:
```
conda create -n DataTalk_Couse python=3.12
conda activate DataTalk_Couse
conda install jupyter
conda install sqlalchemy
conda install psycopg2
```

## Question and answers
### Question 1
Command:
```
(base) alvaro@cenac23:~/Documents/Docker/DataTalk$ docker run -it python:3.12.8
Unable to find image 'python:3.12.8' locally
3.12.8: Pulling from library/python
fd0410a2d1ae: Pull complete 
bf571be90f05: Pull complete 
684a51896c82: Pull complete 
fbf93b646d6b: Pull complete 
5f16749b32ba: Pull complete 
e00350058e07: Pull complete 
eb52a57aa542: Pull complete 
Digest: sha256:5893362478144406ee0771bd9c38081a185077fb317ba71d01b7567678a89708
Status: Downloaded newer image for python:3.12.8
Python 3.12.8 (main, Jan 14 2025, 05:32:36) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import pip
>>> pip.__version__
'24.3.1'
>>> 
```
Answer: **24.3.1** 

### Question 2:
Answer: **postgres:5432**

### Question 3:
Command:
```
SELECT count(*)
FROM green_taxi_data a
WHERE a.lpep_dropoff_datetime BETWEEN timestamp '2019-10-01 00:00:00' AND timestamp '2019-10-31 23:59:59'
AND a.trip_distance >=0 AND a.trip_distance <=1 ;
```
**104800**
```
SELECT count(*)
FROM green_taxi_data a
WHERE a.lpep_dropoff_datetime BETWEEN timestamp '2019-10-01 00:00:00' AND timestamp '2019-10-31 23:59:59'
AND a.trip_distance >1 AND a.trip_distance <=3 ;
```
**198924**
```
SELECT count(*)
FROM green_taxi_data a
WHERE a.lpep_dropoff_datetime BETWEEN timestamp '2019-10-01 00:00:00' AND timestamp '2019-10-31 23:59:59'
AND a.trip_distance >3 AND a.trip_distance <=7 ;
```
**109603**
```
SELECT count(*)
FROM green_taxi_data a
WHERE a.lpep_dropoff_datetime BETWEEN timestamp '2019-10-01 00:00:00' AND timestamp '2019-10-31 23:59:59'
AND a.trip_distance >7 AND a.trip_distance <=10 ;
```
**27678**
```
SELECT count(*)
FROM green_taxi_data a
WHERE a.lpep_dropoff_datetime BETWEEN timestamp '2019-10-01 00:00:00' AND timestamp '2019-10-31 23:59:59'
AND a.trip_distance >10 ;
```
**35189** \
Answer: **104800, 198924, 109603, 27678, 35189**

### QUESTION 4
Command:
```
SELECT DATE_TRUNC('day', lpep_pickup_datetime) AS day, MAX(trip_distance)
FROM green_taxi_data
GROUP BY day
ORDER BY 2 DESC;
```

Answer: **2019-10-31**

### QUESTION 5
Command:
```
SELECT tz.service_zone, tz."Zone"
FROM
  taxi_zone tz
WHERE tz."LocationID" IN (
  SELECT g."PULocationID" lid
    FROM green_taxi_data g
    WHERE g.lpep_pickup_datetime BETWEEN timestamp '2019-10-18 00:00:00' AND timestamp '2019-10-18 23:59:59'
    GROUP BY g."PULocationID"
    HAVING sum(g.total_amount) > 13000
  );

[
  {
    "service_zone": "Boro Zone",
    "Zone": "East Harlem North"
  },
  {
    "service_zone": "Boro Zone",
    "Zone": "East Harlem South"
  },
  {
    "service_zone": "Boro Zone",
    "Zone": "Morningside Heights"
  }
]
```
Answer: **East Harlem North, East Harlem South, Morningside Heights**

### QUESTION 6
Command:
```
SELECT MAX(AA.TIP),TZ."LocationID",tz.service_zone, tz."Zone"
FROM
  taxi_zone tz , (SELECT g."DOLocationID", max(g.tip_amount) TIP
    FROM green_taxi_data g
    WHERE g.lpep_pickup_datetime BETWEEN timestamp '2019-10-01 00:00:00' AND timestamp '2019-10-31 23:59:59'
    AND g."PULocationID" = (SELECT tz."LocationID"
                            FROM taxi_zone tz
                            WHERE tz."Zone" = 'East Harlem North')
    GROUP BY g."DOLocationID"
    ORDER BY max(g.tip_amount) DESC) aa
WHERE tz."LocationID" = aa."DOLocationID"
GROUP BY TZ."LocationID", tz.service_zone, tz."Zone"
order by 1 desc;

[
  {
    "max": 87.3,
    "LocationID": 132,
    "service_zone": "Airports",
    "Zone": "JFK Airport"
  }
]
```
Answer: **JFK Airport**

### QUESTION 7
```
terraform init
terraform plan
terraform apply
terraform destroy
```

