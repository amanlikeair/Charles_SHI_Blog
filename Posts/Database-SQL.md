[< Home](https://amanlikeair.github.io/Charles_SHI_Blog/)



# Database以及SQL笔记

* * *
2020-Sep-24  
Complete Web Development Bootcamp on Udemy  
<https://www.udemy.com/course/the-complete-web-development-bootcamp/>
* * *
### 使用node js和database对比
database的数据是静态储存于本地的, 不会重载浏览器而消失


### SQL和NoSQL对比
- SQL更适合复杂结构. 代表工具: *MySQL*
- NoSQL更灵活. 代表工具: *MongoDB*

### SQL 文档资源/工具
[w3school](https://www.w3schools.com/sql/)  
[在线sql工具](https://www.sqliteonline.com)

# SQL语法
四个操作: 增删改查

#### 增:

创建表格:

```SQL
CREATE TABLE products{
  id INT NOT NULL,
  name STRING,
  price MONEY,
  PRIMARY KEY (id)
}
```

FOREIGN KEY:

```SQL
CREATE TABLE orders{
  id INT NOT NULL,
  order_number INT,
  customer_id INT,
  product_id INT,
  PRIMARY KEY (id)
  FOREIGN KEY customer_id REFERENCES customer(id),
  FOREIGN KEY product_id REFERENCES products(id)
  -- 设置foreign key,与别的表格相关联
}
```

插入一行数据:

```SQL
INSERT INTO products
VALUES (1, "pen", 1.20)
```

或是插入一行数据(指定部分数据):
```SQL
INSERT INTO products(id, price)
VALUES (1,  1.20)
```

### 查:

查看所有

```SQL
SELECT * FROM products WHERE id=1
```

选择查看

```SQL
SELECT id, price FROM products WHERE id=1
```

JOIN:

```SQL
SELECT order.order_number, customers.first_name, customers.last_name, customers.address
FROM orders
INNER JOIN customers ON orders.customer_id = customer_id
```


### 改:

修改某行:

```SQL
UPDATE products
SET price = 0.8
WHERE id = 2
```

新增一列:

```SQL
ALTER TABLE products
ADD stock INT
```

### 删:

```SQL
DELETE FROM products
WHERE id = 2
```
