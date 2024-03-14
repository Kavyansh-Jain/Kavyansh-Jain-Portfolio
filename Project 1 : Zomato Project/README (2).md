# Zomato Customer Analysis

This project analyzes a sample Zomato dataset using SQL queries and Power BI visualizations to explore customer spending habits, product popularity, and the impact of the Zomato Gold membership program.


## Analysis Tools

* SQL
* Power BI

## Steps

**1. Data Exploration**

The analysis delves into various aspects of customer behavior through SQL queries (provided in separate files within the repository for security and clarity):

* **Total amount spent by each customer:**
    ```sql
    SELECT a.userid, SUM(b.price) AS total_amt_spent
    FROM sales a
    INNER JOIN product b ON a.product_id = b.product_id
    GROUP BY a.userid;
    ```
* **Number of visits per customer:**
    ```sql
    SELECT userid, COUNT(DISTINCT created_date) AS distinct_days
    FROM sales
    GROUP BY userid;
    ```
* **First product purchased by each customer:**
    ```sql
    SELECT *
    FROM (
        SELECT *, RANK() OVER (PARTITION BY userid ORDER BY created_date) AS rnk
        FROM sales
    ) a
    WHERE rnk = 1;
    ```
* **Most purchased item and purchase frequency:**
    ```sql
    SELECT userid, COUNT(product_id) AS cnt
    FROM sales
    WHERE product_id =
        (SELECT TOP 1 product_id
         FROM sales
         GROUP BY product_id
         ORDER BY COUNT(product_id) DESC)
    GROUP BY userid;
    ```
* **Customer preferences based on purchase history:**
    ```sql
    SELECT *
    FROM (
        SELECT *, RANK() OVER (PARTITION BY userid ORDER BY cnt DESC) AS rnk
        FROM (
            SELECT userid, product_id, COUNT(product_id) AS cnt
            FROM sales
            GROUP BY userid, product_id
        ) a
    ) b
    WHERE rnk = 1;
    ```

**Additional queries explore:**

* Product purchase behavior before and after becoming a member
* Order details and spending for members before joining
* Points earned through purchases (assuming a point system)
* Product with the highest points generation
* Points earned by customers in the first year after joining Gold (assuming a 5 point per 10 Rs. spent system)
* Ranking of all customer transactions
* Ranking of member transactions and marking non-member transactions

**2. Data Visualization**

A Power BI dashboard visually depicts the key findings from the data analysis.


## Insights

By analyzing the Zomato dataset, we can uncover valuable information about:

* Customer spending patterns
* Product preferences
* The effectiveness of the Zomato Gold program in influencing customer behavior
