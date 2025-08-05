# 1stQuest-Project

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
