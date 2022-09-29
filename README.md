# Dannys_Diner

![image](https://user-images.githubusercontent.com/89623051/143564498-b99619f0-2e9d-4518-a3f0-e60c7cc2eed7.png)

### Introduction

This tutorial contains exactly the same content as the 8 Week SQL Challenge Danny’s Diner case study - however everything has been optimised for our SQLPad Docker instance and there are also solutions included below…but there is a twist!

### Context

Danny seriously loves Japanese food so in the beginning of 2021, he decides to embark upon a risky venture and opens up a cute little restaurant that sells his 3 favourite foods: sushi, curry and ramen.

Danny’s Diner is in need of your assistance to help the restaurant stay afloat - the restaurant has captured some very basic data from their few months of operation but have no idea how to use their data to help them run the business.

### Problem Statement

Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favourite. Having this deeper connection with his customers will help him deliver a better and more personalised experience for his loyal customers.

He plans on using these insights to help him decide whether he should expand the existing customer loyalty program - additionally he needs help to generate some basic datasets so his team can easily inspect the data without needing to use SQL.

### Example Datasets

Danny has provided you with a sample of his overall customer data due to privacy issues - but he hopes that these examples are enough for you to write fully functioning SQL queries to help him answer his questions!

Danny has shared with you 3 key datasets for this case study:

sales
menu
members
You can inspect the entity relationship diagram and example data below.

### Entity Relationship Diagram

![image](https://user-images.githubusercontent.com/89623051/143564804-13544558-aa3b-4a9e-89f0-8c47d8f039c6.png)

### TABLES

![image](https://user-images.githubusercontent.com/89623051/143564922-15ca27b7-55e0-40e2-a83f-76ef7f94e7eb.png)

![image](https://user-images.githubusercontent.com/89623051/143565031-48938f68-442c-489e-901e-51ef31d32a2b.png)

### Case Study Questions

Each of the following case study questions can be answered using a single SQL statement:

What is the total amount each customer spent at the restaurant?

How many days has each customer visited the restaurant?

What was the first item from the menu purchased by each customer?

What is the most purchased item on the menu and how many times was it purchased by all customers?

Which item was the most popular for each customer?

Which item was purchased first by the customer after they became a member?

Which item was purchased just before the customer became a member?

What is the total items and amount spent for each member before they became a member?

If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?

In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?
### CASE STUDY SOLUTION

##### 1. What is the total amount each customer spent at the restaurant?

```sql
SELECT
  	SUM(price) AS Total_amount_spent, customer_id AS customer
    
FROM dannys_diner.sales
JOIN dannys_diner.menu 
ON dannys_diner.sales.product_id = dannys_diner.menu.product_id
GROUP BY customer_id
ORDER BY customer_id;
```

| total_amount_spent | customer |
| ------------------ | -------- |
| 76                 | A        |
| 74                 | B        |
| 36                 | C        |

#### 2. How many days has each customer visited the restaurant?
```sql
SELECT
  	  customer_id AS Customer, COUNT(DISTINCT order_date) AS days_visited
    
FROM dannys_diner.sales
GROUP BY customer_id
ORDER BY customer_id;
```

| customer | days_visited |
| -------- | ------------ |
| A        | 4            |
| B        | 6            |
| C        | 2            |

---
##### 3. What was the first item from the menu purchased by each customer?
```sql
WITH Items_purchased_by_rank AS
(
   SELECT customer_id, order_date, product_name,
      DENSE_RANK() OVER(PARTITION BY s.customer_id
      ORDER BY s.order_date) AS rank
   FROM dannys_diner.sales AS s
   JOIN dannys_diner.menu AS m
      ON s.product_id = m.product_id
)
SELECT customer_id AS Customer, product_name AS First_item_purchased
FROM Items_purchased_by_rank
WHERE rank = 1
GROUP BY customer_id, product_name;
```


---


