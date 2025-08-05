# 1stQuest Travel Data Analysis Project
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

After analyzing all booking tables and aggregating data by hour, we found that the majority of bookings occur between **14:00 and 14:59 (2 PM – 3 PM)**.

To better understand customer behavior, we grouped booking hours into custom parts of the day:

- **Early Morning** → 00:00–05:59  
- **Morning** → 06:00–11:59  
- **Afternoon** → 12:00–17:59  
- **Evening** → 18:00–21:59  
- **Night** → 22:00–23:59

Based on this, we conclude:

> 📈 **Most bookings are made during the afternoon**, with 2 PM being the peak booking hour across all services.
