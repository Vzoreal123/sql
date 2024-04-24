# 570
```
SELECT e.name
FROM Employee e
JOIN (
    SELECT managerId, COUNT(*) AS direct_reports
    FROM Employee
    GROUP BY managerId
) AS report_counts
ON e.id = report_counts.managerId
WHERE report_counts.direct_reports >= 5;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/81dc403b-0b11-4c61-82e4-a74d56a4f5d2)


# 1934
```
SELECT 
    s.user_id,
    ROUND(
        IFNULL(
            SUM(CASE WHEN c.action = 'confirmed' THEN 1 ELSE 0 END) / NULLIF(COUNT(c.user_id), 0),
            0
        ),
        2
    ) AS confirmation_rate
FROM Signups s
LEFT JOIN Confirmations c ON s.user_id = c.user_id
GROUP BY s.user_id;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/00263351-6e55-440d-a2d2-d024e9161890)


# 1193
```
SELECT
    DATE_FORMAT(trans_date, '%Y-%m') AS month,
    country,
    COUNT(*) AS trans_count,
    SUM(CASE WHEN state = 'approved' THEN 1 ELSE 0 END) AS approved_count,
    SUM(amount) AS trans_total_amount,
    SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) AS approved_total_amount
FROM Transactions
GROUP BY month, country;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/a7ab0272-c1be-49fe-ad07-bf58d8387056)




# 1174
```
SELECT 
    ROUND(
        (SUM(CASE WHEN order_date = customer_pref_delivery_date THEN 1 ELSE 0 END) / COUNT(DISTINCT customer_id)) * 100,
        2
    ) AS immediate_percentage
FROM Delivery
WHERE (customer_id, order_date) IN (
    SELECT customer_id, MIN(order_date)
    FROM Delivery
    GROUP BY customer_id
);
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/70b13aca-266a-4aa6-8c77-3808e32e3fea)




# 602
```
SELECT id, num
FROM (
    SELECT id, num, 
           DENSE_RANK() OVER (ORDER BY num DESC) AS ranking
    FROM (
        SELECT id, COUNT(*) AS num
        FROM (
            SELECT requester_id AS id FROM RequestAccepted
            UNION ALL
            SELECT accepter_id FROM RequestAccepted
        ) AS all_ids
        GROUP BY id
    ) AS friend_counts
) AS ranked_friends
WHERE ranking = 1;

```

![image](https://github.com/Vzoreal123/sql/assets/113076179/593ab402-1401-4eda-ab6b-e6b3e04f4da6)


# 585
```
SELECT 
    ROUND(SUM(tiv_2016), 2) AS tiv_2016
FROM 
    Insurance
WHERE 
    tiv_2015 IN (
        SELECT 
            tiv_2015
        FROM 
            Insurance
        GROUP BY 
            tiv_2015
        HAVING 
            COUNT(*) > 1
    )
    AND (lat, lon) IN (
        SELECT 
            lat, lon
        FROM 
            Insurance
        GROUP BY 
            lat, lon
        HAVING 
            COUNT(*) = 1
    );

```

![image](https://github.com/Vzoreal123/sql/assets/113076179/4b6b1455-868e-4418-8ced-73fc78805723)






# 176
```
SELECT
    IFNULL(
        (SELECT DISTINCT Salary
         FROM Employee
         ORDER BY Salary DESC
         LIMIT 1 OFFSET 1),
        NULL
    ) AS SecondHighestSalary;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/30b9396d-6499-4b20-924c-f4ed00930be0)



# 1907
```
SELECT
    all_categories.category,
    COALESCE(COUNT(categorized_accounts.account_id), 0) AS accounts_count
FROM (
    SELECT
        account_id,
        CASE
            WHEN income < 20000 THEN 'Low Salary'
            WHEN income BETWEEN 20000 AND 50000 THEN 'Average Salary'
            ELSE 'High Salary'
        END AS category
    FROM
        Accounts
) AS categorized_accounts
RIGHT JOIN (
    SELECT 'Low Salary' AS category
    UNION ALL
    SELECT 'Average Salary' AS category
    UNION ALL
    SELECT 'High Salary' AS category
) AS all_categories
ON categorized_accounts.category = all_categories.category
GROUP BY
    all_categories.category;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/4da2d1c3-9f76-4d1e-b85b-ecb6b56830b6)




# 1204
```
SELECT
    person_name
FROM (
    SELECT
        person_name,
        SUM(weight) OVER (ORDER BY turn) AS total_weight,
        turn
    FROM
        Queue
) AS cumulative_weight
WHERE
    total_weight <= 1000
ORDER BY
    turn DESC
LIMIT 1;

```

![image](https://github.com/Vzoreal123/sql/assets/113076179/bef3a448-02bf-4dc6-a72b-38e764f11d1f)


# 1164
```
SELECT 
    product_id,
    COALESCE(
        (SELECT new_price 
         FROM Products 
         WHERE product_id = p.product_id AND change_date <= '2019-08-16'
         ORDER BY change_date DESC 
         LIMIT 1),
        10) AS price
FROM 
    (SELECT DISTINCT product_id FROM Products) AS p;

```

![image](https://github.com/Vzoreal123/sql/assets/113076179/06cb330a-9e36-4e1c-964a-7c6ff5ec6279)




# 180
```
SELECT DISTINCT num AS ConsecutiveNums
FROM (
    SELECT 
        num,
        LAG(num) OVER (ORDER BY id) AS prev_num,
        LEAD(num) OVER (ORDER BY id) AS next_num
    FROM Logs
) AS t
WHERE num = prev_num AND num = next_num;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/d40092e0-d5e6-43aa-aa1b-e0715c533e3b)




# 1045
```
SELECT customer_id
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (
    SELECT COUNT(*) FROM Product
);

```

![image](https://github.com/Vzoreal123/sql/assets/113076179/df04fd7e-04d3-4157-ab6f-e9b093754f82)




# 550
```
SELECT 
    ROUND(COUNT(DISTINCT player_id) / (SELECT COUNT(DISTINCT player_id) FROM Activity), 2) AS fraction
FROM 
    Activity AS a1
WHERE 
    EXISTS (
        SELECT 1
        FROM Activity AS a2
        WHERE a1.player_id = a2.player_id
            AND a2.event_date = DATE_ADD(a1.event_date, INTERVAL 1 DAY)
    );
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/cb12932c-00f5-4962-bee2-5f328293089f)






