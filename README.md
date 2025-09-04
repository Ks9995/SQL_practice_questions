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

SELECT ROUND(CAST(SUM(item_count * order_occurrences) AS DECIMAL)/SUM(order_occurrences),1) AS mean

FROM items_per_order;

---
# 📌 Practice Question — 15

You are analyzing ride history data for an Uber-like platform. For each user, you need to find the third transaction they made, based on the order in which transactions occurred.
Write a SQL query to return the following columns:user_id, spend (amount spent in that transaction), transaction_date.
Only include the record corresponding to each user’s third transaction.

<img width="822" height="570" alt="image" src="https://github.com/user-attachments/assets/3dff70ed-0880-455b-8195-8a8ef843160e" />

## 💡 Solution

SELECT user_id, spend, transaction_date

FROM (SELECT user_id,spend,transaction_date,ROW_NUMBER() OVER (PARTITION BY user_id

ORDER BY transaction_date) AS txn_rank

FROM transactions) ranked

WHERE txn_rank = 3;

---
# 📌 Practice Question — 16

You are working with the HR analytics team at a software company. Management wants to review compensation trends and has asked you to find the second highest salary across all employees.
If more than one employee shares that second highest salary, display the salary only once in the output.

<img width="912" height="625" alt="image" src="https://github.com/user-attachments/assets/139661fc-f6e8-4a4a-b749-6949661b6a70" />

## 💡 Solution

SELECT DISTINCT salary AS second_highest_salary

FROM (SELECT salary,DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk

FROM employee) ranked_salaries

WHERE rnk = 2;

---
# 📌 Practice Question — 17

You’re working as a data analyst for a social media platform that wants to study user engagement trends over time.
Your task is to compute a 3-day rolling average of tweets for each user.
For every user, display: user_id, tweet_date, the 3-day rolling average of tweet counts (rounded to 2 decimal places)
This will help visualize how active users are over time and smooth out day-to-day fluctuations in tweet volume.

<img width="888" height="615" alt="image" src="https://github.com/user-attachments/assets/f36be4d2-0fc3-4fc5-90ca-fefce4a70da0" />

## 💡 Solution

SELECT user_id, tweet_date, ROUND(AVG(tweet_count) OVER (PARTITION BY user_id 

ORDER BY tweet_date
            
ROWS BETWEEN 2 PRECEDING AND CURRENT ROW), 2) AS rolling_avg_3d

FROM tweets

ORDER BY user_id, tweet_date;

---
# 📌 Practice Question — 18

You are analyzing Snapchat user engagement data to understand how users spend their time on the platform. Each activity logged includes whether a user sent or opened a snap, along with the time spent on that activity.
Write an SQL query that calculates, for each age group, the percentage of time spent on sending snaps and the percentage of time spent on opening snaps, relative to the total time spent on these two activities.
Round both percentages to two decimal places and display them in separate columns (send_perc and open_perc). Exclude other activity types (like "chat") from your calculations.

<img width="961" height="592" alt="image" src="https://github.com/user-attachments/assets/d65f0346-8148-426c-b796-2d49a627e431" />

<img width="565" height="185" alt="image" src="https://github.com/user-attachments/assets/3ce69fda-d0fa-49ba-b279-fcca358ce882" />

## 💡 Solution

SELECT ab.age_bucket, ROUND(100.0 * SUM(CASE WHEN a.activity_type = 'send' THEN a.time_spent ELSE 0 END) /
         
NULLIF(SUM(CASE WHEN a.activity_type IN ('send','open') THEN a.time_spent ELSE 0 END), 0), 2) AS send_perc,

ROUND(100.0 * SUM(CASE WHEN a.activity_type = 'open' THEN a.time_spent ELSE 0 END) /

NULLIF(SUM(CASE WHEN a.activity_type IN ('send','open') THEN a.time_spent ELSE 0 END), 0), 2) AS open_perc

FROM activities a

JOIN age_breakdown ab

ON a.user_id = ab.user_id

WHERE a.activity_type IN ('send', 'open')

GROUP BY ab.age_bucket

ORDER BY ab.age_bucket;

---
# 📌 Practice Question — 19

You're analyzing Amazon sales data and want to understand which products drive the most revenue within each product category.
Write a SQL query to find the top two highest-earning products in every category for the year 2022. Your output should include:
The category name
The product name
The total amount spent on that product (across all customers)
Sort the results so that the highest-earning products appear first within each category.

<img width="972" height="657" alt="image" src="https://github.com/user-attachments/assets/d4a58af3-6697-4dbd-ac36-5ac37d5c4981" />

