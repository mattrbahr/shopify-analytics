---
layout: post
title: Top Universities
author: matt_bahr
date: '2020-04-12 12:00:00'
categories: sql
intro_paragraph: >
  Are a large bulk of your customers at university? Find out. 
---


{% highlight sql %}
-- this is a basic count of orders with .edu email addresses attached to them 
-- change table name on line 8

WITH uni_customers AS (
	SELECT DISTINCT 
		customer__email
	FROM
		shopify.orders 
	WHERE 
		customer__email iLIKE '%edu'
)

SELECT substring(customer__email from '@(.*)\.edu') AS uni, count(*)
FROM uni_customers
GROUP BY uni
ORDER BY count DESC


{% endhighlight %}