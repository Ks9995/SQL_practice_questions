# SQL_practice_questions
SQL practice question set01

Given a table of Twitter tweets, Write a query that calculates, for the year 2022, how many tweets each user posted, and then groups users by that tweet count. The result should display the tweet count as a bucket along with the number of users who fall into that bucket (i.e., a histogram of tweet frequency per user).

<img width="402" height="282" alt="image" src="https://github.com/user-attachments/assets/f1336757-2b9f-4504-85ad-1085192ab388" />

solution - 

WITH total_tweets AS (SELECT user_id, COUNT(tweet_id) AS tweet_count_per_user

FROM tweets

WHERE tweet_date BETWEEN '2022-01-01' AND '2022-12-31'

GROUP BY user_id)

SELECT tweet_count_per_user AS tweet_bucket,COUNT(user_id) AS users_num

FROM total_tweets

GROUP BY tweet_count_per_user;

#SQL practice question set02

Scenario: Assume you have a user_activity table that tracks events on an e-commerce website. Write a SQL query to calculate the Click-Through Rate (CTR) for product ads in 2022 and round the result to 2 decimal places.

Definition: CTR (%) = 100.0 * (Number of "ad_click" events) / (Number of "ad_view" events)

⚠️ Note: To avoid integer division, multiply by 100.0 (not 100).

<img width="611" height="283" alt="image" src="https://github.com/user-attachments/assets/7552c474-fba8-41c1-ae20-eadc823d2d75" />

Solution - 

SELECT ROUND(100.0 * SUM(CASE WHEN action_type = 'ad_click' THEN 1 ELSE 0 END)
          / NULLIF(SUM(CASE WHEN action_type = 'ad_view' THEN 1 ELSE 0 END), 0), 2) AS ctr_percentage
FROM user_activity
WHERE activity_ts >= '2022-01-01'
AND activity_ts < '2023-01-01';