## 💡 Solution

WITH product_totals AS (SELECT category,product,SUM(spend) AS total_spend

FROM product_spend

WHERE EXTRACT(YEAR FROM transaction_date) = 2022

GROUP BY category, product),

ranked_products AS (SELECT category,product,total_spend,
        
RANK() OVER (PARTITION BY category ORDER BY total_spend DESC) AS rnk

FROM product_totals)

SELECT category,product,total_spend

FROM ranked_products

WHERE rnk <= 2

ORDER BY category, total_spend DESC;

---
# 📌 Practice Question — 20

You're working on a salary analysis project to identify top talent within the company. Your manager asks you to prepare a report listing the top three highest-paid employees in each department based on their salaries.Write a SQL query that returns:
Employee name
Department name
Salary

Make sure to:Use a ranking window function to correctly handle duplicate salaries (multiple employees can share the same rank).
Sort the final results by department name (A–Z), then by salary (highest first), and finally by employee name (alphabetically) when salaries are tied.

<img width="826" height="571" alt="image" src="https://github.com/user-attachments/assets/b502a4f3-f2ef-4fbc-b01e-3ebb07fb8ece" />

<img width="898" height="508" alt="image" src="https://github.com/user-attachments/assets/55d8cded-a526-4a32-8170-936aac175467" />

<img width="651" height="237" alt="image" src="https://github.com/user-attachments/assets/d27b43ad-7018-49a3-9a75-c4428db22ee2" />

## 💡 Solution

WITH ranked_employees AS (SELECT e.name, d.department_name,e.salary,
        
DENSE_RANK() OVER (PARTITION BY e.department_id ORDER BY e.salary DESC) AS salary_rank
    
FROM employee e

JOIN department d

ON e.department_id = d.department_id)

SELECT name, department_name,salary

FROM ranked_employees

WHERE salary_rank <= 3

ORDER BY department_name ASC, salary DESC, name ASC;

---
# 📌 Practice Question — 21

TikTok tracks new user sign-ups through their email addresses. After signing up, users receive one or more text messages prompting them to confirm their account activation. Some users may need multiple reminders before confirming, while others might never confirm at all.
Your task as a data analyst is to calculate the activation rate for the set of users listed in the emails table. The activation rate represents the percentage of these users who successfully confirmed their accounts (at least once).
Round the activation rate to two decimal places in the output.

<img width="833" height="531" alt="image" src="https://github.com/user-attachments/assets/6e9afb09-9464-4436-8554-f85c07d44ecd" />

<img width="783" height="513" alt="image" src="https://github.com/user-attachments/assets/07312041-a09c-42cb-8737-9fa56c045ebf" />

## 💡 Solution

SELECT ROUND(COUNT(t.email_id)::DECIMAL / COUNT(DISTINCT e.email_id),2) AS activation_rate

FROM emails e

LEFT JOIN texts t
  
ON e.email_id = t.email_id

AND t.signup_action = 'Confirmed';

---
# 📌 Practice Question — 22

You have two tables containing Spotify streaming activity — one with historical listening data (songs_history) and one with data from the current week (songs_weekly).
Write a query to return each user's song play count as of August 4, 2022, including both their past plays (from songs_history) and plays recorded during the current week (from songs_weekly).
Your output should list:
user_id
song_id
the total cumulative number of times that song has been played by that user up until August 4, 2022.
The results should be sorted by the cumulative song play count in descending order

🔑 Key Details:
songs_history contains all plays that occurred before August 1, 2022.
songs_weekly contains plays recorded between August 1 and August 7, 2022.
Be sure to include users or songs that appear only in the weekly table (i.e., new users or new songs).
Only consider data from the weekly table up to and including August 4, 2022.

<img width="872" height="567" alt="image" src="https://github.com/user-attachments/assets/a9dd2a5f-fb20-45bc-81b1-9666f3703a4b" />

<img width="853" height="557" alt="image" src="https://github.com/user-attachments/assets/9a3cc49b-a089-4304-b2d1-4ce5edc47140" />

## 💡 Solution

SELECT user_id, song_id, SUM(song_plays) AS song_count

FROM (SELECT user_id, song_id, song_plays FROM songs_history

UNION ALL

SELECT user_id, song_id, COUNT(*) AS song_plays
    
FROM songs_weekly

WHERE listen_time <= '2022-08-04 23:59:59'

GROUP BY user_id, song_id) AS combined

GROUP BY user_id, song_id

ORDER BY song_count DESC;






















































