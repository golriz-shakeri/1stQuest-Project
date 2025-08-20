# 1stQuest Data Analysis Project
<br><br>
<p align= "center">
  <img src="1stquest_logo_black.svg" alt="1stQuest Logo" width="400" height="100">
</p>
<br><br>


**Author:** Golriz Shakeri  
**Date:** 04 August 2025  

This portfolio project analyzes booking patterns across multiple travel-related datasets (Experiences, Hotels, Pickups, Visas, Insurances, Transportations, and Manual Bookings) to answer business questions using SQL and Tableau. The goal is to showcase advanced data manipulation and storytelling skills using real-world data.

---

## 📌 Business Questions

1. **When are most of our bookings made?** (Night or day? What times?)
2. **Which weekdays have the most bookings?**
3. **What percentage of bookings occur on Thursdays and Fridays?**
4. **Which hotels are most booked by Chinese and Russian users?**
5. **Which hotel types are booked by tourists vs. business travelers?**

---

## 🛠 Tools Used

- SQL Server (SSMS)
- Tableau
- Excel (pre-cleaning)
- GitHub for version control & documentation

## 📌 Question 1: During What Hours Are Most Bookings Made?

In this query, I extracted the **hour** from the `created_at` timestamps across all booking-related tables using the `DATEPART(HOUR, created_at)` function. Then, I combined the results using `UNION ALL` to include all bookings regardless of type (experiences, hotels, insurances, pickups, visas, transportations, manual bookings).

Finally, I grouped the data by `hour_of_day` and counted total bookings for each hour to identify peak booking hours.

### 🧠 SQL Query:



```sql
SELECT hour_of_day, COUNT (*) AS total_bookings
FROM(
SELECT DATEPART(HOUR, created_at) AS hour_of_day FROM experience_bookings
UNION ALL
SELECT DATEPART(HOUR, created_at) FROM hotel_bookings
UNION ALL
SELECT DATEPART(HOUR, created_at) FROM insurances
UNION ALL 
SELECT DATEPART(HOUR, created_at) FROM manual_bookings
UNION ALL
SELECT DATEPART(HOUR, created_at) FROM pickup_bookings
UNION ALL
SELECT DATEPART(HOUR, created_at) FROM transportations
UNION ALL
SELECT DATEPART(HOUR, created_at) FROM visas) AS all_booking
GROUP BY hour_of_day
ORDER BY total_bookings 
```

### ✅ Answer:

After analyzing all booking tables and aggregating data by hour, we found that the majority of bookings occur between:

> ⏰ **14:00 and 14:59 (2 PM)**

#### 🔢 Top 10 Booking Hours:

| Hour (24h) | Total Bookings |
|------------|----------------|
| 14         | 3,020          |
| 13         | 2,890          |
| 15         | 2,745          |
| 12         | 2,690          |
| 11         | 2,488          |
| 10         | 2,370          |
| 16         | 2,155          |
| 17         | 1,982          |
| 09         | 1,745          |
| 18         | 1,521          |

This insight helps us understand that **afternoon hours (12 PM – 3 PM)** are the busiest for bookings, potentially ideal for running promotional campaigns or ensuring customer support availability.

---

## 📊 Question 2: Which day of the week are most of our bookings made?

### 🧠 Explanation:

To determine which weekday users are most likely to book services, I extracted the `created_at` timestamps from all 7 booking-related datasets:

- `experience_bookings`  
- `hotel_bookings`  
- `manual_bookings`  
- `pickup_bookings`  
- `transportations`  
- `visas`  
- `insurances`

Using the SQL function `DATENAME(WEEKDAY, created_at)`, I extracted the **name of the weekday** (e.g., Monday, Tuesday) from each timestamp. Then I grouped the bookings by weekday and counted them to find which days had the highest activity.

---

### 💻 SQL Code:

```sql
SELECT 
  DATENAME(WEEKDAY, created_at) AS weekday,
  COUNT(*) AS total_bookings
FROM (
    SELECT created_at FROM experience_bookings WHERE created_at IS NOT NULL
    UNION ALL
    SELECT created_at FROM hotel_bookings WHERE created_at IS NOT NULL
    UNION ALL
    SELECT created_at FROM manual_bookings WHERE created_at IS NOT NULL
    UNION ALL
    SELECT created_at FROM pickup_bookings WHERE created_at IS NOT NULL
    UNION ALL
    SELECT created_at FROM transportations WHERE created_at IS NOT NULL
    UNION ALL
    SELECT created_at FROM visas WHERE created_at IS NOT NULL
    UNION ALL
    SELECT created_at FROM insurances WHERE created_at IS NOT NULL
) AS all_bookings
GROUP BY DATENAME(WEEKDAY, created_at)
ORDER BY total_bookings DESC;
```

---

### ✅ Answer:

After aggregating data from all booking tables, we found that:

> 📅 **Tuesday** is the most popular booking day, with the highest number of bookings across all services.

#### 🔢 Bookings by weekday:

| Weekday     | Total Bookings |
|-------------|----------------|
| Tuesday     | 5,567          |
| Monday      | 5,350          |
| Wednesday   | 4,900          |
| Thursday    | 4,740          |
| Sunday      | 4,706          |
| Saturday    | 3,869          |
| Friday      | 3,299          |



## 📅 Question 3: What percentage of our bookings are made on Thursdays and Fridays?

### 🧠 Explanation:

This SQL query shows what percent of total bookings happen on **Thursdays and Fridays**, across all booking services.

Steps:
1. ✅ Combine all booking timestamps from 7 different datasets.
2. 📆 Use `DATENAME(WEEKDAY, created_at)` to extract the weekday name.
3. 🔢 Count total bookings for each weekday.
4. 📊 Divide Thursday and Friday counts by the total bookings and convert to a percentage.

This is helpful for identifying booking spikes near the weekend.

---

### 💻 SQL Code:

<pre lang="sql">


WITH all_bookings AS (
  SELECT created_at FROM experience_bookings
  UNION ALL SELECT created_at FROM hotel_bookings
  UNION ALL SELECT created_at FROM manual_bookings
  UNION ALL SELECT created_at FROM pickup_bookings
  UNION ALL SELECT created_at FROM transportations
  UNION ALL SELECT created_at FROM visas
  UNION ALL SELECT created_at FROM insurances
),
weekday_totals AS (
  SELECT 
    DATENAME(WEEKDAY, created_at) AS weekday,
    COUNT(*) AS total_bookings
  FROM all_bookings
  GROUP BY DATENAME(WEEKDAY, created_at)
),
total_bookings AS (
  SELECT COUNT(*) AS grand_total FROM all_bookings
)
SELECT 
  weekday,
  total_bookings,
  ROUND(CAST(total_bookings AS FLOAT) / grand_total * 100, 2) AS percentage
FROM weekday_totals, total_bookings
WHERE weekday IN ('Thursday', 'Friday')
ORDER BY percentage DESC;
</pre>

---

### ✅ Answer:

| Weekday  | Total Bookings | Percentage |
|----------|----------------|------------|
| Thursday | 4,740          | 14.62%     |
| Friday   | 3,299          | 10.17%     |

📌 **Total: 24.79% of bookings happen on Thursdays and Fridays**
