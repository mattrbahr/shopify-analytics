---
layout: post
title: What Percentage is a SKU on a Customer's First Purchase Vs. Subsequent Purchase
author: matt_bahr
date: '2020-04-27 12:00:00'
categories: sql, retention
intro_paragraph: >
  A look into acquisition vs. retention SKUs. 
description: A SQL query analyzing what percentage a SKU is a first purchase vs. return purchase.
---

Assigning your products as acquisition vs retention SKUs is a great strategy to better align your org with what products to push during different stages of the customer lifecycle. 

For some brands like <a href="https://mybillie.com" target="_blank">Billie</a> or <a href="https://supply.co" target="_blank">Supply</a>, it’s quite clear that certains SKUs are acquisition SKUs (i.e. razor handles), and other SKUs are retention SKUs, razor blades. Dollar Shave Club famously sold a razor for a dollar and made up the lack of first order margin on subsequent purchases. For other non-consumable brands, this strategy falls short. 

This week, we break down how often a SKU is on a first purchase vs repeat purchase. Using the output of this query, you can optimize your marketing spend to push users to product detail pages that get people to convert. 


Output Columns:
	`SKU`
	`Average Sell Price`
	`1st Order Percentage`
	`2nd+ Order Percentage`
	`1st Order Count`
	`2nd+ Order Count`
	`Total`

<script src="https://gist.github.com/mattrbahr/09ec36a81df9a6161c774b8724842f85.js"></script>

If you'd like to see this broken out by order number, you can use the below query. 

<script src="https://gist.github.com/mattrbahr/28476f6b429091893e43e19a3c11551c.js"></script>

<br>

Thanks to [Fred Perrotta](https://twitter.com/fredperrotta) of [Tortuga](https://www.tortugabackpacks.com/) for the query suggestion. 