# 00
```
  SELECT m.pizza_name, m.price, p.name AS pizzeria_name, pv.visit_date
  FROM menu m
  JOIN person_visits pv ON m.pizzeria_id = pv.pizzeria_id
  JOIN pizzeria p ON m.pizzeria_id = p.id
  JOIN person pe ON pv.person_id = pe.id
  WHERE pe.name = 'Kate' AND m.price BETWEEN 800 AND 1000
  ORDER BY m.pizza_name, m.price, p.name, pv.visit_date;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/38d5b65d-a7e2-4503-81e6-babeeaa2326d)



# 01
```
  SELECT menu.id AS menu_id
  FROM menu
  LEFT JOIN person_order ON menu.id = person_order.menu_id
  WHERE person_order.id IS NULL
  ORDER BY menu.id;
```
![image](https://github.com/Vzoreal123/sql/assets/113076179/3a94a031-e860-4b22-9802-19095778d253)



# 08
```
  WITH DominosID AS (
      SELECT
          id
      FROM
          pizzeria
      WHERE
          name = 'Dominos'
      LIMIT 1
  )
  
  INSERT INTO menu (id, pizza_name, price, pizzeria_id)
  SELECT
      (SELECT COALESCE(MAX(id), 0) + 1 FROM menu), -- Формула для вычисления нового идентификатора
      'sicilian pizza',
      900,
      DominosID.id
  FROM
      DominosID;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/bf54fed4-3fa9-4e4b-a2f0-b9eeb5645207)




# 11
```
  UPDATE menu
  SET price = price - (price * 0.1)
  WHERE pizza_name = 'greek pizza';
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/08a84a70-ff77-4dd6-9db1-70344b090d7a)



# 13
```
  DELETE FROM person_orders
  WHERE order_date = '2022-02-25';
  
  DELETE FROM menu
  WHERE pizza_name = 'greek pizza';
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/d849dbff-b058-4c2c-b74c-b68a4a3a51be)

