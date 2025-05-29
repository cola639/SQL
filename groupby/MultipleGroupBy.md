太棒了！下面我来给你出一个完整的实战练习题，包含：

---

## ✅ 多表 JOIN + GROUP BY 分析 + 二次聚合 场景题目

---

### 🎯 场景背景：

你有一个电商数据库，包含以下三张表：

#### `users`（用户信息）

| id | name  | region |
| -- | ----- | ------ |
| 1  | Alice | East   |
| 2  | Bob   | East   |
| 3  | Carol | West   |
| 4  | David | North  |

#### `orders`（订单信息）

| id  | user\_id | product\_id | amount |
| --- | -------- | ----------- | ------ |
| 101 | 1        | 201         | 100    |
| 102 | 1        | 202         | 200    |
| 103 | 2        | 203         | 50     |
| 104 | 3        | 201         | 300    |
| 105 | 3        | 202         | 200    |
| 106 | 4        | 203         | 150    |

#### `products`（商品信息）

| id  | name   | category |
| --- | ------ | -------- |
| 201 | Laptop | Digital  |
| 202 | Phone  | Digital  |
| 203 | Shoes  | Fashion  |

---

## 🧩 任务：

> 1. 先统计每位用户在每个商品分类（`category`）的总消费（`SUM(amount)`）
> 2. 然后再统计每个地区（`region`）在每类商品的**平均消费金额**

---

## 📝 你该怎么写 SQL？提示结构如下：

```sql
-- 第1层：先统计每个用户在每个商品分类的总消费
WITH user_category_spending AS (
    SELECT u.id AS user_id, u.region, p.category, SUM(o.amount) AS total_spending
    FROM orders o
    JOIN users u ON o.user_id = u.id
    JOIN products p ON o.product_id = p.id
    GROUP BY u.id, u.region, p.category
)

-- 第2层：对每个 region + category 求平均消费
SELECT region, category, AVG(total_spending) AS avg_spending
FROM user_category_spending
GROUP BY region, category;
```

---

## ✅ 最终查询结果示例：

| region | category | avg\_spending |
| ------ | -------- | ------------- |
| East   | Digital  | 175.0         |
| West   | Digital  | 250.0         |
| West   | Fashion  | 300.0         |
| North  | Fashion  | 150.0         |

---

## 🧠 小结你练到的内容：

| 技术点         | 涉及                        |
| ----------- | ------------------------- |
| 多表 JOIN     | orders + users + products |
| GROUP BY 分析 | 分组统计用户消费                  |
| CTE 分层聚合    | 用 `WITH` 结构做二次聚合          |
| 二次 GROUP BY | 第二层区域平均消费                 |
| 分析逻辑        | 用户 → 商品分类 → 地区平均          |

---

你现在可以试着在本地 MySQL/H2 跑一下，或者我可以给你生成：

👉 全套 SQL 脚本（建表 + 插数）
👉 或者 JPA 实现这个分析逻辑的 Repository + DTO

你需要哪种？告诉我：

* `生成SQL脚本`
* `生成JPA统计模板`
