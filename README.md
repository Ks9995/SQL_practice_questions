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











