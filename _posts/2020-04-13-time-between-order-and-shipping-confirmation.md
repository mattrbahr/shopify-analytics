---
layout: post
title: How Fast Do You Ship Your Orders?
author: matt_bahr
date: '2020-04-13 12:00:00'
categories: sql
intro_paragraph: >
  What is the time between order created and shipping confirmation sent?
description: A SQL query anlyzing the time between when an order is placed and when it's shipped.
---

This week’s analysis looks at the time difference between when an order is placed and when a customer receives a shipping confirmation email. In Shopify, shipping confirmation emails are automatically sent when a tracking number is added to an order. Fast order processing is, of course, a value proposition. The time of day in which an order is placed is also a factor.

I think it’s safe to assume, a lot of DTC brands are seeing a bit of an increase in their order processing times given the current state of affairs.

In this query, we'll be using the `created_at` timestamps from the **orders** table and **orders__fulfillments** table. To do this, we simply join these two tables on `order_id` and compare the timestamps. For easy analysis, I  grouped the averages by month to see month-over-month changes. 

For reference:<br>
**orders table** - master order record. Contains information about completed customer orders (does not include line-items).
<br>
**orders__fulfillments table** - created when fulfillment is created. Includes tracking number and carrier.

<script src="https://gist.github.com/mattrbahr/7ed95e950870b16096a413889d492b18.js"></script>

