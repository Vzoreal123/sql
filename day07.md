# 00
```
SELECT person_id, COUNT(*) AS count_of_visits
FROM person_visits
GROUP BY person_id
ORDER BY count_of_visits DESC, person_id ASC;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/55153edf-514c-4a14-8155-7b4d3e84b3e0)

# 01
```
SELECT p.name, COUNT(*) AS count_of_visits
FROM person_visits pv
JOIN person p ON pv.person_id = p.id
GROUP BY p.name
ORDER BY count_of_visits DESC, p.name ASC
LIMIT 4;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/c8d0c8e7-13b6-4e92-9ecc-edbdff176df1)


# 02
```
(
    SELECT pizzeria.name, COUNT(*) AS count, 'order' AS action_type
    FROM person_order po
    JOIN menu ON po.menu_id = menu.id
    JOIN pizzeria ON menu.pizzeria_id = pizzeria.id
    GROUP BY pizzeria.name
)
UNION ALL
(
    SELECT pizzeria.name, COUNT(*) AS count, 'visit' AS action_type
    FROM person_visits pv
    JOIN pizzeria ON pv.pizzeria_id = pizzeria.id
    GROUP BY pizzeria.name
)
ORDER BY action_type ASC, count DESC;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/ee3e54d8-4470-491c-b6ed-ad03596320e9)


# 03
```
SELECT COALESCE(visits.name, orders.name) AS name, 
       COALESCE(visits.total_count, 0) + COALESCE(orders.total_count, 0) AS total_count
FROM
(
    SELECT pizzeria.name, COUNT(*) AS total_count
    FROM person_visits pv
    JOIN pizzeria ON pv.pizzeria_id = pizzeria.id
    GROUP BY pizzeria.name
) AS visits
FULL JOIN
(
    SELECT pizzeria.name, COUNT(*) AS total_count
    FROM person_order po
    JOIN menu ON po.menu_id = menu.id
    JOIN pizzeria ON menu.pizzeria_id = pizzeria.id
    GROUP BY pizzeria.name
) AS orders
ON visits.name = orders.name
ORDER BY total_count DESC, name ASC;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/97cd9090-c6c1-42d4-8b7d-885cf40f0440)



# 05
```
SELECT DISTINCT p.name 
FROM person p
JOIN person_order pv ON pv.person_id = p.id
ORDER BY p.name;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/3a672b98-a861-403d-971d-437bd03e6bdb)


# 07
```
SELECT round(AVG(rating), 4) AS global_rate 
FROM pizzeria
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/6afdeb73-eb09-4630-8537-cf761c240675)


# 08
```
SELECT address, pi.name, COUNT(po.order_date) 
FROM person p
JOIN pizzeria pi ON pi.id = p.id
JOIN person_order po ON po.person_id = p.id 
GROUP BY p.address, pi.name
ORDER BY p.address;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/d8cb0fe6-709a-4030-9924-6dc649f0c5fd)


# 09
```
WITH formula_average AS (
   SELECT 
       p.address, 
       (MAX(p.age) - (MIN(p.age) / NULLIF(MAX(p.age), 0)))::NUMERIC AS formula, 
       ROUND(AVG(p.age), 2)::NUMERIC AS average 
   FROM 
       person p
   GROUP BY 
       p.address
)
SELECT 
    fa.address, 
    fa.formula, 
    fa.average, 
    (fa.formula > fa.average) AS comparison 
FROM 
    formula_average fa;

```

![image](https://github.com/Vzoreal123/sql/assets/113076179/7d8a5660-e123-4056-b66a-accf10e1ecec)






