## Question 6
```sql
WITH churn AS
( 
  SELECT *,
  DENSE_RANK() OVER(PARTITION BY customer_id ORDER BY start_date)
  FROM subscriptions JOIN plans 
  ON subscriptions.plan_id = plans.plan_id
  WHERE plan_name NOT LIKE 'trial'
)

SELECT plan_name,COUNT(customer_id),
ROUND(COUNT(customer_id) * 100.0 / ( SELECT COUNT(DISTINCT customer_id )
  FROM subscriptions ) ,2) AS percentage
FROM churn
```
<img width="1149" height="228" alt="image" src="https://github.com/user-attachments/assets/e4754ddf-56ea-4d13-be81-21e459f73dd5" />

## Question 7
```sql
WITH q7 AS
(
  SELECT customer_id,plan_name,start_date,
  DENSE_RANK() OVER(PARTITION BY customer_id ORDER BY start_date DESC) AS rank
  FROM subscriptions JOIN plans
  ON subscriptions.plan_id = plans.plan_id
  WHERE start_date < '2021-01-01'::timestamp
)

SELECT plan_name,COUNT(customer_id),
ROUND( COUNT(customer_id) * 100.0 / (SELECT COUNT(DISTINCT customer_id) FROM subscriptions ),2 ) AS percentage
FROM q7
WHERE rank = 1
GROUP BY plan_name
```
<img width="1211" height="272" alt="image" src="https://github.com/user-attachments/assets/a3beaaa1-58bd-4959-ab2f-576cf541fe59" />

## Question 8
```sql
SELECT COUNT(customer_id)
FROM subscriptions
WHERE plan_id = 3 AND EXTRACT( YEAR FROM start_date ) = 2020
```
<img width="371" height="94" alt="Screenshot 2026-05-29 at 1 19 12 PM" src="https://github.com/user-attachments/assets/ec3e053f-f1af-46a1-8c57-3b8afff97513" />

## Question 9
```sql
SELECT AVG( m.start_date - e.start_date)
FROM subscriptions e  JOIN subscriptions m
ON e.customer_id = m.customer_id
WHERE e.plan_id = 0 AND m.plan_id = 3
```
<img width="250" height="96" alt="Screenshot 2026-05-29 at 3 38 27 PM" src="https://github.com/user-attachments/assets/c6bb62ba-bce2-47cf-84db-3cc5fdfa3098" />

## Question 10
```sql
WITH q9 AS
( 
  SELECT e.customer_id,
  FLOOR((m.start_date - e.start_date)/30) + 1 AS upgrade_window
  FROM subscriptions e JOIN subscriptions m 
  ON e.customer_id = m.customer_id
  WHERE e.plan_id = 0 AND m.plan_id = 3
)

SELECT upgrade_window ,COUNT(customer_id)
FROM q9
GROUP BY upgrade_window;
```
<img width="1360" height="523" alt="Screenshot 2026-05-29 at 4 50 49 PM" src="https://github.com/user-attachments/assets/f542e5b6-99a9-4794-a5cd-c3a96f880eeb" />

## Question 11
```sql
WITH q9 AS
( 
  SELECT *,
  EXTRACT(YEAR FROM m.start_date ) AS down_year
  FROM subscriptions e JOIN subscriptions m 
  ON e.customer_id = m.customer_id
  WHERE e.plan_id = 2 AND m.plan_id = 1 AND e.start_date <        m.start_date
)

SELECT *
FROM q9
WHERE down_year = 2020
```
<img width="1146" height="91" alt="Screenshot 2026-05-29 at 4 58 24 PM" src="https://github.com/user-attachments/assets/0f8dde62-ea68-46f2-992d-731918357331" />



