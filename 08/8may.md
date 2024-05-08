#SQL

## Task 1

```sql
  SELECT
      u.name,
      r.date,
      r.totalsum,
      GROUP_CONCAT(s.title) AS service_all
  FROM
      users AS u
  JOIN
      records AS r ON u.id = r.user_id
  JOIN
      services_records AS sr ON r.id = sr.records_id
  JOIN
      services AS s ON sr.service_id = s.id
  GROUP BY
      u.name,
      r.date,
      r.totalsum;
```





## Task 2

```sql
  BEGIN TRANSACTION;
  
  INSERT INTO records (user_id, date, totalsum)
  VALUES (
      @user_id,
      @date,
      @totalsum
  );
  
  SET @records_id = LAST_INSERT_ID();
  
  INSERT INTO services_records (service_id, records_id)
  VALUES
      @services_list;
  
  COMMIT;
```
