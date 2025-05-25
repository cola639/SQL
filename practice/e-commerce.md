太好了！这是你主动练习复杂 SQL 的最佳方式 👏
我来为你出一道“**每类商品销量前 1 名**”的综合练习题，包含：

---

## ✅ 一、题目目标：

> 在一个电商订单系统中，找出每个商品分类（category）中，销量最高的商品（按销售数量排序，取第一）。

---

## ✅ 二、你需要实现的是：

用以下结构（必须用）：

```sql
FROM (SELECT ...) t
WHERE ...
```

---

## ✅ 三、表结构设计（DDL）

你可以在 H2 / MySQL 中执行：

```sql
-- 商品表
CREATE TABLE products (
  id INT PRIMARY KEY,
  name VARCHAR(50),
  category VARCHAR(50)
);

-- 订单明细表（多条表示多个购买记录）
CREATE TABLE order_items (
  id INT PRIMARY KEY,
  product_id INT,
  quantity INT
);
```

---

## ✅ 四、测试数据（INSERT）

```sql
-- 商品数据
INSERT INTO products (id, name, category) VALUES
(1, 'Laptop A', 'Digital'),
(2, 'Laptop B', 'Digital'),
(3, 'Sneaker X', 'Fashion'),
(4, 'Sneaker Y', 'Fashion'),
(5, 'Smartphone P', 'Digital');

-- 订单明细数据（表示销量）
INSERT INTO order_items (id, product_id, quantity) VALUES
(101, 1, 10),
(102, 1, 20),
(103, 2, 5),
(104, 3, 40),
(105, 3, 10),
(106, 4, 55),
(107, 5, 100);
```

---

## ✅ 五、你要实现的逻辑目标

基于上面数据，查询出每个分类（category）中：

- 销量（order_items 中 quantity 的总和）最高的商品（products 表中的 name）

---

## ✅ 六、预期结果：

| category | product_name | total_quantity |
| -------- | ------------ | -------------- |
| Digital  | Smartphone P | 100            |
| Fashion  | Sneaker Y    | 55             |

> 因为：

- Digital 类中：Smartphone P = 100，Laptop A = 30，Laptop B = 5
- Fashion 类中：Sneaker Y = 55，Sneaker X = 50

---

## ✅ 七、练习要求提示

1. 你**必须用子查询 + 别名**：

   ```sql
   FROM (SELECT ...) t
   ```

2. 你可能会需要：

   - `GROUP BY` 统计销量
   - `ROW_NUMBER()` 或 `RANK()` 排序
   - 外层查询筛选第一名

---

## ✍️ 请你尝试写出完整 SQL 语句！

写好贴上来，我可以：

- 给你逐句讲解语法 ✅
- 检查是否满足练习目标 ✅
- 如果有错误，我会提示你哪错了 ✅

准备好了吗？上你的答案吧！💪

```mysql
SELECT category, name, total_quantity
FROM (
    SELECT
        p.category,
        p.name,
        SUM(oi.quantity) AS total_quantity,
        ROW_NUMBER() OVER (PARTITION BY p.category ORDER BY SUM(oi.quantity) DESC) AS rn
    FROM products p
    JOIN order_items oi ON p.id = oi.product_id
    GROUP BY p.id, p.name, p.category
) t
WHERE rn = 1;
```
