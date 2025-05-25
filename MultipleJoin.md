太好了！下面是一个完整的可执行 **多表 JOIN 练习 Demo**，适合你练习 `WITH + 多表 JOIN + SQL 查询逻辑`。你可以直接复制运行于 MySQL、H2、PostgreSQL（几乎无改动），并包含：

---

## ✅ 一、创建数据库结构（6 张表）

## ✅ 二、插入测试数据

## ✅ 三、执行 JOIN 查询练习

## ✅ 四、思考题 + 答案

---

## ✅ 一、建表语句（DDL）

```sql
-- 用户表
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);

-- 商品表
CREATE TABLE products (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);

-- 订单表
CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    product_id INT,
    delivery_id INT,
    amount DECIMAL(10,2),
    status VARCHAR(20)
);

-- 配送表
CREATE TABLE delivery (
    id INT PRIMARY KEY,
    region_id INT,
    delivery_time DATE
);

-- 区域表
CREATE TABLE region (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);
```

---

## ✅ 二、插入测试数据

```sql
-- 用户
INSERT INTO users (id, name) VALUES 
(1, 'Alice'), (2, 'Bob'), (3, 'Carol');

-- 商品
INSERT INTO products (id, name) VALUES 
(101, 'Laptop'), (102, 'Smartphone'), (103, 'Tablet');

-- 区域
INSERT INTO region (id, name) VALUES 
(10, 'North'), (11, 'South');

-- 配送
INSERT INTO delivery (id, region_id, delivery_time) VALUES 
(201, 10, '2024-05-01'), 
(202, 11, '2024-05-02');

-- 订单
INSERT INTO orders (id, user_id, product_id, delivery_id, amount, status) VALUES
(301, 1, 101, 201, 899.99, 'PAID'),
(302, 2, 102, 202, 499.99, 'PENDING'),
(303, 3, 103, 201, 699.99, 'PAID'),
(304, 1, 103, 202, 799.99, 'CANCELLED');
```

---

## ✅ 三、练习 SQL（你来写写）

### 🎯 目标：

查询已付款订单（status='PAID'），显示：用户名、商品名、订单金额、配送区域。

### 📝 练习 SQL（你试试）

```sql
WITH latest_order AS (
    SELECT * FROM orders WHERE status = 'PAID'
),
user_info AS (
    SELECT id, name FROM users
),
product_info AS (
    SELECT id, name FROM products
)
SELECT 
    u.name AS user_name,
    p.name AS product_name,
    o.amount,
    r.name AS region_name
FROM latest_order o
JOIN user_info u ON o.user_id = u.id
JOIN product_info p ON o.product_id = p.id
JOIN delivery d ON o.delivery_id = d.id
JOIN region r ON d.region_id = r.id;
```

---

## ✅ 四、练习结果预期：

| user\_name | product\_name | amount | region\_name |
| ---------- | ------------- | ------ | ------------ |
| Alice      | Laptop        | 899.99 | North        |
| Carol      | Tablet        | 699.99 | North        |

---

## 🧠 你可以进一步练习的变体：

| 练习题                      | 思路提示                         |
| ------------------------ | ---------------------------- |
| 1. 查询每个区域配送的所有订单（含状态）    | 先 JOIN `delivery` + `region` |
| 2. 查询每个用户下的最新一条订单        | 使用 `ROW_NUMBER()` 分组排序       |
| 3. 查询未付款订单（LEFT JOIN 产品） | `WHERE status != 'PAID'`     |

---

## ✅ 是否需要 JPA 实现这张多表结构 + SQL 查询？

只要你说一句：
👉 `生成JPA 6表 join 结构 + Repository + DTO`

我可以帮你把这个练习完整封装为 Spring Boot 项目片段（含实体类、VO、SQL语句、Repository 查询接口），供你实战练习。

是否继续？
