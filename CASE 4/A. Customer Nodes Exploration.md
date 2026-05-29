## Q1
```sql
SELECT COUNT( DISTINCT node_id )
FROM Customer_Nodes
```
<img width="235" height="92" alt="image" src="https://github.com/user-attachments/assets/5fa9544a-d443-49e5-a924-d6ce5b323917" />

## Q2
```sql
SELECT region_name,COUNT(node_id )
FROM Customer_Nodes JOIN Regions 
ON Customer_Nodes.region_id = Regions.region_id
GROUP BY region_name
```
<img width="1048" height="272" alt="image" src="https://github.com/user-attachments/assets/4cf95776-edd2-4226-b317-07f369c8b20e" />

## Q3
```sql
SELECT region_name, COUNT( DISTINCT customer_id)
FROM Customer_Nodes JOIN Regions 
ON Customer_Nodes.region_id = Regions.region_id
GROUP BY region_name
```
<img width="1091" height="274" alt="Screenshot 2026-05-29 at 5 16 58 PM" src="https://github.com/user-attachments/assets/23d200d5-e056-41d2-9a77-027897ded0e8" />

