  # Zadanie 1
   
  ```
      SELECT name, age, address FROM person
      WHERE address = 'Kazan';
  ```
![image](https://github.com/Vzoreal123/sql/assets/113076179/51ed1858-a992-4e3a-a74e-2c5e95da4374)


 # Zadanie 2
  ``` 
    SELECT name, age, address FROM person
    WHERE gender = 'female' and address = 'Kazan'
    ORDER BY name;
  ```
![image](https://github.com/Vzoreal123/sql/assets/113076179/24ebc4f9-d1d3-4223-9ebc-267bda427dda)


 # Zadanie 3
  ``` 
    SELECT name, rating FROM pizzeria
    WHERE rating >= 3.5 and rating <= 5
    ORDER BY rating;
  ```

![image](https://github.com/Vzoreal123/sql/assets/113076179/77a8ef74-06f6-44ad-8ba4-7a0d36389e79)


 # Zadanie 4
  ``` 
    SELECT DISTINCT person_id FROM person_visits
    WHERE visit_date >= '2022-01-05' or pizzeria_id = 2
    ORDER BY person_id DESC;
  ```
![image](https://github.com/Vzoreal123/sql/assets/113076179/0d6068f8-e7dd-486d-977a-ede9f11c9887)


 # Zadanie 5
  ``` 
    SELECT name FROM person
    WHERE id IN(SELECT person_id FROM person_order WHERE order_date ='2022-01-01' or order_date = '2022-01-07' or order_date = '2022-01-07');
  ```
![image](https://github.com/Vzoreal123/sql/assets/113076179/a6a438d1-d80f-4585-b5e4-9d21e2e393e9)


 # Zadanie 6
  ``` 
    SELECT true FROM person
    WHERE name = 'Anna';
  ```

![image](https://github.com/Vzoreal123/sql/assets/113076179/ac6d91c9-6abb-4dfe-8374-937c29faa413)

# 2
 # Zadanie 1
  ``` 
    SELECT
        m.id AS object_id,
        m.pizza_name AS object_name
    FROM
        menu m
    UNION ALL
    SELECT
        p.id AS object_id,
        p.name AS object_name
    FROM
        person p
    ORDER BY
        object_id,
        object_name;
  ```
![image](https://github.com/Vzoreal123/sql/assets/113076179/0e8aac75-f21e-4fae-aeaf-df0d48c3a5ad)


# Zadanie 2
  ``` 
    SELECT
        m.pizza_name AS object_name
    FROM
        menu m
    UNION ALL
    SELECT
        p.name AS object_name
    FROM
        person p
    ORDER BY
        object_name;
  ```

![image](https://github.com/Vzoreal123/sql/assets/113076179/6f22ecd6-ee28-4a34-a0f6-5fd8b6524183)


# Zadanie 3
  ``` 
    SELECT
        po.person_id,
        po.order_date AS action_date
    FROM
        person_order po
    JOIN
        person_visits pv ON po.person_id = pv.person_id AND po.order_date = pv.visit_date
    ORDER BY
        action_date ASC,
        po.person_id DESC;
  ```

![image](https://github.com/Vzoreal123/sql/assets/113076179/cae429c8-cba5-4cf8-8bc4-c7959c3e4acb)






  
