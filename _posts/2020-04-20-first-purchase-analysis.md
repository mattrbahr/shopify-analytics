---
layout: post
title: Repeat Rate By First Product Purchased
author: matt_bahr
date: '2020-04-20 12:00:00'
categories: sql
intro_paragraph: >
  What products lead to the highest customer repeat rate?
---

At EnquireLabs, we often talk about the *birth of the customer* and how important that first order is. In this query, we'll be looking at what items customers buy in that first order and how that compares to their repeat rate. 

In some occurrences, it may be more lucrative to market a lower-priced, higher-conversion product with a track record of nurturing stronger LTV, rather than push a higher-priced item up front. This query should help you make that decision.

This query uses the **orders table** and **orders__line_items table**. The output is a list of all SKUs and the customer repeat rate percentage if that SKU was in the customer's first order.

<script src="https://gist.github.com/mattrbahr/44f8e0880bb4b7e63815555996b0931f.js"></script>