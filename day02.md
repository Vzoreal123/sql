# 00
```
  SELECT
      p.name AS pizzeria_name,
      p.rating
  FROM
      pizzeria p
  LEFT JOIN
      person_visits pv ON p.id = pv.pizzeria_id
  WHERE
      pv.pizzeria_id IS NULL
  ORDER BY
      p.name ASC;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/df50cdf4-0271-412b-9f16-03a2d36d9545)



# 01
```
  SELECT
      generate_series('2022-01-01'::date, '2022-01-10'::date, '1 day')::date AS missing_date
  EXCEPT
  SELECT
      DISTINCT visit_date
  FROM
      person_visits
  WHERE
      person_id IN (1, 2)
  ORDER BY
      missing_date ASC;

```
![image](https://github.com/Vzoreal123/sql/assets/113076179/3cf38ada-5bbb-47e0-94c2-eac96659094e)




# 02
```
  SELECT
      COALESCE(p.name, '-') AS person_name,
      pv.visit_date,
      COALESCE(pz.name, '-') AS pizzeria_name
  FROM
      (SELECT DISTINCT id, name FROM person) p
  FULL OUTER JOIN
      person_visits pv ON p.id = pv.person_id
  FULL OUTER JOIN
      (SELECT DISTINCT id, name FROM pizzeria) pz ON pv.pizzeria_id = pz.id
  WHERE
      pv.visit_date BETWEEN '2022-01-01' AND '2022-01-03'
  ORDER BY
      person_name ASC,
      visit_date ASC,
      pizzeria_name ASC;

```
![image](https://github.com/Vzoreal123/sql/assets/113076179/1b6716b6-9ed3-4653-8688-f828ffd91153)



# 04
```
  SELECT
      m.pizza_name,
      p.name AS pizzeria_name,
      m.price
  FROM
      menu m
  JOIN
      pizzeria p ON m.pizzeria_id = p.id
  WHERE
      m.pizza_name IN ('mushroom pizza', 'pepperoni pizza')
  ORDER BY
      m.pizza_name ASC,
      p.name ASC;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/6577cf1d-463b-4340-9a59-f1c46c860a99)


# 05
```
  SELECT
      name
  FROM
      person
  WHERE
      gender = 'female' AND age > 25
  ORDER BY
      name ASC;
```
![image](https://github.com/Vzoreal123/sql/assets/113076179/cb2b08c6-07c2-4f35-a7d9-84a9abf874a9)



# 07
```
  SELECT DISTINCT pv.pizzeria_id, m.pizza_name
  FROM person_visits pv
  JOIN menu m ON pv.pizzeria_id = m.pizzeria_id
  JOIN person_order po ON pv.person_id = po.person_id AND pv.pizzeria_id = pv.pizzeria_id
  WHERE po.order_date = '2022-01-08' AND m.price < 800;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/025b4476-4577-4f01-a83d-6b21537f73b5)



# 08
```
  SELECT DISTINCT p.name
  FROM person p
  JOIN person_order po ON p.id = po.person_id
  JOIN menu m ON po.menu_id = m.id
  WHERE (p.gender = 'male') 
    AND (p.address IN ('Moscow', 'Samara')) 
    AND (m.pizza_name IN ('pepperoni pizza', 'mushroom pizza'))
  ORDER BY p.name DESC;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/708524c8-7996-416d-bdd8-1d92c2e12e69)



# 09 
```
  SELECT DISTINCT p.name
  FROM person p
  JOIN person_order po ON p.id = po.person_id
  JOIN menu m ON po.menu_id = m.id
  WHERE p.gender = 'female'
    AND m.pizza_name IN ('pepperoni pizza', 'cheese pizza')
  GROUP BY p.id
  HAVING COUNT(DISTINCT m.pizza_name) = 2
  ORDER BY p.name;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/f0cb8d40-04b6-46bf-965a-7661dffbf59f)




# 10
```
  SELECT p1.name AS person_name1, p2.name AS person_name2, p1.address AS common_address
  FROM person p1
  JOIN person p2 ON p1.address = p2.address AND p1.id < p2.id
  ORDER BY p1.name, p2.name, p1.address;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/498491a5-d4b2-4180-9e74-ef555ed3cd3e)
