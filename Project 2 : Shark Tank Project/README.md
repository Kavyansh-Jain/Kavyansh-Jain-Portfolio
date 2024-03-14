# Shark Tank Project

This project analyzes a Shark Tank dataset using SQL queries and Power BI to gain insights into various aspects of the pitches, investments, and demographics of participants.

## Data Source

* Shark Tank dataset (attached to the repository)

## Analysis Tools

* SQL
* Power BI

## Steps

**1. Data Exploration**

The analysis covers different dimensions of the Shark Tank dataset using SQL queries (provided in separate files within the repository):

* **Total Episodes:**
    ```sql
    SELECT MAX(epno) FROM project..data;
    SELECT COUNT(DISTINCT epno) FROM project..data;
    ```

* **Total Pitches:**
    ```sql
    SELECT COUNT(DISTINCT brand) FROM project..data;
    ```

* **Pitches Converted:**
    ```sql
    SELECT CAST(SUM(a.converted_not_converted) AS FLOAT) / CAST(COUNT(*) AS FLOAT) 
    FROM (
        SELECT amountinvestedlakhs , CASE WHEN amountinvestedlakhs > 0 THEN 1 ELSE 0 END AS converted_not_converted 
        FROM project..data
    ) a;
    ```

* **Total Male and Female Participants:**
    ```sql
    SELECT SUM(male) FROM project..data;
    SELECT SUM(female) FROM project..data;
    ```

* **Gender Ratio:**
    ```sql
    SELECT SUM(female) / SUM(male) FROM project..data;
    ```

* **Total Invested Amount:**
    ```sql
    SELECT SUM(amountinvestedlakhs) FROM project..data;
    ```

* **Average Equity Taken:**
    ```sql
    SELECT AVG(a.equitytakenp) FROM (SELECT * FROM project..data WHERE equitytakenp > 0) a;
    ```

* **Highest Deal Taken:**
    ```sql
    SELECT MAX(amountinvestedlakhs) FROM project..data;
    ```

* **Highest Equity Taken:**
    ```sql
    SELECT MAX(equitytakenp) FROM project..data;
    ```

* **Startups with At Least One Woman:**
    ```sql
    SELECT SUM(a.female_count) AS startups_having_at_least_women 
    FROM (
        SELECT female, CASE WHEN female > 0 THEN 1 ELSE 0 END AS female_count 
        FROM project..data
    ) a;
    ```

* **Pitches Converted with At Least One Woman:**
    ```sql
    SELECT SUM(b.female_count) 
    FROM (
        SELECT CASE WHEN a.female > 0 THEN 1 ELSE 0 END AS female_count, a.*
        FROM (SELECT * FROM project..data WHERE deal != 'No Deal') a
    ) b;
    ```

* **Average Team Members:**
    ```sql
    SELECT AVG(teammembers) FROM project..data;
    ```

* **Amount Invested per Deal:**
    ```sql
    SELECT AVG(a.amountinvestedlakhs) AS amount_invested_per_deal 
    FROM (SELECT * FROM project..data WHERE deal != 'No Deal') a;
    ```

* **Average Age Group of Contestants:**
    ```sql
    SELECT avgage, COUNT(avgage) AS cnt 
    FROM project..data 
    GROUP BY avgage 
    ORDER BY cnt DESC;
    ```

* **Location Group of Contestants:**
    ```sql
    SELECT location, COUNT(location) AS cnt 
    FROM project..data 
    GROUP BY location 
    ORDER BY cnt DESC;
    ```

* **Sector Group of Contestants:**
    ```sql
    SELECT sector, COUNT(sector) AS cnt 
    FROM project..data 
    GROUP BY sector 
    ORDER BY cnt DESC;
    ```

* **Partner Deals:**
    ```sql
    SELECT partners, COUNT(partners) AS cnt 
    FROM project..data 
    WHERE partners != '-' 
    GROUP BY partners 
    ORDER BY cnt DESC;
    ```

* **Highest Amount Invested in Each Sector:**
    ```sql
    SELECT c.* 
    FROM (
        SELECT brand, sector, amountinvestedlakhs, RANK() OVER(PARTITION BY sector ORDER BY amountinvestedlakhs DESC) AS rnk 
        FROM project..data
    ) c 
    WHERE c.rnk = 1;
    ```

**2. Dashboard Creation**

The insights derived from the SQL analysis were visualized using Power BI to create an interactive dashboard, providing a comprehensive overview of the Shark Tank project metrics, demographics, and investment trends.
