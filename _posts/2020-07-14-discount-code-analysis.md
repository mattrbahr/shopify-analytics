---
layout: post
title: How do discounts affect AOV and LTV?
author: matt_bahr
date: '2020-06-07 12:00:00'
categories: sql
intro_paragraph: >
  Do users who use discount codes on their first purchase spend more over time?
description: A SQL query to understand discount code usage.
---

Discount codes. Love or hate them, they're part of nearly every DTC brand's acquisition strategy. 

Train customers to expect discount codes, and that's what they'll do... wait until sale, thus erroding your margins and further reducing brand equity. A few questions every DTC founder/marker needs to ask: how often are we running promotions? Is it an evergreen strategy? Do the ends justify the means? 

In general, discount codes are quite useful for a few reasons:
- &#8226;  Incentivizing users to purchase
- &#8226;  Increasing on-site email capture
- &#8226;  Tracking channel attribution

<br>
The third point is super important if you're using discount codes in your attribution models. By having different codes for each platform (Facebook, Snap, Google, etc), you'll get a solid data point to reconcile self-reported platfrom ROI. We'll dive into that one in another post. 

The first step in this analsyis involves building out a simple table answering some general questions around discount code usage; what percentage of orders have discount codes? How does AOV differ if a code is used? We'll use the following query to output this table.

<script src="https://gist.github.com/mattrbahr/3350d5722644b7084cb9b456bb8e01c3.js"></script>

The output will look like the below. Using this table, we can then use a BI tool to plot this data over time to understand general trends around code usage. 

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{border-color:black;border-style:solid;border-width:1px;font-size:14px;
  overflow:hidden;padding:10px 5px;word-break:normal;}
.tg th{border-color:black;border-style:solid;border-width:1px;font-size:14px;
  font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-0lax">date</th>
    <th class="tg-0lax">no_disc_aov</th>
    <th class="tg-0lax">disc_aov</th>
    <th class="tg-0lax">perc_disc</th>
    <th class="tg-0lax">no_disc_aov_new</th>
    <th class="tg-0lax">disc_new_aov</th>
    <th class="tg-0lax">perc_disc_new</th>
  </tr>
  <tr>
    <td class="tg-0lax">2020-06</td>
    <td class="tg-0lax">$224.23</td>
    <td class="tg-0lax">$178.98</td>
    <td class="tg-0lax">0.34</td>
    <td class="tg-0lax">$214.20</td>
    <td class="tg-0lax">$147.02</td>
    <td class="tg-0lax">.37</td>
  </tr>
  <tr>
    <td class="tg-0lax">2020-05</td>
    <td class="tg-0lax">$229.21</td>
    <td class="tg-0lax">$172.82</td>
    <td class="tg-0lax">0.41</td>
    <td class="tg-0lax">$221.28</td>
    <td class="tg-0lax">$157.21</td>
    <td class="tg-0lax">.35</td>
  </tr>
</table><br>

The nexts data point we'll analyze is the difference in LTV from customers who use discount codes and those who don't. To do this, we'll create a simple cohort table based on first_order_month and if a discount code was used, then compare LTV between the two groups. In the below example, we can see that discount doesn't necessarily lead to higher LTVs. 

<script src="https://gist.github.com/mattrbahr/f3cda4ce90479740cfa89c8ce4341a98.js"></script>

The output of the above query is the following cohort table. Discounting, of course, drives short term revenue increases, but in the long-run may train customers to wait for discounts, ultimately reducing LTV per customer. 

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{border-color:black;border-style:solid;border-width:1px;font-size:14px;
  overflow:hidden;padding:10px 5px;word-break:normal;}
.tg th{border-color:black;border-style:solid;border-width:1px;font-size:14px;
  font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-0lax">cohort_date</th>
    <th class="tg-0lax">first_order_disc</th>
    <th class="tg-0lax">total_spend</th>
  </tr>
  <tr>
    <td class="tg-0lax">2019-01</td>
    <td class="tg-0lax">discount</td>
    <td class="tg-0lax">$172,830</td>
  </tr>
  <tr>
    <td class="tg-0lax">2019-01</td>
    <td class="tg-0lax">no_discount</td>
    <td class="tg-0lax">$329,102</td>
  </tr>
  <tr>
    <td class="tg-0lax">2019-02</td>
    <td class="tg-0lax">discount</td>
    <td class="tg-0lax">$161,032</td>
  </tr>
  <tr>
    <td class="tg-0lax">2019-02</td>
    <td class="tg-0lax">no_discount</td>
    <td class="tg-0lax">$379,998</td>
  </tr>
</table><br>



