### Basic KPI’s With SQL



While every company’s metrics are defined slightly differently, the basics are usually very similar.

No one metric can tell the whole story. That’s why it’s so helpful to have many KPIs on the same dashboard.

Here are some Basic KPIs and there implematation using SQL

**basic KPIs like Daily Revenue, Daily Active Users, ARPU, and Retention**



* Key Performance Indicators are high level health metrics for a business.

* Daily Revenue is the sum of money made per day.

* Daily Active Users are the number of unique users seen each day

* Daily Average Revenue Per Purchasing User (ARPPU) is the average amount of money spent by purchasers each day.

* Daily Average Revenue Per User (ARPU) is the average amount of money across all users.

* 1 Day Retention is defined as the number of players from Day N who came back to play again on Day N+1.





I) **Daily Revenue**

**Daily Revenue is simply the sum of money made per day.**

```sql
select

 date(/\**/), — enter the date col

 round(sum(/\**/), 2) — enter the sum pay col

from /\**/

group by 1

order by 1;
```







II)  **Daily Active Users**

**DAU is defined as the number of unique users active each day**



```SQL
**select**

 **date(/\**/), — enter the date col**

 **count(DISTINCT /\**/) as dau**

**from /\**/**

**group by 1**

**order by 1;**
```







**III) ARPPU**



***Daily ARPPU\* - Average Revenue Per Purchasing User.** 

**This metric shows if the average amount of money spent by purchasers is going up over time.**

**Daily ARPPU is defined as the sum of revenue divided by the number of purchasers per day.** 

**Or : sum of revenue per day/ number of purchasers per day**



```SQL
**select**

 **date(/\**/), -- enter the date col**

 **round(sum(/\**/) / count( distinct/\**/), 2) as arppu — sum of revenue per day/ number of purchasers per day(user))**

**from /\**/**

**group by 1**

**order by 1;**
```





**The more popular (and difficult) cousin to Daily ARPPU is Daily ARPU, Average Revenue Per User. ARPU measures the average amount of money we’re getting across all users, whether or not they’ve purchased.**



**ARPPU increases if purchasers are spending more money. ARPU increases if more players are choosing to purchase, even if the purchase size stays consistent.**



**IV) ARPU**



**Daily ARPU is defined as revenue divided by the number of users, per-day.**

 **To get that, we’ll need to calculate the daily revenue and daily active users separately, and then join them on their dates.**



```sql
WITH daily_revenue AS (
 SELECT
  DATE(/\**/) AS dt, — date col
  ROUND(SUM(/\**/), 2) as rev — payments col
 FROM /\**/ — purchases-table or eqivalent 
 GROUP BY 1
),
daily_users AS (
 SELECT
	DATE(/\**/) AS dt, — date col  
	COUNT(DISTINCT /\**/) as users — users_id col
FROM /\**/ — users table
GROUP BY 1)
SELECT 
	daily_revenue.dt,
	daily_revenue.rev / daily_players.players — this the main formula
FROM daily_revenue
JOIN daily_users USING (dt);**
```



​	



**1 Day Retention**

**Now let’s find out what percent of users are returning to the next day. This KPI is 1 Day Retention. (We can use any other time period like a month a quarter, year )**

**Retention can be defined many different ways, but we’ll stick to the most basic definition.** 

**For all users on Day N, we’ll consider them retained if they came back to play again on Day N+1.**

**1 Day Retention is defined as the number of users who returned the next day divided by the number of original users, per day.**

**number of users who returned / number of original users**



**Suppose 100 users logged on jun 2d. If 40 of them logged on Jun 3d, the 1 day retention for jun 2d is 40%.**

**This will let us track whether or not our product/e-store/web_site is getting “stickier” over time. The stickier our product, the more days users will spend using it.**

**And more time using the product means more opportunities to monetize and grow our business.**





```sql
SELECT
DATE(/\**/) as dt, — enter the date col
ROUND(100.0 \* COUNT(DISTINCT user_2./\**/) / COUNT(DISTINCT user_1/\**/) ,1) as retention_rate — enter the user id col, THIS IS THE MAIN FORMULA number of users who #returned(from user_2 table) / number of original users(user_1 table)
FROM /\**/ as user_1 — using user table and aliasing it as _1 to prepare for the self join
LEFT JOIN /\**/ as user_1 — self join with itself calling the second _2, we use left join #to keep all the not retained user_ids
ON user_1./\**/ = user_2./\**/ # ENTER THE USER_ID col we want only the same user_ids self  #join will join all rows with all rows i.e all users and all dates with all users and dates #and we don’t want
AND DATE(user_1./\**/, ‘+1 day’) = DATE(user_2./\**/) — entering the date columns joining on the following days
GROUP BY 1 — group by date of the day we are looking for retention rate
ORDER BY 1 — order by date   
```

