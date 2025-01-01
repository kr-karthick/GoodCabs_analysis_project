## 🚖 Goodcabs - Unlocking Data-Driven Growth! 🚀



## 📌 Overview

Welcome to my journey of completing Codebasics Resume Project Challenge #13, where I was tasked with analyzing and providing strategic insights to Goodcabs, a rapidly growing cab service in India’s tier-2 cities. Through this project, I dove deep into the Transportation & Mobility sector, focusing on enhancing growth and improving passenger satisfaction using data.

Goodcabs, operating in 10 tier-2 cities, supports local drivers and aims to offer better experiences to passengers. With ambitious performance goals for 2024, the company needed immediate insights to drive data-driven decision-making and boost performance. 

## 📜 Problem Statement

Goodcabs’ Chief of Operations needed a comprehensive analysis of key performance metrics for trip volume, passenger satisfaction, repeat passenger rate, trip distribution, and more. With the task of analyzing and reporting this critical data, the goal was to provide actionable insights that would help Goodcabs hit its 2024 growth targets and improve passenger experience.

## Goal & Purpose

Key Objectives

1️⃣ Identify patterns in revenue, trips, passengers, and ratings.

Delve into trip volume trends and passenger behavior to recognize growth areas.

2️⃣ Analyze city-specific performance to find growth opportunities.

Identify which cities are performing well and which need attention to meet the growth targets.

3️⃣ Establish correlations between customer behavior, trip distance, and fare.

Investigate how factors like trip distance and fare affect passenger satisfaction and repeat trips.

4️⃣ Enhance the decision-making process with interactive dashboards.

Design an intuitive and self-explanatory dashboard to make data easy to interpret for decision-makers.


## Approach & Tools

1️⃣ Data Cleaning & Analysis

Explored and cleaned the provided dataset to ensure the clarity of insights.
Addressed business questions using SQL and Power BI for data processing and visualization.

2️⃣ Dashboard Creation

Designed an interactive dashboard to display key metrics and city-wise insights, making the data easily accessible and actionable for stakeholders.

3️⃣ Insights & Recommendations

Delivered data-driven strategies to improve operations, enhance passenger satisfaction, and support growth initiatives.

## 🛠️ Tools & Technologies

SQL 🗃️: For data extraction and ad-hoc analysis.

Power Query 🔄: For data cleaning and transformation.

Power BI 📊: For interactive dashboard creation.

Canva 🎨: For presenting insights to stakeholders.

Flaticon 🖼️: For icons.

Colorhunt 🎨: For selecting appealing color schemes.


## Key Learnings

✔️ SQL & Power BI: Improved skills in data manipulation, querying, and visualization.

✔️ Real-world Problem Solving: Applied data analysis to solve business problems and provide actionable insights.

✔️ Transportation & Mobility Sector Insights: Gained deeper knowledge of the cab service industry and how data can be leveraged to drive growth.






