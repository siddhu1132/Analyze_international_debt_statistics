# Analyze_international_debt_statistics

## Introduction 

It's not that we humans only take debts to manage our necessities. A country may also take debt to manage its economy. For example, infrastructure spending is one costly ingredient required for a country's citizens to lead comfortable lives. The World Bank is the organization that provides debt to countries.

In this project, you are going to analyze international debt data collected by The World Bank. The dataset contains information about the amount of debt (in USD) owed by developing countries across several categories. You are going to find the answers to questions like:

What is the total amount of debt that is owed by the countries listed in the dataset?
Which country owns the maximum amount of debt and what does that amount look like?
What is the average amount of debt owed by countries across different debt indicators?
The data used in this project is provided by The World Bank. It contains both national and regional debt statistics for several countries across the globe as recorded from 1970 to 2015.

## Table of Contents

1. dataset file contains the information of the data to answer the questions

2. notebook.ipynb file is the jupyter notebbok file which is used to run the SQL queries.

## Project Tasks

1) _The World Bank's international debt data_

```sql
%%sql
postgresql:///international_debt
    
SELECT *
FROM international_debt
LIMIT 10;
```

2) _Finding the number of distinct countries_

```sql
%%sql
SELECT COUNT(DISTINCT country_name)
AS total_distinct_countries
FROM international_debt;
```

3) _Finding out the distinct debt indicators_

```sql
%%sql
SELECT DISTINCT indicator_code
AS distinct_debt_indicators
FROM international_debt
ORDER BY 1;
```

4) _Totaling the amount of debt owed by the countries_

```sql
%%sql
SELECT ROUND(SUM(debt)/1000000, 2)
AS total_debt
FROM international_debt;
```

5) _Country with the highest debt_

```sql
%%sql
SELECT 
    country_name, 
    SUM(debt) AS total_debt
FROM international_debt
GROUP BY 1
ORDER BY 2 DESC
LIMIT 1;
```

6) _Average amount of debt across indicators_

```sql
%%sql
SELECT 
    indicator_code AS debt_indicator,
    indicator_name,
    AVG(debt) AS average_debt
FROM international_debt
GROUP BY 1,2
ORDER BY 3 DESC
LIMIT 10;
```

7) _The highest amount of principal repayments_

```sql
%%sql
SELECT 
    country_name, 
    indicator_name
FROM international_debt
WHERE debt = (SELECT 
                 MAX(debt)
             FROM international_debt
             WHERE indicator_code = 'DT.AMT.DLXF.CD');
```

8) _The most common debt indicator_

```sql
%%sql
SELECT indicator_code, COUNT(indicator_code) AS indicator_count
FROM international_debt
GROUP BY 1
ORDER BY 2 DESC,1 DESC
LIMIT 20;
```

9) _Other viable debt issues and conclusion_

```sql
%%sql
SELECT country_name, MAX(debt) AS maximum_debt
FROM international_debt
GROUP BY country_name
ORDER BY 2 DESC
LIMIT 10;
```
