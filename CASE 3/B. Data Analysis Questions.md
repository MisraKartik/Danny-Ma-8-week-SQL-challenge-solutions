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


