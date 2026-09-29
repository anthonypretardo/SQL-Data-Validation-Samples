# SQL Data Validation Samples

Real-world SQL queries used for enterprise data validation and ETL testing.
Based on 14+ years of QA experience including Oracle Philippines (RGBU).

---

## 1. Row Count Validation (Source vs Target)
Use this to verify no records are lost during ETL migration.
```sql
-- Compare row counts between source and target tables
SELECT 
  (SELECT COUNT(*) FROM source_db.orders) AS source_count,
  (SELECT COUNT(*) FROM target_db.orders) AS target_count,
  (SELECT COUNT(*) FROM source_db.orders) 
   - (SELECT COUNT(*) FROM target_db.orders) AS difference;
```

## 2. Duplicate Record Check
Catch duplicate primary keys after data migration.
```sql
SELECT order_id, COUNT(*) AS duplicate_count
FROM target_db.orders
GROUP BY order_id
HAVING COUNT(*) > 1
ORDER BY duplicate_count DESC;
```

## 3. Null Value Audit
Validate required fields are not null after transformation.
```sql
SELECT 
  COUNT(*) AS total_records,
  SUM(CASE WHEN customer_id IS NULL THEN 1 ELSE 0 END) AS null_customer_id,
  SUM(CASE WHEN order_date IS NULL THEN 1 ELSE 0 END) AS null_order_date,
  SUM(CASE WHEN total_amount IS NULL THEN 1 ELSE 0 END) AS null_total_amount
FROM target_db.orders;
```

## 4. Data Type & Format Validation
Ensure dates and amounts are correctly transformed.
```sql
SELECT order_id, order_date, total_amount
FROM target_db.orders
WHERE order_date NOT BETWEEN '2020-01-01' AND SYSDATE
   OR total_amount < 0
   OR total_amount IS NULL;
```

## 5. Referential Integrity Check
Verify foreign key relationships are intact after migration.
```sql
SELECT o.order_id, o.customer_id
FROM target_db.orders o
LEFT JOIN target_db.customers c ON o.customer_id = c.customer_id
WHERE c.customer_id IS NULL;
```

---

## Tools Used
- Oracle SQL, PostgreSQL, MS SQL Server
- Used in CI/CD pipelines via GitLab CI and Jenkins
- Integrated with Python scripts for automated validation reporting

## Author
Anthony Retardo — Senior QA Engineer | Oracle Philippines (RGBU)  
[LinkedIn](https://www.linkedin.com/in/anthonypretardo/)
