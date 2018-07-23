{:title "a bigquery example"
:layout :post
 :tags ["old-post"]}



I had read about BigQuery but hadn't had a chance to use the external product yet. This week I came across some article about Uber and taxis etc, and it had a link to recently released data by [The NYC Taxi and Limousine Commission](http://www.nyc.gov/html/tlc/html/about/statistics.shtml). In addition to directly downloading the data, or looking at it through [NYC OpenData](https://data.cityofnewyork.us/data?agency=Taxi+and+Limousine+Commission+%28TLC%29&cat=&type=new_view&browseSearch=&scope), you can also [play around with it using BigQuery](https://bigquery.cloud.google.com/table/nyc-tlc:yellow.trips_2015).



So here's a sample I threw together:



```sql

SELECT trip_distance, fare_amount, tip_amount, tolls_amount, round(100.0 * tip_amount / fare_amount) as tip_ratio

FROM [nyc-tlc:yellow.trips_2015]

WHERE fare_amount > 50 AND trip_distance > 15 AND passenger_count > 8

ORDER BY trip_distance DESC

```



The screenshot shows the cached results, it took about 6 seconds to run (which is not bad, considering the dataset is about _10 Gigs_ !!)



![bigquery example](https://lh3.googleusercontent.com/rdJeePtMNq4uPKHvPWRNioZfZpQcqMe86rt-2M1bUAJJ=w1019-h665-no)



_(9 people went 27 miles, paid $95 and gave no tip!)_
