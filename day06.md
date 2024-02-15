# 00
```
CREATE TABLE IF NOT EXISTS person_discounts (
    id BIGINT PRIMARY KEY,
    person_id BIGINT,
    pizzeria_id BIGINT,
    discount NUMERIC,
    CONSTRAINT fk_person_discounts_person_id FOREIGN KEY (person_id) REFERENCES person (id),
    CONSTRAINT fk_person_discounts_pizzeria_id FOREIGN KEY (pizzeria_id) REFERENCES pizzeria (id)
);
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/9e491a76-74f1-404e-bf6d-2311ac20b864)


# 01
```
CREATE TABLE IF NOT EXISTS person_discounts (
    id BIGINT PRIMARY KEY,
    person_id BIGINT,
    pizzeria_id BIGINT,
    discount NUMERIC,
    CONSTRAINT fk_person_discounts_person_id FOREIGN KEY (person_id) REFERENCES person (id),
    CONSTRAINT fk_person_discounts_pizzeria_id FOREIGN KEY (pizzeria_id) REFERENCES pizzeria (id)
);
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/9e491a76-74f1-404e-bf6d-2311ac20b864)



# 02
```
SELECT 
    p.name AS name,
    m.pizza_name AS pizza_name,
    m.price AS price,
    COALESCE(ROUND(m.price * (1 - pd.discount / 100), 0), m.price) AS discount_price,
    pz.name AS pizzeria_name
FROM 
    person_order po
JOIN 
    person p ON po.person_id = p.id
JOIN 
    menu m ON po.menu_id = m.id
JOIN 
    pizzeria pz ON m.pizzeria_id = pz.id
LEFT JOIN 
    person_discounts pd ON po.person_id = pd.person_id AND m.pizzeria_id = pd.pizzeria_id
ORDER BY 
    p.name, m.pizza_name;
);
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/4e416c90-efa5-42db-90ff-1cd491af3888)



# 03
```
CREATE UNIQUE INDEX idx_person_discounts_unique ON person_discounts (person_id, pizzeria_id);

EXPLAIN ANALYZE SELECT * FROM person_discounts WHERE person_id = 1 AND pizzeria_id = 1;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/823c0596-a00f-40ea-93ff-d9cc017c3602)


# 04
```
ALTER TABLE person_discounts
ADD CONSTRAINT ch_nn_person_id CHECK (person_id IS NOT NULL);

ALTER TABLE person_discounts
ADD CONSTRAINT ch_nn_pizzeria_id CHECK (pizzeria_id IS NOT NULL);

ALTER TABLE person_discounts
ADD CONSTRAINT ch_nn_discount CHECK (discount IS NOT NULL);

ALTER TABLE person_discounts
ALTER COLUMN discount SET DEFAULT 0;

ALTER TABLE person_discounts
ADD CONSTRAINT ch_range_discount CHECK (discount >= 0 AND discount <= 100);
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/0d778015-5316-45ec-b3fd-4c976c072ebb)




# 05
```
COMMENT ON TABLE person_discounts IS 'This table stores information about personal discounts for customers at different pizzeria restaurants.';

COMMENT ON COLUMN person_discounts.person_id IS 'The unique identifier of the person who receives the discount.';

COMMENT ON COLUMN person_discounts.pizzeria_id IS 'The unique identifier of the pizzeria restaurant where the discount is applicable.';

COMMENT ON COLUMN person_discounts.discount IS 'The percentage value of the discount applied to the customer''s order.';
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/fdeb5ad4-a209-4808-9c86-baa96b777c8a)


# 06
```
DO $$
DECLARE
    max_id bigint;
    sql_text text;
BEGIN
    SELECT MAX(id) INTO max_id FROM person_discounts;
    
    IF max_id IS NOT NULL THEN
        sql_text := 'ALTER SEQUENCE seq_person_discounts RESTART WITH ' || (max_id + 1);
        EXECUTE sql_text;
    END IF;
END $$;
```

![image](https://github.com/Vzoreal123/sql/assets/113076179/cce7046c-ba06-4919-b798-646d6bed455f)
