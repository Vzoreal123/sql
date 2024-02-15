# 00
```
CREATE INDEX idx_menu_pizzeria_id ON public.menu (pizzeria_id);

CREATE INDEX idx_person_visits_person_id ON public.person_visits (person_id);

```

![image](https://github.com/Vzoreal123/sql/assets/113076179/5784b99e-9a20-4f35-b0e9-7ad38f589e96)


# 01
```
SELECT m.pizza_name, p.name AS pizzeria_name
FROM menu m
JOIN pizzeria p ON m.pizzeria_id = p.id;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/801fc017-5afb-446e-86f0-b2b6f3d18598)


# 02
```
CREATE INDEX idx_person_name ON person (UPPER(name));

EXPLAIN ANALYZE
SELECT *
FROM person
WHERE UPPER(name) = 'JOHN';

```

![image](https://github.com/Vzoreal123/sql/assets/113076179/20f0c719-8fcb-4228-9e88-98cffb6b23b7)


# 03
```
CREATE INDEX idx_person_order_multi ON person_order (person_id, menu_id, order_date);

EXPLAIN ANALYZE
SELECT person_id, menu_id, order_date
FROM person_order
WHERE person_id = 8 AND menu_id = 19;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/2a85cfd9-e4dd-43be-976d-f79764dd066c)



# 04
```
CREATE UNIQUE INDEX idx_menu_unique ON menu (pizzeria_id, pizza_name);

EXPLAIN ANALYZE
SELECT pizzeria_id, pizza_name
FROM menu;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/9fb3f504-3fc5-4a49-b10f-3799d9355d9a)



# 05
```
CREATE UNIQUE INDEX idx_person_order_order_date ON person_order (person_id, menu_id, order_date)
WHERE order_date = '2022-01-01';

EXPLAIN ANALYZE
SELECT person_id, menu_id, order_date
FROM person_order
WHERE order_date = '2022-01-01';
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/9a80e017-e6ed-4515-ae39-26c10bd7f4d3)


# 06
```
DROP INDEX IF EXISTS idx_1;

CREATE INDEX idx_1 ON menu (pizzeria_id, pizza_name);

EXPLAIN ANALYZE
SELECT
    m.pizza_name AS pizza_name,
    max(rating) OVER (PARTITION BY rating ORDER BY rating ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS k
FROM  menu m
INNER JOIN pizzeria pz ON m.pizzeria_id = pz.id
ORDER BY 1,2;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/1e529739-1fd2-4b00-8a8a-04b6cff8f581)

![image](https://github.com/Vzoreal123/sql/assets/113076179/05f2cf39-a328-4b11-97d6-5ce946972c8e)


