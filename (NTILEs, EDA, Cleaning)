--EDA: Select all columns from Table
SELECT * FROM medical_costs LIMIT 10;

--EDA: Find any Null values
SELECT * FROM medical_costs WHERE Age IS NULL OR BMI IS NULL OR Charges IS NULL;

--Clean: Clean data by deleting duplicates
DELETE 
FROM medical_costs
WHERE ID IN (
    SELECT ID 
    FROM medical_costs 
    GROUP BY ID 
    HAVING COUNT(*) > 1
    )
;

--EDA: Retrieve Summary Statistics
SELECT 
    MIN(Age) AS MinAge,
    MAX(Age) AS MaxAge,
    AVG(Age) AS AvgAge,
    MIN(BMI) AS MinBMI,
    MAX(BMI) AS MaxBMI,
    AVG(BMI) AS AvgBMI,
    MIN(Charges) AS MinCharges,
    MAX(Charges) AS MaxCharges,
    AVG(Charges) AS AvgCharges
FROM medical_costs;

--EDA: Check for Missing Values
SELECT
    SUM(CASE WHEN Age IS NULL THEN 1 ELSE 0 END) AS MissingAge,
    SUM(CASE WHEN Sex IS NULL THEN 1 ELSE 0 END) AS MissingSex,
    SUM(CASE WHEN BMI IS NULL THEN 1 ELSE 0 END) AS MissingBMI,
    SUM(CASE WHEN Children IS NULL THEN 1 ELSE 0 END) AS MissingChildren,
    SUM(CASE WHEN Smoker IS NULL THEN 1 ELSE 0 END) AS MissingSmoker,
    SUM(CASE WHEN Region IS NULL THEN 1 ELSE 0 END) AS MissingRegion,
    SUM(CASE WHEN Charges IS NULL THEN 1 ELSE 0 END) AS MissingCharges
FROM medical_costs;

--EDA: Find Distribution and Outliers
SELECT Age, COUNT(*) AS Frequency
FROM medical_costs
GROUP BY Age
ORDER BY Age;

SELECT BMI, COUNT(*) AS Frequency
FROM medical_costs
GROUP BY BMI
ORDER BY BMI;

SELECT Charges, COUNT(*) AS Frequency
FROM medical_costs
GROUP BY Charges
ORDER BY Charges;

--EDA: Identify outliers using quartiles for BMI
WITH Quartiles AS (
    SELECT 
        BMI,
        NTILE(4) OVER (ORDER BY BMI) AS Quartile
    FROM medical_costs
)
SELECT 
    Quartile,
    MIN(BMI) AS MinBMI,
    MAX(BMI) AS MaxBMI
FROM Quartiles
GROUP BY Quartile;

--EDA: Explore Categorical Variables
SELECT Sex, COUNT(*) AS Frequency
FROM medical_costs
GROUP BY Sex;

SELECT Smoker, COUNT(*) AS Frequency
FROM medical_costs
GROUP BY Smoker;

SELECT Region, COUNT(*) AS Frequency
FROM medical_costs
GROUP BY Region;

--EDA: Impact of categorical variables on Charges
SELECT Sex, ROUND(AVG(Charges),2) AS AvgCharges
FROM medical_costs
GROUP BY Sex;
--Finding 1/5: males typically spend over 1k more than females

SELECT Smoker, ROUND(AVG(Charges),2) AS AvgCharges
FROM medical_costs
GROUP BY Smoker;
--Finding 2/5: Smoking greatly affects medical charges

SELECT Region, ROUND(AVG(Charges),2) AS AvgCharges
FROM medical_costs
GROUP BY Region;
--Finding 3/5: southeast region typically spends at least 1k more than any other region

--EDA: Count + AVG charge per BMI group
SELECT 
    CASE
        WHEN BMI < 18.5 THEN 'Underweight'
        WHEN BMI BETWEEN 18.5 AND 24.9 THEN 'Normal weight'
        WHEN BMI BETWEEN 25 AND 29.9 THEN 'Overweight'
    ELSE 'Obese'
   END AS BMICategory
,COUNT(*)
,ROUND(AVG(Charges), 2) AS AverageCharges
FROM medical_costs
GROUP BY BMICategory
ORDER BY COUNT(*) DESC;
--Finding 4/5: BMI correlates with higher medical expenses

-- Find age group and expenses
SELECT 
    CASE
        WHEN age BETWEEN 0 AND 19 THEN 'Teens'
        WHEN age BETWEEN 20 AND 29 THEN '20s'
        WHEN age BETWEEN 30 AND 39 THEN '30s'
        WHEN age BETWEEN 40 AND 49 THEN '40s'    
        WHEN age BETWEEN 50 AND 59 THEN '50s'
        WHEN age >= 60 THEN '60+'
        ELSE 0
   END AS AgeRange
,COUNT(*)
,ROUND(AVG(Charges), 2) AS AverageCharges
FROM medical_costs
GROUP BY AgeRange
ORDER BY averagecharges DESC;

-- Find avg bmi by age group
WITH agegroup as (
    SELECT 
    bmi
    ,age
    ,charges 
    ,CASE
        WHEN age BETWEEN 0 AND 19 THEN 'Teens'
        WHEN age BETWEEN 20 AND 29 THEN '20s'
        WHEN age BETWEEN 30 AND 39 THEN '30s'
        WHEN age BETWEEN 40 AND 49 THEN '40s'    
        WHEN age BETWEEN 50 AND 59 THEN '50s'
        WHEN age >= 60 THEN '60+'
        ELSE 0
   END AS age_range
   FROM medical_costs
   )
SELECT
age_range
,ROUND(AVG(bmi),1) as avg_bmi
,ROUND(AVG(Charges), 2) AS AverageCharges

FROM agegroup
GROUP BY Age_Range
ORDER BY averagecharges desc
--Finding 5/5: Average charge is correleated with age
;
----------------------------------------
Export Clean Data to Tableau for visualizations of findings.

