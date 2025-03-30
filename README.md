# BSAD384 Term Project

## Overview

This project explores the housing affordability crisis in Ottawa by designing a relational database that models key factors contributing to the issue. The data structure supports analysis on residents, housing transactions, occupations, rental agreements, subsidies, homelessness, and cost-of-living metrics.

The project includes:
- A conceptual and logical ERD
- A normalized database design
- SQL queries for analysis
- A backup of the full database and the create/insert SQL script

---

## Conceptual & Logical Design

### Conceptual ERD

![Conceptual ERD](images/ERD-Conceptual.png)

---

### Logical ERD

![Logical ERD](images/ERD-Logical.png)

---

## Database Files

- 🔗 [Database Backup File (.bak)](https://stfxcamy.sharepoint.com/:u:/g/personal/x2022akx_stfx_ca/EaQAQZnJnjJPpq0vXC00qewBDyLvqlXgVi1_4hNgNOADvg)
- 🔗 [Create Table + Insert Statements](https://stfxca-my.sharepoint.com/:u:/g/personal/x2022akx_stfx_ca/EUDbdE5IcZJDspc2jF-j5FcBnmy61EKet8dSSWuKKVWEwA)

---

## Sample Queries

```sql
-- Query 1: List all residents and their current rental agreement details
SELECT r.ResidentID, r.Name, ra.PropertyID, ra.StartDate, ra.EndDate
FROM Resident r
LEFT JOIN Rental_Agreement ra 
    ON r.ResidentID = ra.ResidentID 
    AND (ra.EndDate IS NULL OR ra.EndDate >= GETDATE());

-- Query 2: List all properties with their city and current resident name
SELECT p.PropertyID, p.PropertyType, c.CityName, r.Name AS CurrentResident
FROM Property p
JOIN City_Subsidy_Rate c 
    ON p.CityID = c.CityID
LEFT JOIN Resident r 
    ON p.ResidentID = r.ResidentID;

-- Query 3: Calculate the total subsidy amount for each resident
SELECT r.ResidentID, r.Name, SUM(ra.SubsidyAmount) AS TotalSubsidy
FROM Resident r
LEFT JOIN Rental_Agreement ra 
    ON r.ResidentID = ra.ResidentID
GROUP BY r.ResidentID, r.Name;

-- Query 4: Find the average homeless rate for properties in each city
SELECT c.CityName, AVG(p.HomelessRate) AS AverageHomelessRate
FROM Property p
JOIN City_Subsidy_Rate c 
    ON p.CityID = c.CityID
GROUP BY c.CityName;

-- Query 5: List residents who have had more than one occupation
SELECT r.ResidentID, r.Name, COUNT(o.OccupationID) AS NumberOfOccupations
FROM Resident r
JOIN Occupation o 
    ON r.ResidentID = o.ResidentID
GROUP BY r.ResidentID, r.Name
HAVING COUNT(o.OccupationID) > 1;
