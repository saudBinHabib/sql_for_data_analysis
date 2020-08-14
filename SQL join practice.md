Join Practice
==========

This file contains the detail practice of joins with questions.

1) Provide a table for all web_events associated with account name of Walmart. There should be three columns. Be sure to include the primary_poc, time of the event, and the channel for each event. Additionally, you might choose to add a fourth column to assure only Walmart events were chosen.
~~~~~~~~~~~~~~~~

::
	SELECT account.name, account.primary_poc, web.channel, web.occurred_at 
	FROM web_events AS web
	JOIN accounts AS account
	ON web.account_id = account.id AND account.name LIKE '%Walmart%'

~~~~~~~~~~~~~~~~

2) Provide a table that provides the region for each sales_rep along with their associated accounts. Your final table should include three columns: the region name, the sales rep name, and the account name. Sort the accounts alphabetically (A-Z) according to account name.

::
    

	SELECT sales_region.region_name, sales_region.sales_representive, accounts.name AS account_name
	FROM 
	(
  		SELECT sales_reps.id AS id, region.name AS region_name, sales_reps.name AS sales_representive
  		FROM sales_reps
  		JOIN region
  		ON sales_reps.region_id = region.id
	) AS sales_region
	JOIN accounts
	ON accounts.sales_rep_id = sales_region.id
	ORDER BY accounts.name ASC


~~~~~~~~~~~~~~~~

3) Provide the name for each region for every order, as well as the account name and the unit price they paid (total_amt_usd/total) for the order. Your final table should have 3 columns: region name, account name, and unit price. A few accounts have 0 for total, so I divided by (total + 0.01) to assure not dividing by zero.

::
	SELECT sales_region.name as region_name, account_orders.name as account_name, account_orders.unit_price
	FROM 
	(
  		SELECT accounts.sales_rep_id as sales_rep_id, accounts.name,   (orders.total_amt_usd) / (orders.total + 0.01) AS unit_price
  		FROM orders
  		JOIN accounts
  		ON orders.account_id = accounts.id
	) AS account_orders
	JOIN
	(
  		SELECT region.name, sales_reps.id
  		FROM sales_reps
  		JOIN region
  		ON sales_reps.region_id = region.id
	) AS sales_region
	ON sales_region.id = account_orders.sales_rep_id


~~~~~~~~~~~~~~~~
4) Provide a table that provides the region for each sales_rep along with their associated accounts. This time only for the Midwest region. Your final table should include three columns: the region name, the sales rep name, and the account name. Sort the accounts alphabetically (A-Z) according to account name.

::

	SELECT r.name region, s.name sales_rep, a.name account_name
	FROM region r
	JOIN sales_reps s
	ON s.region_id = r.id
	JOIN accounts a
	ON a.sales_rep_id = s.id
	WHERE r.name = 'Midwest'
	ORDER BY a.name ASC


~~~~~~~~~~~~~~~~
5) Provide a table that provides the region for each sales_rep along with their associated accounts. This time only for accounts where the sales rep has a first name starting with S and in the Midwest region. Your final table should include three columns: the region name, the sales rep name, and the account name. Sort the accounts alphabetically (A-Z) according to account name.

::

	SELECT r.name region, s.name sales_rep, a.name account_name
	FROM region r
	JOIN sales_reps s
	ON s.region_id = r.id
	JOIN accounts a
	ON a.sales_rep_id = s.id
	WHERE r.name = 'Midwest' AND s.name LIKE 'S%'
	ORDER BY a.name ASC


~~~~~~~~~~~~~~~~
6) Provide the name for each region for every order, as well as the account name and the unit price they paid (total_amt_usd/total) for the order. However, you should only provide the results if the standard order quantity exceeds 100. Your final table should have 3 columns: region name, account name, and unit price. In order to avoid a division by zero error, adding .01 to the denominator here is helpful total_amt_usd/(total+0.01).

::

	SELECT r.name region, a.name account_name, (o.total_amt_usd / (o.total + 0.01)) unit_price
	FROM region r
	JOIN sales_reps s
	ON s.region_id = r.id
	JOIN accounts a
	ON a.sales_rep_id = s.id
	JOIN orders o
	ON a.id = o.account_id
	WHERE o.standard_qty > 100


~~~~~~~~~~~~~~~~
7) Provide the name for each region for every order, as well as the account name and the unit price they paid (total_amt_usd/total) for the order. However, you should only provide the results if the standard order quantity exceeds 100 and the poster order quantity exceeds 50. Your final table should have 3 columns: region name, account name, and unit price. Sort for the largest unit price first. In order to avoid a division by zero error, adding .01 to the denominator here is helpful (total_amt_usd/(total+0.01).

::

	SELECT r.name region, a.name account_name, (o.total_amt_usd / (o.total + 0.01)) unit_price
	FROM region r
	JOIN sales_reps s
	ON s.region_id = r.id
	JOIN accounts a
	ON a.sales_rep_id = s.id
	JOIN orders o
	ON a.id = o.account_id
	WHERE o.standard_qty > 100 AND poster_qty > 50
	ORDER BY unit_price DESC

~~~~~~~~~~~~~~~~
8) What are the different channels used by account id 1001? Your final table should have only 2 columns: account name and the different channels. You can try SELECT DISTINCT to narrow down the results to only the unique values.

::

	SELECT DISTINCT a.name, w.channel
	FROM accounts a
	JOIN web_events w
	ON a.id = w.account_id
	WHERE a.id = '1001'


9) Find all the orders that occurred in 2015. Your final table should have 4 columns: occurred_at, account name, order total, and order total_amt_usd.


SELECT o.occurred_at, a.name account_name, o.total, o.total_amt_usd
FROM orders o
JOIN accounts a
ON a.id = o.account_id
WHERE o.occurred_at BETWEEN ' 2015-01-01' AND ' 2016-01-01'
ORDER BY o.total_amt_usd DESC



