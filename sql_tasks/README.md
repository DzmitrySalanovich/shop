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
-- Create tables;
```

## Queries

#### 1. Добавить запись в таблицу `Product`

```sql
INSERT INTO product VALUES (10, 1, 'fish', TRUE, 5, '2019-06-26 13:15:20');
```

#### 2. Добавить запись в таблицу `ProductStatus`

```sql
INSERT INTO productstatus VALUES ('confirmed');
```

#### 3. Добавить 4 записи в таблицу `ProductStatus`

```sql
INSERT INTO productstatus VALUES 
    ('confirmed'), 
    ('placed'), 
    ('delivered'), 
    ('paid') 
;
```

#### 4. Добавить 500 записей в таблицу `Product` (название, текст не важен)

```sql
DO $$
declare status_id integer :=1;
BEGIN
	WHILE status_id > 0 AND status_id < 501 LOOP
		INSERT INTO product values ('gg', status_id, 'Dima', True, 10, current_timestamp);
		status_id = status_id + 1;
	END LOOP;
END $$;
```

#### 5. Обновить название одной из записей в таблице `ProductStatus`

```sql
UPDATE productstatus
SET name = 'available'
WHERE
	name = 'paid'
```

#### 6. Обновить значение `taxable` в первых 10 записях таблицы `Product`

```sql
UPDATE product
SET tax = false
WHERE status_id < 11
```

#### 7. Удалить запись из таблицы `Product`

```sql
DELETE FROM product
WHERE status_id = 1;
```

#### 8. Удалить 100 последних записей из таблицы `Product`

```sql
-- todo;
```

#### 9. Удалить 1 запись из таблицы `ProductStatus`

```sql
DELETE FROM productstatus
WHERE name = 'available';
```

#### 10. Выбрать все записи из таблицы `Product`, у которых `taxable` верно

```sql
SELECT tax
FROM product
WHERE tax = true
```

#### 11. Выбрать все записи из таблицы `Product` с включением имени соответствующего статуса

```sql
-- todo;
```

#### 12. Выбрать все записи из таблицы `Product` с включением соответствующего статуса, у которых `taxable = true`

```sql
-- todo;
```

#### 13. Выбрать все записи из таблицы `ProductStatus`, у которых нет соответствия в таблице `Product`

```sql
-- todo;
```

#### 14. Вывести количество записей в таблице `Product` для каждой записи в таблице `ProductStatus`. Отсортировать записи по имени статуса в порядке возрастания и по `price` в порядке убывания

```sql
-- todo;
```

#### 15. Вывести количество записей в таблице `Product`, у которых IsPublished верно, для каждой записи в таблице `ProductStatus`

```sql
-- todo;
```

#### 16. Вывести количество записей в таблице `Product`, у которых `taxable` верно и нет

```sql
-- todo;
```

#### 17. Вывести количество записей в таблице `Product`, для каждой записи в таблице `ProductStatus`, у которых `taxable` верно и нет. Отсортировать записи по имени статуса.

```sql
-- todo;
```
