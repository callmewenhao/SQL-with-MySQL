## USING 子句

USING 简化 JOIN 的拼接条件

```sql
SELECT 
    o.order_id,
    c.first_name
FROM orders o  
JOIN customers c
	ON o.customer_id = c.customer_id
```

- 使用 USING 简化上述代码。前提是使用的列名，在两张表中都相同

```sql
SELECT
    DISTINCT o.customer_id,
    order_id
FROM orders o
JOIN customers c
    USING (customer_id)  -- 注意加 ()
JOIN shippers
	USING (shipper_id)
```

- 使用 USING 简化多条件 JOIN 代码，优化 ON 和 AND 的使用

```sql
SELECT *
FROM order_items oi
JOIN order_item_notes oin
    USING (order_id, product_id)
```

- 练习：

```sql
## USING 子句

USING 简化 JOIN 的拼接条件

```sql
SELECT 
    o.order_id,
    c.first_name
FROM orders o  
JOIN customers c
	ON o.customer_id = c.customer_id
```

- 使用 USING 简化上述代码。前提是使用的列名，在两张表中都相同

```sql
SELECT
    DISTINCT o.customer_id,
    order_id
FROM orders o
JOIN customers c
    USING (customer_id)  -- 注意加 ()
JOIN shippers
	USING (shipper_id)
```

- 使用 USING 简化多条件 JOIN 代码，优化 ON 和 AND 的使用

```sql
SELECT *
FROM order_items oi
JOIN order_item_notes oin
    USING (order_id, product_id)
```

- 练习：

```sql
USE sql_invoicing;

SELECT 
	p.date, -- 名字就是 date
    c.name AS client,
    p.amount,
    pm.name
FROM payments p
JOIN clients c
    USING (client_id)
JOIN payment_methods pm
	ON p.payment_method = pm.payment_method_id
```

## 自然连接 natural join

不建议使用。因为不用告诉数据库使用哪个列名连接，他自己会找一个进行 JOIN，不确定性太大

```sql
SELECT -- 这里的结果与 INNER 的效果一样
	o.order_id,
    c.first_name
FROM orders o
NATURAL JOIN customers c
```

## CROSS JOIN 交叉拼接

两个表中的每条记录都会相互结合，没有结合条件

```sql
SELECT 
    c.first_name AS customer,
    p.name AS product
FROM customers c
CROSS JOIN products p  -- 显式的交叉拼接写法
ORDER BY c.first_name

SELECT 
    c.first_name AS customer,
    p.name AS product
FROM customers c, products p -- 隐式的交叉拼接写法
ORDER BY c.first_name
```

- 练习：CROSS JOIN 表 shippers 和 表 products

```sql
SELECT 
    sh.name AS shipper,
    p.name AS product
FROM shippers sh, products p
ORDER BY sh.name

SELECT 
    sh.name AS shipper,
    p.name AS product
FROM shippers sh
CROSS JOIN products p
ORDER BY sh.name
```

## UNION

合并多张表中的行，或者合并多个查询的结果

注意查询返回的列数目一定要一致，否则就会报错

整体结果的列名和第一段查询的列名一致

```sql
SELECT 
    order_id,
    order_date,
    'Active' AS status
FROM orders
WHERE order_date >= '2019-01-01'

UNION  -- 将两段查询结果拼起来

SELECT 
    order_id,
    order_date,
    'Archived' AS status
FROM orders
WHERE order_date < '2019-01-01'
```

- 练习：

```sql
SELECT 
    customer_id,
    first_name,
    points,
    'Bronze' AS type
FROM customers
WHERE points < 2000

UNION  -- 将两段查询结果拼起来

SELECT 
    customer_id,
    first_name,
    points,
    'Silver' AS type
FROM customers
WHERE points BETWEEN 2000 AND 3000

UNION

SELECT 
    customer_id,
    first_name,
    points,
    'Gold' AS type
FROM customers
WHERE 3000 < points

ORDER BY first_name
```








