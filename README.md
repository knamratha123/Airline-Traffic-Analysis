# ✈️ International Airlines SQL Analysis 

## 🧩 Problem Statement

As a Data Analyst working with a national aviation board, your mission is to analyze international air traffic trends between Australian cities and global destinations using historical flight data (2003–2020). The goal is to extract actionable insights that can help stakeholders understand international route performance, airline seat capacity, city-level contributions, and travel patterns across time, stops, and regions.

This project will showcase your SQL skills in answering real-world business questions using a wide range of SQL functions such as data filtering, time extraction, grouping, aggregation, conditional logic, and ranking.

## 🎯 Project Objectives

### 1. 📅 Explore Time-Based Trends
- Identify peak travel months and years.
'''SELECT Month_num, sum (all_flights) as max_flights
FROM  Airlines
GROUP BY Month_num
ORDER BY max_flights DESC
LIMIT 3;'''

### 2. 🏆 Rank Top Airlines by Traffic and Capacity
- Find airlines with the highest flight and seat capacity.
'''SELECT 
    Airline,
    SUM(All_flights) AS Total_Flights,
    SUM(max_seats) AS Total_Seat_Capacity
FROM 
    airlines
GROUP BY 
    Airline
ORDER BY 
    Total_Flights DESC, Total_Seat_Capacity DESC
LIMIT 3;'''



### 3. 🌍 Understand Route-Level Performance
- Identify top-performing international routes by flight volume.
'''Select Route, sum(all_flights) as max_flights from airlines
where stops = '0'
group by route
order by max_flights desc;'''
- Find the least-served routes.
'''SELECT australian_city, COUNT(*) AS Leaststop
FROM airlines
WHERE stops = 0
GROUP BY australian_city
ORDER BY Leaststop ASC;'''

### 5. 🗺️ Regional and Country-Level Analysis
- Rank regions based on total flights and seat count.
'''SELECT
  Service_Region,
  SUM(All_Flights) AS total_flights,
  SUM(Max_Seats) AS total_seats,
  RANK() OVER (ORDER BY SUM(All_Flights) DESC) AS flight_rank,
  RANK() OVER (ORDER BY SUM(Max_Seats) DESC) AS seat_rank
FROM airlines
GROUP BY Service_Region
ORDER BY total_flights DESC;'''

- Rank countries based on total flights and seat count.
'''SELECT
  Service_Country,
  SUM("All_Flights") AS total_flights,
  SUM("Max_Seats") AS total_seats,
  RANK() OVER (ORDER BY SUM("All_Flights") DESC) AS flight_rank,
  RANK() OVER (ORDER BY SUM("Max_Seats") DESC) AS seat_rank
FROM airlines
GROUP BY Service_Country
ORDER BY total_flights DESC;'''

- Track growth in travel from Southeast Asia, Northeast Asia
'''SELECT
  Year,
  Service_Region,
  SUM(All_Flights) AS total_flights,
  SUM(Max_Seats) AS total_seats
FROM your_table_name
WHERE "Service_Region" IN ('SE Asia', 'NE Asia')
GROUP BY Year, Service_Region
ORDER BY Year, Service_Region;'''

### 6. 🛑 Flight Stop Analysis
- Analyze flight capacity and counts based on number of stops.
'''SELECT
  stops,
  COUNT(*) AS route_count,
  SUM(all_flights) AS total_flights,
  SUM(max_seats) AS total_seats,
  ROUND(AVG(all_flights)::numeric, 2) AS avg_flights_per_route,
  ROUND(AVG(max_seats)::numeric, 2) AS avg_seats_per_route
FROM your_table_name
GROUP BY stops
ORDER BY stops;'''

- Compare direct vs. connecting routes in terms of performance.
'''SELECT
  CASE 
    WHEN stops = 0 THEN 'Direct'
    ELSE 'Connecting'
  END AS route_type,
  COUNT(*) AS route_count,
  SUM(all_flights) AS total_flights,
  SUM(max_seats) AS total_seats,
  ROUND(AVG(all_flights)::numeric, 2) AS avg_flights_per_route,
  ROUND(AVG(max_seats)::numeric, 2) AS avg_seats_per_route
FROM your_table_name
GROUP BY route_type
ORDER BY route_type;'''

### 7. 🏙️ Australian City-Level Insights
- Find cities with the most international connections.
 ''' SELECT
  australian_city,
  COUNT(DISTINCT international_city) AS total_connections
FROM your_table_name
GROUP BY australian_city
ORDER BY total_connections DESC;'''

- Track yearly growth of outbound flights for each city.
'''SELECT
  year,
  australian_city,
  SUM(all_flights) AS total_outbound_flights,
  LAG(SUM(all_flights)) OVER (PARTITION BY australian_city ORDER BY year) AS prev_year_flights,
  ROUND(
    100.0 * (SUM(all_flights) - LAG(SUM(all_flights)) OVER (PARTITION BY australian_city ORDER BY year)) 
    / NULLIF(LAG(SUM(all_flights)) OVER (PARTITION BY australian_city ORDER BY year), 0), 2
  ) AS growth_percentage
FROM your_table_name
WHERE in_out = 'O'
GROUP BY year, australian_city
ORDER BY australian_city, year;'''


## 🛠️ Tools Used
- **PostgreSQL** for SQL querying
- **Markdown / GitHub** for documentation and version control

---
