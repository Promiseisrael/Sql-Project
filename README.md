# Sql-Project

CREATE TABLE sales (
  "customer_id" VARCHAR(1),
  "order_date" DATE,
  "product_id" INTEGER
);

INSERT INTO sales
  ("customer_id", "order_date", "product_id")
VALUES
  ('A', '2021-01-01', '1'),
  ('A', '2021-01-01', '2'),
  ('A', '2021-01-07', '2'),
  ('A', '2021-01-10', '3'),
  ('A', '2021-01-11', '3'),
  ('A', '2021-01-11', '3'),
  ('B', '2021-01-01', '2'),
  ('B', '2021-01-02', '2'),
  ('B', '2021-01-04', '1'),
  ('B', '2021-01-11', '1'),
  ('B', '2021-01-16', '3'),
  ('B', '2021-02-01', '3'),
  ('C', '2021-01-01', '3'),
  ('C', '2021-01-01', '3'),
  ('C', '2021-01-07', '3');
 

CREATE TABLE menu (
  "product_id" INTEGER,
  "product_name" VARCHAR(5),
  "price" INTEGER
);

INSERT INTO menu
  ("product_id", "product_name", "price")
VALUES
  ('1', 'sushi', '10'),
  ('2', 'curry', '15'),
  ('3', 'ramen', '12');
  
drop table menu
CREATE TABLE members (
  "customer_id" VARCHAR(1),
  "join_date" DATE
);

INSERT INTO members
  ("customer_id", "join_date")
VALUES
  ('A', '2021-01-07'),
  ('B', '2021-01-09');
  
drop table members
  
CREATE TABLE orders (
  customer_id VARCHAR(1),
  order_date DATE,
  product_name VARCHAR(10),
  price INT,
  member CHAR(1)
);

INSERT INTO orders (customer_id, order_date, product_name, price, member) VALUES
('A', '2021-01-01', 'curry', 15, 'N'),
('A', '2021-01-01', 'sushi', 10, 'N'),
('A', '2021-01-07', 'curry', 15, 'Y'),
('A', '2021-01-10', 'ramen', 12, 'Y'),
('A', '2021-01-11', 'ramen', 12, 'Y'),
('A', '2021-01-11', 'ramen', 12, 'Y'),
('B', '2021-01-01', 'curry', 15, 'N'),
('B', '2021-01-02', 'curry', 15, 'N'),
('B', '2021-01-04', 'sushi', 10, 'N'),
('B', '2021-01-11', 'sushi', 10, 'Y'),
('B', '2021-01-16', 'ramen', 12, 'Y'),
('B', '2021-02-01', 'ramen', 12, 'Y'),
('C', '2021-01-01', 'ramen', 12, 'N'),
('C', '2021-01-01', 'ramen', 12, 'N'),
('C', '2021-01-07', 'ramen', 12, 'N');

drop table orders
  
  
CREATE TABLE orders (
  customer_id VARCHAR(1),
  order_date DATE,
  product_name VARCHAR(10),
  price INT,
  members CHAR(1),
  ranking INT
);

INSERT INTO orders (customer_id, order_date, product_name, price, members, ranking) VALUES
('A', '2021-01-01', 'curry', 15, 'N', NULL),
('A', '2021-01-01', 'sushi', 10, 'N', NULL),
('A', '2021-01-07', 'curry', 15, 'Y', 1),
('A', '2021-01-10', 'ramen', 12, 'Y', 2),
('A', '2021-01-11', 'ramen', 12, 'Y', 3),
('A', '2021-01-11', 'ramen', 12, 'Y', 3),
('B', '2021-01-01', 'curry', 15, 'N', NULL),
('B', '2021-01-02', 'curry', 15, 'N', NULL),
('B', '2021-01-04', 'sushi', 10, 'N', NULL),
('B', '2021-01-11', 'sushi', 10, 'Y', 1),
('B', '2021-01-16', 'ramen', 12, 'Y', 2),
('B', '2021-02-01', 'ramen', 12, 'Y', 3),
('C', '2021-01-01', 'ramen', 12, 'N', NULL),
('C', '2021-01-01', 'ramen', 12, 'N', NULL),
('C', '2021-01-07', 'ramen', 12, 'N', NULL);

---What is the total amount each customer spent at the restaurant?
---How many days has each customer visited the restaurant?
---What was the first item from the menu purchased by each customer?
---What is the most purchased item on the menu and how many times was it purchased by all customers?
---Which item was the most popular for each customer?
---Which item was purchased first by the customer after they became a member?
---Which item was purchased just before the customer became a member?
---What is the total items and amount spent for each member before they became a member?
---If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
---In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?


---What is the total amount each customer spent at the restaurant?---
1)
select customer_id, sum(price) Total_amount from orders
group by 1
order by 1

2) 
--How many days has each customer visited the restaurant?
select customer_id, count(order_date) as no_of_days from orders
group by 1
order by 1

3)
--- What was the first item from the menu purchased by each customer?

select customer_id, product_name from orders
where order_date = '2021-01-01'
group by 2,1
limit 3

4) 
---What is the most purchased item on the menu and how many times was it purchased by all customers?
select product_name, count(product_name) from orders
group by 1
order by 1

5)
---Which item was the most popular for each customer?
select customer_id, product_name from orders
group by 1,2
order by 1
--- Incomplete

6)
---Which item was purchased first by the customer after they became a member?


SELECT customer_id, MIN(order_date) AS first_purchase_date, product_name
FROM orders
WHERE members = 'Y'
GROUP BY customer_id, product_name
ORDER BY customer_id, first_purchase_date;

7) 
---Which item was purchased just before the customer became a member?

SELECT customer_id, MIN(order_date) AS first_purchase_date, product_name
FROM orders
WHERE members = 'N'
GROUP BY customer_id, product_name
ORDER BY customer_id 

8)
---What is the total items and amount spent for each member before they became a member?

select product_name, count(product_name), Sum(price) from orders
where members = 'N'
group by 1
order by 1

9) 
---If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
select customer_id, sum(case when product_name = 'sushi' then (price * 2 * 10)
else(price * 10) end) as total_points from orders
group by 1




10)
---In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?

SELECT
  customer_id,
  SUM(CASE
      WHEN order_date >= '2021-01-01' AND order_date <= '2021-01-07' THEN (price * 2 * 10)
      ELSE (price * 10)
    END) AS total_points
FROM
  orders
GROUP BY
  customer_id;
