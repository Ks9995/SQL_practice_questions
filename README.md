# SQL_practice_questions
# 📌 Practice Question — 01

Given a table of candidates and their skills, you're tasked with finding the candidates best suited for an open Data Analytics job. You want to find candidates who are proficient in Excel, PowerBI, SQL, and Python.
Write a query to list the candidates who possess all of the required skills for the job. Sort the output by candidate_id in descending order.

<img width="513" height="192" alt="image" src="https://github.com/user-attachments/assets/67bd0637-2708-4be1-ad4f-fc364463e5b1" />

## 💡 Solution

SELECT candidate_id

FROM candidates

WHERE skill IN ('Excel', 'PowerBI', 'SQL', 'Python')

GROUP BY candidate_id

HAVING COUNT(DISTINCT skill) = 4

ORDER BY candidate_id DESC;

---

# 📌 Practice Question — 02

Given a table of Twitter tweets, Write a query that calculates, for the year 2022, how many tweets each user posted, and then groups users by that tweet count. The result should display the tweet count as a bucket along with the number of users who fall into that bucket (i.e., a histogram of tweet frequency per user).

<img width="402" height="282" alt="image" src="https://github.com/user-attachments/assets/f1336757-2b9f-4504-85ad-1085192ab388" />

## 💡 Solution

WITH total_tweets AS (SELECT user_id, COUNT(tweet_id) AS tweet_count_per_user

FROM tweets

WHERE tweet_date BETWEEN '2022-01-01' AND '2022-12-31'

GROUP BY user_id)

SELECT tweet_count_per_user AS tweet_bucket,COUNT(user_id) AS users_num

FROM total_tweets

GROUP BY tweet_count_per_user;

---

# 📌 Practice Question — 03

Assume you have a user_activity table that tracks events on an e-commerce website. Write a SQL query to calculate the Click-Through Rate (CTR) for product ads in 2022, grouped by user_id, and round the result to 2 decimal places.

Definition: CTR (%) = 100.0 * (Number of "ad_click" events) / (Number of "ad_view" events)

⚠️ Note: To avoid integer division, multiply by 100.0 (not 100).

<img width="611" height="283" alt="image" src="https://github.com/user-attachments/assets/7552c474-fba8-41c1-ae20-eadc823d2d75" />

## 💡 Solution

SELECT user_id, ROUND(100.0 * SUM(CASE WHEN action_type = 'ad_click' THEN 1 ELSE 0 END)

/ NULLIF(SUM(CASE WHEN action_type = 'ad_view' THEN 1 ELSE 0 END), 0), 2) AS ctr_percentage

FROM user_activity

WHERE activity_ts >= '2022-01-01'

AND activity_ts < '2023-01-01'

GROUP BY user_id;

---

# 📌 Practice Question — 04

You are given two tables that store information about Instagram pages and the likes they have received.

Write a query to find all Instagram pages that have not received any likes. Return the page IDs in ascending order.

<img width="678" height="241" alt="image" src="https://github.com/user-attachments/assets/09e0f5d7-d2f0-4680-9fb0-e33fb0c1848a" />

<img width="713" height="263" alt="image" src="https://github.com/user-attachments/assets/1cdc87e5-8d8f-4d91-b146-b530e28e43e1" />

## 💡 Solution

SELECT ip.insta_page_id

FROM instagram_pages ip

LEFT JOIN insta_likes il

ON ip.insta_page_id = il.insta_page_id

WHERE il.insta_page_id IS NULL

ORDER BY ip.insta_page_id ASC;

---
# 📌 Practice Question — 05

You are given a table that stores information about users’ posts. For every user who has made at least two posts in the year 2021, write a query to calculate the number of days between their earliest post and latest post within that year.
Return each user_id along with the calculated number of days.

<img width="606" height="317" alt="image" src="https://github.com/user-attachments/assets/f47f76af-af83-4a50-ba4b-c2f1f7fb6905" />


## 💡 Solution

SELECT user_id, DATE_PART('day', MAX(post_date) - MIN(post_date)) AS days_between

FROM posts

WHERE EXTRACT(YEAR FROM post_date) = 2021

GROUP BY user_id

HAVING COUNT(*) >= 2;

---
# 📌 Practice Question — 06

You are given a table that records messages exchanged on Microsoft Teams. Write a query to find the top 2 users who sent the most messages during August 2022.
Return their sender_id along with the total number of messages they sent, ordered from the highest to lowest message count.

<img width="592" height="367" alt="image" src="https://github.com/user-attachments/assets/2eee1a55-635f-4eeb-91da-2cb4d2d9d8c1" />

## 💡 Solution

SELECT sender_id, COUNT(*) AS message_count

FROM messages

WHERE EXTRACT(YEAR FROM sent_date) = 2022

AND EXTRACT(MONTH FROM sent_date) = 8

GROUP BY sender_id

ORDER BY message_count DESC

LIMIT 2;

---

# 📌 Practice Question — 07

You are given a table that stores job postings from multiple companies on LinkedIn. Write a query to find the number of companies that have posted duplicate job listings.
A duplicate job listing is defined as two or more job postings within the same company that have the same job title and description.

<img width="562" height="321" alt="image" src="https://github.com/user-attachments/assets/ed96fd5e-f1d8-4db8-8290-6222bc20b05d" />

## 💡 Solution

SELECT COUNT(DISTINCT company_id) AS duplicate_companies

FROM (SELECT company_id
      
FROM job_listings
    
GROUP BY company_id, title, description
    
HAVING COUNT(*) > 1

) AS dup;

---

# 📌 Practice Question — 08