Output :![image](https://github.com/user-attachments/assets/9626b2f8-f961-4bc4-b792-8965355aea63)



## 📌 Ad hoc requests:


Q1 Generate a report that displays the total trips, average fare per km, average fare per trip, and the percentage contribution of each city’s trips to the overall trips. This report will help in assessing trip volume, pricing efficiency, and each city’s contribution to the overall trip count.


Query


```Sql 
        SELECT 
    c.city_name AS 'City_Name',
    COUNT(ft.trip_id) AS 'Total_Trips',
    ROUND(SUM(ft.fare_amount) / SUM(ft.distance_travelled_km), 2) AS 'Avg. Fare/Km',
    ROUND(SUM(ft.fare_amount) / COUNT(ft.trip_id), 2) AS 'Avg. Fare/Trip',
    ROUND(COUNT(ft.trip_id) * 100.0 / (SELECT COUNT(*) FROM fact_trips), 2) AS '%_Contribution_to_Total_Trips'
FROM 
    fact_trips ft
INNER JOIN 
    dim_city c ON c.city_id = ft.city_id
GROUP BY 
    c.city_id, c.city_name
ORDER BY 
    COUNT(ft.trip_id) DESC;

```

Q2 Generate a report that evaluates the target performance for trips at the monthly and city level. For each city and month, compare the actual total trips with the target trips and categorize the performance as follows:

-- If actual trips are greater than target trips, mark it as "Above Target".
-- If actual trips are less than or equal to target trips, mark it as "Below Target".

-- Additionally, calculate the % difference between actual and target trips to quantify the performance gap.

Query

```Sql 
        SELECT 
    c.city_name AS 'City_Name', 
    MONTHNAME(ft.date) AS 'Month_Name', 
    COUNT(ft.trip_id) AS 'Actual_Trips', 
    tt.total_target_trips AS 'Target_Trips',
    CASE 
        WHEN COUNT(ft.trip_id) > tt.total_target_trips THEN 'Above Target'
        WHEN COUNT(ft.trip_id) <= tt.total_target_trips THEN 'Below Target'
    END AS 'Performance_Status',
    ROUND((COUNT(ft.trip_id) - tt.total_target_trips) * 100.0 / tt.total_target_trips, 2) AS '%_Difference'
FROM 
    trips_db.fact_trips ft
INNER JOIN 
    targets_db.monthly_target_trips tt 
    ON tt.city_id = ft.city_id AND MONTH(tt.month) = MONTH(ft.date)
INNER JOIN 
    trips_db.dim_city c 
    ON c.city_id = ft.city_id
INNER JOIN 
    trips_db.dim_date d 
    ON d.date = ft.date
GROUP BY 
    ft.city_id, MONTH(ft.date), c.city_name, tt.total_target_trips
ORDER BY 
    c.city_name, MONTH(ft.date);


```



Q3 Generate a report that shows the percentage distribution of repeat passengers by the number of trips they have taken in each city. Calculate the percentage of repeat passengers who took 2 trips, 3 trips, and so on, up to 10 trips.

-- Each column should represent a trip count category, displaying the percentage of repeat passengers who fall into that category out of the total repeat passengers for that city.


Query

```Sql 
        SET SQL_MODE = ' ';

SELECT 
    c.city_name, 
    ROUND((SUM(CASE WHEN drt.trip_count = 2 THEN drt.repeat_passenger_count ELSE 0 END) / 
           (SELECT SUM(repeat_passenger_count) FROM dim_repeat_trip_distribution)) * 100, 2) AS '2_trips_%',
    ROUND((SUM(CASE WHEN drt.trip_count = 3 THEN drt.repeat_passenger_count ELSE 0 END) / 
           (SELECT SUM(repeat_passenger_count) FROM dim_repeat_trip_distribution)) * 100, 2) AS '3_trips_%',
    ROUND((SUM(CASE WHEN drt.trip_count = 4 THEN drt.repeat_passenger_count ELSE 0 END) / 
           (SELECT SUM(repeat_passenger_count) FROM dim_repeat_trip_distribution)) * 100, 2) AS '4_trips_%',
    ROUND((SUM(CASE WHEN drt.trip_count = 5 THEN drt.repeat_passenger_count ELSE 0 END) / 
           (SELECT SUM(repeat_passenger_count) FROM dim_repeat_trip_distribution)) * 100, 2) AS '5_trips_%',
    ROUND((SUM(CASE WHEN drt.trip_count = 6 THEN drt.repeat_passenger_count ELSE 0 END) / 
           (SELECT SUM(repeat_passenger_count) FROM dim_repeat_trip_distribution)) * 100, 2) AS '6_trips_%',
    ROUND((SUM(CASE WHEN drt.trip_count = 7 THEN drt.repeat_passenger_count ELSE 0 END) / 
           (SELECT SUM(repeat_passenger_count) FROM dim_repeat_trip_distribution)) * 100, 2) AS '7_trips_%',
    ROUND((SUM(CASE WHEN drt.trip_count = 8 THEN drt.repeat_passenger_count ELSE 0 END) / 
           (SELECT SUM(repeat_passenger_count) FROM dim_repeat_trip_distribution)) * 100, 2) AS '8_trips_%',
    ROUND((SUM(CASE WHEN drt.trip_count = 9 THEN drt.repeat_passenger_count ELSE 0 END) / 
           (SELECT SUM(repeat_passenger_count) FROM dim_repeat_trip_distribution)) * 100, 2) AS '9_trips_%',
    ROUND((SUM(CASE WHEN drt.trip_count = 10 THEN drt.repeat_passenger_count ELSE 0 END) / 
           (SELECT SUM(repeat_passenger_count) FROM dim_repeat_trip_distribution)) * 100, 2) AS '10_trips_%'
FROM 
    dim_repeat_trip_distribution drt
INNER JOIN 
    dim_city c ON c.city_id = drt.city_id
GROUP BY 
    drt.city_id, c.city_name;



```


Q4 Generate a report that calculates the total new passengers for each city and ranks them based on this value. Identify the top 3 cities with the highest number of new passengers as well as the bottom 3 cities with the lowest number of new passengers, categorizing them as "Top 3" or "Bottom 3" accordingly.


Query

```Sql 
        WITH RankedCities AS (
    SELECT 
        c.city_name, 
        SUM(fps.new_passengers) AS Passenger,
        CASE 
            WHEN RANK() OVER (ORDER BY SUM(fps.new_passengers) DESC) <= 3 THEN 'Top 3'
            WHEN RANK() OVER (ORDER BY SUM(fps.new_passengers) ASC) <= 3 THEN 'Bottom 3'
            ELSE NULL
        END AS City_category
    FROM 
        fact_passenger_summary fps
    INNER JOIN 
        dim_city c ON c.city_id = fps.city_id
    GROUP BY 
        c.city_name
)
SELECT 
    city_name, 
    Passenger, 
    City_category
FROM 
    RankedCities
WHERE 
    City_category IS NOT NULL
ORDER BY 
    City_category, Passenger DESC;




```



Q5 Generate a report that identifies the month with the highest revenue for each city. For each city, display the month_name, the revenue amount for that month, and the percentage contribution of that month’s revenue to the city’s total revenue.



Query

```Sql 
        SET SQL_MODE = ' ';
SELECT 
    city_name, 
    Higest_revenue_month, 
    Revenue, 
    Pecentage_contribution AS 'Pecentage_contribution (%)'
FROM (
    SELECT 
        c.city_name, 
        MONTHNAME(f.date) AS 'Higest_revenue_month', 
        SUM(f.fare_amount) AS 'Revenue', 
        ROUND((SUM(f.fare_amount) / (SELECT SUM(fare_amount) FROM fact_trips)) * 100, 2) AS 'Pecentage_contribution',
        ROW_NUMBER() OVER (PARTITION BY c.city_name ORDER BY SUM(f.fare_amount) DESC) AS revenue_rank
    FROM 
        fact_trips f
    INNER JOIN 
        dim_city c ON c.city_id = f.city_id
    GROUP BY 
        c.city_name, MONTHNAME(f.date)
) ranked_revenue
WHERE revenue_rank = 1;





```


Q6 Generate a report that calculates two metrics:

-- Monthly Repeat Passenger Rate: Calculate the repeat passenger rate for each city and month by comparing the number of repeat passengers to the total passengers.

-- City-wide Repeat Passenger Rate: Calculate the overall repeat passenger rate for each city, considering all passengers across months.




Query

```Sql 
        SELECT 
    city.city_name, 
    MONTHNAME(fp.month) AS 'month_name', 
    fp.total_passengers AS 'total_passengers', 
    fp.repeat_passengers AS 'repeat_passengers',  
    ROUND((fp.repeat_passengers / fp.total_passengers) * 100, 2) AS 'monthly_repeat_pax_rate', 
    city_rates.city_repeat_pax_rate
FROM 
    fact_passenger_summary fp
INNER JOIN dim_city city ON city.city_id = fp.city_id
INNER JOIN (
    SELECT 
        city_id, 
        SUM(total_passengers) AS total_passengers_sum, 
        SUM(repeat_passengers) AS repeat_passengers_sum, 
        ROUND((SUM(repeat_passengers) / SUM(total_passengers)) * 100, 2) AS city_repeat_pax_rate
    FROM 
        fact_passenger_summary
    GROUP BY 
        city_id
) city_rates ON city_rates.city_id = fp.city_id;






```