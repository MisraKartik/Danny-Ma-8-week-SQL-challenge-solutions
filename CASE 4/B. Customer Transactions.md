## Q1
```sql
SELECT txn_type, COUNT(customer_id), SUM(txn_amount)
FROM Customer_Transactions
GROUP BY txn_type
```
<img width="1033" height="178" alt="Screenshot 2026-05-29 at 6 05 08 PM" src="https://github.com/user-attachments/assets/b36ed6f6-7f9f-4f29-9748-f3f9fdd0515c" />

## Q2
```sql
 WITH q2 AS (
 SELECT COUNT(txn_type),SUM(txn_amount)
 FROM Customer_Transactions
 WHERE txn_type = 'deposit'
 GROUP BY customer_id
)
 SELECT ROUND(AVG(count)), ROUND(AVG(sum),2)
 FROM q2
```
<img width="863" height="97" alt="Screenshot 2026-05-29 at 6 14 27 PM" src="https://github.com/user-attachments/assets/c42c8422-625e-4735-ab41-86f968bac650" />

## Q3
```sql
WITH txn AS ( 
 SELECT TO_CHAR(txn_date,'FMMonth') AS month,
  customer_id,
  CASE WHEN txn_type = 'deposit' THEN 1 ELSE 0 END AS deposit,
  CASE WHEN txn_type = 'purchase' THEN 1 ELSE 0 END AS purchase,
  CASE WHEN txn_type = 'withdrawal' THEN 1 ELSE 0 END AS withdrawal
  FROM Customer_Transactions
),
total_txn AS
(
  SELECT month, customer_id,SUM(deposit) AS deposit,
  SUM(purchase) AS purchase,SUM(withdrawal) AS withdrawal
  FROM txn
  GROUP BY customer_id,month
)
SELECT month, COUNT( customer_id)
FROM total_txn
WHERE deposit > 1 AND (purchase = 1 OR withdrawal = 1)
GROUP BY month
```
<img width="1016" height="222" alt="Screenshot 2026-05-29 at 6 45 47 PM" src="https://github.com/user-attachments/assets/5f453287-3082-4a52-8c92-02f687fc2860" />

