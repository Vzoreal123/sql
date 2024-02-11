# 00
```
  CREATE OR REPLACE VIEW v_persons_female AS
  SELECT *
  FROM persons
  WHERE gender = 'female';
  
  CREATE OR REPLACE VIEW v_persons_male AS
  SELECT *
  FROM persons
  WHERE gender = 'male';
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/51ab8525-1aca-418b-bb48-6a7b76c94f4c)



# 01
```
  SELECT name
  FROM v_persons_female
  UNION ALL
  SELECT name
  FROM v_persons_male
  ORDER BY name;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/2626d20a-6fa3-4fce-87f9-8e34d4aa8105)


# 02
```
  CREATE OR REPLACE VIEW v_generated_dates AS
  SELECT generate_series('2022-01-01'::date, '2022-01-31'::date, '1 day') AS generated_date;
  
  SELECT * FROM v_generated_dates;

```

![image](https://github.com/Vzoreal123/sql/assets/113076179/2061926e-c8ed-442f-9c7c-0c90483c102c)



# 03
```
  SELECT gd.generated_date AS missing_date
  FROM v_generated_dates gd
  LEFT JOIN person_visits pv ON gd.generated_date = pv.visit_date
  WHERE pv.visit_date IS NULL
  ORDER BY missing_date;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/3c6aa77f-72de-48cf-baff-2adc1efbcd76)



# 04
```
  CREATE OR REPLACE VIEW v_symmetric_union AS
  SELECT person_id
  FROM
  (
      SELECT person_id
      FROM person_visits
      WHERE visit_date = '2022-01-02'
      EXCEPT
      SELECT person_id
      FROM person_visits
      WHERE visit_date = '2022-01-06'
      
      UNION ALL
      
      SELECT person_id
      FROM person_visits
      WHERE visit_date = '2022-01-06'
      EXCEPT
      SELECT person_id
      FROM person_visits
      WHERE visit_date = '2022-01-02'
  ) AS symmetric_union
  ORDER BY person_id;
```
![image](https://github.com/Vzoreal123/sql/assets/113076179/39b673cd-2698-48f6-883f-d7d1e807b377)



# 05
```
  CREATE TEMP TABLE temp_price_with_discount (
      name VARCHAR(100),
      pizza_name VARCHAR(100),
      price NUMERIC,
      discount_price INTEGER
  );
  
  INSERT INTO temp_price_with_discount (name, pizza_name, price, discount_price)
  SELECT p.name,
         m.pizza_name,
         m.price,
         ROUND(m.price * 0.9)::integer AS discount_price
  FROM person_order po
  JOIN menu m ON po.menu_id = m.id
  JOIN persons p ON po.person_id = p.id
  ORDER BY p.name, m.pizza_name;
  
  SELECT * FROM temp_price_with_discount;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/93b2535f-81ff-4271-831e-145a89a1c3a6)



# 06
```
  SELECT COUNT(*)
  FROM person_visits pv
  JOIN person_order po ON po.person_id = pv.person_id
  JOIN menu ON po.menu_id = menu.id
  JOIN persons ON pv.person_id = persons.id
  WHERE pv.visit_date = '2022-01-08'
    AND menu.price < 800
    AND persons.name = 'Dmitriy';
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/6594bb0b-3567-40c6-806e-30d6011118f8)



# 08
```
  DROP VIEW IF EXISTS v_persons_female;
  DROP VIEW IF EXISTS v_persons_male;
  
  DROP MATERIALIZED VIEW IF EXISTS mv_dmitriy_visits_and_eats;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/8c1d5dda-4b24-4759-9775-e7351cf538aa)
