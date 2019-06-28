## Create tables

#### `Product`

| Field | Type |
| ----- | ---- |
| sku | str |
| status_id | int |
| name | str |
| taxable | bool |
| price | int |
| created_at | datetime |

#### `ProductStatus`

| Field | Type |
| ----- | ---- |
| name | str |

```sql
CREATE TABLE productstatus (
	id serial primary key,
	name varchar(10)		
);

CREATE TABLE product (
	id serial primary key,
	sku varchar(10),		
	status_id integer references productstatus(id),
	name varchar(10),		
	tax BOOLEAN,			
	price integer,			
	created_at timestamp	
);
```

## Queries

#### 1. Добавить запись в таблицу `Product`

```sql
INSERT INTO product(sku, status_id, name, tax, price, created_at) 
VALUES (10, 1, 'fish', TRUE, 5, '2019-06-26 13:15:20');
```

#### 2. Добавить запись в таблицу `ProductStatus`

```sql
INSERT INTO productstatus(name) VALUES ('confirmed');
```

#### 3. Добавить 4 записи в таблицу `ProductStatus`

```sql
INSERT INTO productstatus(name) VALUES 
    ('confirmed'), 
    ('placed'), 
    ('delivered'), 
    ('paid');
```

#### 4. Добавить 500 записей в таблицу `Product` (название, текст не важен)

```sql
DO $$
declare id integer :=2;
BEGIN
	WHILE id > 0 AND id < 500 LOOP	
		INSERT INTO product(sku, status_id, name, tax, price, created_at)  
		values (10, 3, 'Dima', True, 10, current_timestamp);
		id = id + 1;
	END LOOP;
END $$;
```

#### 5. Обновить название одной из записей в таблице `ProductStatus`

```sql
UPDATE productstatus
SET name = 'available'
WHERE
	id = 1;
```

#### 6. Обновить значение `taxable` в первых 10 записях таблицы `Product`

```sql
UPDATE product
SET tax = true
WHERE id = any (
	array(SELECT id 
		  FROM product ORDER BY id 
		  LIMIT 10));
```

#### 7. Удалить запись из таблицы `Product`

```sql
DELETE FROM product
WHERE id = any (array(
	SELECT id 
	FROM product 
	ORDER BY id 
	LIMIT 10));
```

#### 8. Удалить 100 последних записей из таблицы `Product`

```sql
delete from product
where id in (
	select id 
	from product
	order by id DESC 
	limit 100
)
```

#### 9. Удалить 1 запись из таблицы `ProductStatus`

```sql
DELETE FROM productstatus
WHERE id = 1;
```

#### 10. Выбрать все записи из таблицы `Product`, у которых `taxable` верно

```sql
SELECT *
FROM product
WHERE tax = true;
```

#### 11. Выбрать все записи из таблицы `Product` с включением имени соответствующего статуса

```sql
SELECT 
p.sku, 
p.status_id, 
p.name, 
p.tax, 
p.price, 
p.created_at, 
i.name 
FROM 
product p 
LEFT JOIN productstatus i ON 
p.status_id = i.id;
```

#### 12. Выбрать все записи из таблицы `Product` с включением соответствующего статуса, у которых `taxable = true`

```sql
SELECT 	
		p.id,
		p.sku, 
		p.status_id, 
		p.name, 
		p.tax, 
		p.price, 
		p.created_at, 
		i.name 
FROM product p 
LEFT JOIN productstatus i 
ON p.status_id = i.id
WHERE tax = true
```

#### 13. Выбрать все записи из таблицы `ProductStatus`, у которых нет соответствия в таблице `Product`

```sql
SELECT COUNT(p.name) AS Status_Count,
		i.name,
		i.id
FROM productstatus i 
LEFT JOIN product p 
ON i.id = p.status_id 
WHERE p.name is null
GROUP BY i.id
```

#### 14. Вывести количество записей в таблице Product для каждой записи в таблице ProductStatus. Отсортировать записи по имени статуса в порядке возрастания

```sql
SELECT COUNT(p.name) AS Status_Count, i.name 
FROM productstatus i 
LEFT JOIN product p 
ON i.id = p.status_id 
GROUP BY i.name 
ORDER BY i.name ASC 
```

#### 15. Вывести количество записей в таблице Product, у которых taxable = true, для каждой записи в таблице ProductStatus

```sql
SELECT COUNT(p.tax) AS Count_tax, 
i.name
FROM productstatus3 i 
LEFT JOIN product3 p 
ON i.id = p.status_id 
WHERE tax = true
GROUP BY i.name
```

#### 16. Вывести количество записей в таблице `Product`, у которых `taxable` верно и нет

```sql
SELECT COUNT( 1 ),
		p.tax
FROM product p
WHERE tax = true or tax = false
GROUP BY p.tax
```

#### 17. Вывести количество записей в таблице `Product`, для каждой записи в таблице `ProductStatus`, у которых `taxable` верно и нет. Отсортировать записи по имени статуса.

```sql
SELECT COUNT(p.tax) AS Status_tax, 
p.tax, 
i.name 
FROM productstatus i 
LEFT JOIN product p 
ON i.id = p.status_id 
WHERE tax = true or tax = false 
GROUP BY i.name, p.tax 
ORDER BY p.tax
```