You are provided with two tables from a trading platform: one containing trade orders and another with user details. Write a query to find the top 3 cities where users have executed the highest number of completed trades. Return the city name along with the total number of completed orders, sorted in descending order by the count.

<img width="908" height="692" alt="image" src="https://github.com/user-attachments/assets/de50ca73-de21-498d-b4b5-a0d031d17364" />

<img width="923" height="575" alt="image" src="https://github.com/user-attachments/assets/9ed12b2a-e436-4188-9509-fcfb2508692e" />

## 💡 Solution

SELECT u.city, COUNT(*) AS total_completed_orders

FROM trades t

JOIN users u

ON t.user_id = u.user_id

WHERE t.status = 'Completed'

GROUP BY u.city

ORDER BY total_completed_orders DESC

LIMIT 3;

---

# 📌 Practice Question — 09

You have a table that stores product reviews. Write a query to calculate the average star rating for each product on a monthly basis.
The results should include: the month of the review (as a number), the product ID, and the average rating rounded to two decimal places.
Finally, sort the output by month and then by product ID.

<img width="613" height="348" alt="image" src="https://github.com/user-attachments/assets/30aadca4-1b0e-4b16-af35-f8f61df0dfcd" />

## 💡 Solution

SELECT EXTRACT(MONTH FROM submit_date) AS mth, product_id,ROUND(AVG(stars), 2) AS avg_stars

FROM reviews

GROUP BY EXTRACT(MONTH FROM submit_date), product_id

ORDER BY mth, product_id;

---
# 📌 Practice Question — 10

In order to evaluate compensation fairness, HR wants to check if some employees are earning higher salaries than their direct managers.
Write a query to find all such employees. Your output should include the employee’s ID and name.

<img width="885" height="682" alt="image" src="https://github.com/user-attachments/assets/46515371-3abf-48af-8ef6-6f22525099a3" />

## 💡 Solution

SELECT E.employee_id, E.name

FROM employee E

JOIN employee M

ON E.manager_id = M.employee_id

WHERE E.salary > M.salary;

---
# 📌 Practice Question — 11

You are given a table that records all PayPal transactions, including deposits and withdrawals, for different accounts. Write a query to calculate the final balance of each account after applying all transactions.
Assume that:
All deposits increase the balance.
All withdrawals decrease the balance.
No transactions are missing from the log.

<img width="877" height="637" alt="image" src="https://github.com/user-attachments/assets/912b4dc8-b9ae-478f-8d59-2c3f8708b4f5" />

## 💡 Solution

SELECT account_id,SUM(CASE WHEN transaction_type = 'Deposit' THEN amount

WHEN transaction_type = 'Withdrawal' THEN -amount

ELSE 0 END) AS final_balance

FROM transactions

GROUP BY account_id;

---
# 📌 Practice Question — 12

A tech company wants to evaluate how actively their employees are using the company’s Db2 database system. Each query run by an employee is logged, and management would like to build a histogram that shows how many unique queries employees executed in Q3 of 2023 (July–September).
The histogram should also include employees who did not run any queries in that timeframe.
Write a SQL query to display:
The number of distinct queries executed (as the histogram bucket).
The count of employees who fall into each bucket.

queries table:
employee_id (integer) – Employee who ran the query.
query_id (integer) – Unique identifier of the query.
query_starttime (datetime) – Time the query started.
execution_time (integer) – Duration of query execution in seconds.

employees table:
employee_id (integer) – Unique ID of each employee.
full_name (string) – Employee’s name.
gender (string) – Gender of the employee.

<img width="878" height="578" alt="image" src="https://github.com/user-attachments/assets/b7ed3bef-9424-4981-bc38-40cdb4f772c0" />

## 💡 Solution

SELECT COALESCE(qry.unique_queries, 0) AS unique_queries, COUNT(*) AS employee_count

FROM employees e

LEFT JOIN (SELECT employee_id, COUNT(DISTINCT query_id) AS unique_queries
    
FROM queries

WHERE query_starttime >= '2023-07-01'

AND query_starttime < '2023-10-01'
    
GROUP BY employee_id) qry

ON e.employee_id = qry.employee_id

GROUP BY COALESCE(qry.unique_queries, 0)

ORDER BY unique_queries;

---
# 📌 Practice Question — 13

Scenario:
Your analytics team is reviewing the performance of different credit card products by looking at how many cards were issued over time. The goal is to find out which cards show the largest swings in monthly issuance volumes.

Task:
Write a SQL query that returns each card’s name along with the difference between its highest and lowest number of issued cards across months. The result should be sorted so that cards with the largest differences appear first.

<img width="886" height="641" alt="image" src="https://github.com/user-attachments/assets/368e3434-eac5-4dd1-9887-ea25eaa1391b" />

## 💡 Solution

SELECT card_name, MAX(issued_amount) - MIN(issued_amount) AS difference

FROM monthly_cards_issued

GROUP BY card_name

ORDER BY difference DESC;

---

# 📌 Practice Question — 14

An e-commerce analyst at Alibaba wants to calculate the average number of items included in each order. The available dataset summarizes how many items are in each order (item_count) and how many orders fall into each category (order_occurrences).
Write a query to compute the mean items per order, rounded to one decimal place.

<img width="892" height="523" alt="image" src="https://github.com/user-attachments/assets/8914d835-0496-4d29-a1ed-cc650f57aac2" />

## 💡 Solution

SELECT ROUND(CAST(SUM(item_count * order_occurrences) AS DECIMAL)/SUM(order_occurrences),

1) AS mean

FROM items_per_order;

































