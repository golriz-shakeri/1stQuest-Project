# 1stQuest Travel Data Analysis Project

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







In this project we are analyzing the booking trends

1st Question:
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
