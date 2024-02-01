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




# 02
```
  SELECT object_name
  FROM (
      SELECT name AS object_name, 'person' AS source FROM person
      UNION ALL
      SELECT pizza_name, 'menu' FROM menu
  ) AS combined_data
  ORDER BY
      CASE
          WHEN source = 'person' THEN 1
          ELSE 2
      END,
      object_name;
```
![image](https://github.com/Vzoreal123/sql/assets/113076179/972e9c94-1306-4c7d-8de0-222d93312713)


# 03
```
  SELECT pizza_name
  FROM menu m1
  WHERE NOT EXISTS (
      SELECT 1
      FROM menu m2
      WHERE m2.pizza_name = m1.pizza_name AND m2.id < m1.id
  )
  ORDER BY pizza_name DESC;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/c2194b0c-5ff9-433f-ae31-e63c10080764)


# 04
```
  SELECT
      po.order_date AS action_date,
      po.person_id
  FROM
      person_order po
  WHERE
      po.person_id IN (
          SELECT pv.person_id
          FROM person_visits pv
          WHERE pv.visit_date = po.order_date
      )
  ORDER BY
      action_date ASC,
      person_id DESC;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/286443c3-f4d1-4c27-8f55-c29c7c4de3d9)


# 05
```
  SELECT
      person.id AS person_id,
      person.name AS person_name,
      person.age,
      person.gender,
      person.address,
      pizzeria.id AS pizzeria_id,
      pizzeria.name AS pizzeria_name,
      pizzeria.rating
  FROM
      person, pizzeria
  ORDER BY
      person.id,
      pizzeria.id;
```
![image](https://github.com/Vzoreal123/sql/assets/113076179/d016c157-4cc3-443f-b77f-9d733944ca0b)



# 06
```
  SELECT po.order_date AS action_date, p.name AS person_name
  FROM person_order po
  JOIN person p ON po.person_id = p.id
  WHERE po.person_id IN (
      SELECT pv.person_id
      FROM person_visits pv
      WHERE pv.visit_date = po.order_date
  )
  ORDER BY action_date ASC, person_name DESC;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/67bc47f9-616e-470b-b985-e743ec775452)




# 07
```
  SELECT
      po.order_date,
      format('%s (age:%s)', p.name, p.age) AS person_information
  FROM
      person_order po
  JOIN
      person p ON po.person_id = p.id
  ORDER BY
      po.order_date ASC,
      person_information ASC;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/71685963-0408-4313-9391-d158c7040e40)





# 08
```
  SELECT
      order_date AS action_date,
      person_id
  FROM
      person_order NATURAL JOIN person_visits
  ORDER BY
      action_date ASC,
      person_id DESC;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/fa6192d1-dba5-4ef6-859c-9839e410ffb2)



# 09 IN
```
  SELECT name
  FROM pizzeria
  WHERE id NOT IN (
      SELECT DISTINCT pizzeria_id
      FROM person_visits
  );
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/51b514ea-6dca-4008-af67-2b94a3c2fe93)


# 09 EXISTS
```
  SELECT name
  FROM pizzeria p
  WHERE NOT EXISTS (
      SELECT 1
      FROM person_visits pv
      WHERE pv.pizzeria_id = p.id
  );
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/2663924a-e118-4b6f-b9a0-0dfd0f4fede2)


# 10
```
  SELECT
      p.name AS person_name,
      m.pizza_name,
      pz.name AS pizzeria_name
  FROM
      person p
  JOIN
      person_order po ON p.id = po.person_id
  JOIN
      menu m ON po.menu_id = m.id
  JOIN
      pizzeria pz ON m.pizzeria_id = pz.id
  ORDER BY
      p.name ASC,
      m.pizza_name ASC,
      pz.name ASC;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/cf310e7c-cfd4-4308-8a69-f33dcb881ee6)
