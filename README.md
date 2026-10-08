# 🚗 ZoomRide SQL Project

## 📌 Overview

ZoomRide is a ride-hailing company operating in multiple African cities. This project analyzes trip data to identify which city and vehicle type the company should invest in for growth.

---

##  ## 🎯 Business Problem
The ZoomRide manager wants answers to three questions:
1.Which city should ZoomRide invest in next?
2.Which vehicle type is most profitable? 
3.What was the best performing month?

The data was deliberately messy: misspelled city names, trips saved twice, and missing fares. The job was to explore the data, clean it, answer the questions with SQL, and send the manager a short recommendation.
> **Money rule:** only trips with `status = 'Completed'` count as revenue. Cancelled trips earn nothing.

---

## 🗂️ Database Schema

| Table | Description | Key columns |
|---|---|---|
| `customers` | 40 riders | `customer_id` (PK), `customer_name`, `home_city`, `signup_date`, `signup_channel` |
| `drivers` | 20 drivers | `driver_id` (PK), `driver_name`, `city`, `vehicle_type`, `rating`, `joined_date` |
| `trips` | One row per trip | `trip_id` (PK), `customer_id`, `driver_id`, `city`, `trip_date`, `distance_km`, `fare`, `status`, `payment_method` |

**How the tables link:** `trips.customer_id → customers.customer_id` and `trips.driver_id → drivers.driver_id`

- **Vehicle types:** Economy, Comfort, Bike
- **Trip status:** Completed, Cancelled
- **Payment methods:** Cash, Card, Wallet

---

## 🧹 Data Quality Issues Found & Fixed

A simple `GROUP BY city` returned **12 city names instead of 6**:

| Issue | Examples | Fix |
|---|---|---|
| Extra spaces | `' Lagos'`, `' Accra'` | `UPDATE ... SET city = TRIM(city)` |
| Inconsistent naming | `PH`, `Port-Harcourt`, `Port Harcourt` | Standardised to `Port Harcourt` |
| Misspellings | `Nairobbi`, `Kampla` | Corrected to `Nairobi`, `Kampala` |
| Duplicate trips | Same customer, driver, date and fare under different `trip_id`s | Kept the earliest `trip_id`, deleted the later copy |
| Missing fares | Completed trips with `NULL` fare | Counted and reported, **not** filled in with invented values |

I re-ran the city count and the duplicate check after cleaning to confirm the fixes worked.

---

## ❓ Questions Answered

| # | Question | SQL techniques |
|---|---|---|
| Q1 | How many rows are in `trips`? | `COUNT(*)` |
| Q2 | The 5 longest completed trips | `WHERE`, `ORDER BY`, `LIMIT` |
| Q3 | Trips per city (and why the result looked wrong) | `GROUP BY` |
| Q4a | Find duplicate trips | `GROUP BY` + `HAVING`, `MIN`/`MAX` |
| Q4b | Completed trips with a missing fare | `IS NULL` |
| Q5 | Fix city names and delete duplicates | `UPDATE`, `TRIM`, `DELETE` with subquery |
| Q6 | Revenue, trip count and average fare by city | `SUM`, `AVG`, `ROUND` |
| Q7 | Revenue by month | `DATE_FORMAT` |
| Q8 | Revenue by vehicle type | `INNER JOIN` |
| Q9 | Customers who never booked a trip | `LEFT JOIN` + `IS NULL` |
| Q10 | Top 3 customers by total spend | `JOIN`, `SUM`, `ORDER BY`, `LIMIT` |

---

## 📈 Key Findings & Recommendation

- **Lagos** earned the most revenue (**₦119,650 from 47 trips**), well ahead of Accra and Abuja.
- **Comfort** vehicles earned the most by vehicle type (**₦142,510**).
- **May** was the peak revenue month (**₦64,140**).
- Leaving the data uncleaned would have **overstated revenue and trip counts** because of duplicate trips and inconsistent city names.

**Recommendation to the manager:** Invest in **Lagos**. Before a major investment, study driver supply and customer demand there to confirm the growth can be sustained.

---
🛠️ Tools Used
- MySQL	Database and query language
- MySQL Workbench	Writing and running the SQL script

---

## 🛠️ Skills Demonstrated

- Creating databases and tables, and loading data
- Data cleaning: standardising text, handling `NULL`s, removing duplicates
- Filtering, grouping and aggregating (`WHERE`, `GROUP BY`, `HAVING`)
- `INNER JOIN` and `LEFT JOIN`
- Date functions and subqueries
- Turning query results into a clear business recommendation

---
## 📝 Conclusion

This analysis clearly shows that Lagos is ZoomRide's most valuable market, generating the highest revenue and trip volume despite data quality challenges like duplicate records and inconsistent city names. While Economy vehicles had more trips, Comfort vehicles delivered higher profitability, indicating customers are willing to pay more for comfort.
For future growth, ZoomRide should prioritize investment in Lagos, specifically expanding its Comfort fleet during peak months like May. Fixing data quality issues at source will also ensure more accurate reporting going forward. This data-driven approach ensures the company invests where demand and profit are highest.



