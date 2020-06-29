---
layout: post
title: CAC:LTV Cohort Analysis Using Postgres
author: matt_bahr
date: '2020-05-18 12:00:00'
categories: sql
intro_paragraph: >
  A simple query to help understand ROI by cohort.
---
A cohort is a group of customers who share a common characteristic over a certain period of time. Typically, within ecommerce this common characteristic is month of first purchase, but it can be any variable you can segment your customers by (products purchased, last-click UTMs, subscribed to email list, etc). It's valuable for a multitude of reasons, most notably evaluating cohort profitability and understanding repeat purchase behavior.

### SaaS &#8800; Ecommerce
Cohort analysis was popularized in SaaS where variables like churn and retention are the lifeblood of the organization. These variables, although important are measured a bit differently in traditional ecommerce. This is even true if you have a subscription offering as variables like product consumption rate are a factor. In short, slapping a SaaS Cohort Analysis chart onto your ecom business, probably won't do you much good. 

### Keep It Simple
To get started, keep it simple and analyze cohort first month revenue compared to cohort future revenue. Cohort charts you've seen most likely display variables over months, for most ecommerce merchants this is data overload and may make it even harder to interpret your data. Analyzing current vs future state allows you to create a more digestible output that you can always analyze further.

The first step is creating creating a table with `order_date`, `customer_id`, `order_id`, and `subtotal_price`. If you're analyzing customer cohorts by purchase month and repeat purchase date, this is actually all you will need. 

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
    <th class="tg-0lax">order_date</th>
    <th class="tg-0lax">customer_id</th>
    <th class="tg-0lax">order_id</th>
    <th class="tg-0lax">subtotal_price</th>
  </tr>
  <tr>
    <td class="tg-0lax">10/1/2018</td>
    <td class="tg-0lax">126012</td>
    <td class="tg-0lax">193920</td>
    <td class="tg-0lax">$54.78</td>
  </tr>
  <tr>
    <td class="tg-0lax">12/13/2018</td>
    <td class="tg-0lax">126012</td>
    <td class="tg-0lax">293029</td>
    <td class="tg-0lax">$54.78</td>
  </tr>
  <tr>
    <td class="tg-0lax">3/5/2019</td>
    <td class="tg-0lax">126012</td>
    <td class="tg-0lax">392033</td>
    <td class="tg-0lax">$54.78</td>
  </tr>
</table><br>

### CAC:LTV Query

This query utilizes revenue per cohort month, subsequent cohort revenue, and marketing spend to calculate CAC:LTV ratios per cohort. This query is helpful when analyzing the long-term profitability of a given month's marketing spend. The first `SELECT` statement generates a simple table similar to the above with `order_id`, `customer_id`, `order_date`, and `order_total` (we use `order_total` minus `returned_amount` to calculate net revenue per order. This is important for stores with high return rates). We then introduce a new table `marketing_spend` that contains monthly spend data and compare it to monthly revenue aggregrates (in the future we'll wire this directly to your Facebook and Google Ads accounts). We use a 12 month purchase window, to edit this change the interval on line 62.

<script src="https://gist.github.com/mattrbahr/3fc73ca5de224062eb7a0012f7f1ebe0.js"></script>

This query uses data from a `marketing_spend` table. If don't know how to add a table to you db, [use this query](https://gist.github.com/mattrbahr/8f4a2bdd5dcdbe93343b15c6a01647d7) and insert your spend data. 

Output:
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
    <th class="tg-0lax">cohort</th>
    <th class="tg-0lax">cohort_month_rev</th>
    <th class="tg-0lax">cohort_month_roi</th>
    <th class="tg-0lax">future_rev</th>
    <th class="tg-0lax">total_cohort_rev</th>
    <th class="tg-0lax">12_month_roi</th>
  </tr>
  <tr>
    <td class="tg-0lax">2019-07</td>
    <td class="tg-0lax">$220,006</td>
    <td class="tg-0lax">2.3</td>
    <td class="tg-0lax">$110,203</td>
    <td class="tg-0lax">$330,209</td>
    <td class="tg-0lax">6.1</td>
  </tr>
  <tr>
    <td class="tg-0lax">2019-08</td>
    <td class="tg-0lax">$260,825</td>
    <td class="tg-0lax">3.7</td>
    <td class="tg-0lax">$92,023</td>
    <td class="tg-0lax">$352,848</td>
    <td class="tg-0lax">5.2</td>
  </tr>
  <tr>
    <td class="tg-0lax">2019-09</td>
    <td class="tg-0lax">$221,788</td>
    <td class="tg-0lax">3.4</td>
    <td class="tg-0lax">$82,032</td>
    <td class="tg-0lax">$303,820</td>
    <td class="tg-0lax">4.9</td>
  </tr>
  <tr>
    <td class="tg-0lax">2019-10</td>
    <td class="tg-0lax">$110,203</td>
    <td class="tg-0lax">3.7</td>
    <td class="tg-0lax">$365,034</td>
    <td class="tg-0lax">$413,243</td>
    <td class="tg-0lax">4.2</td>
  </tr>
</table>
<br>

To note, this query uses aggregated spend and revenue. If you're using [EnquireLabs](https://enquirelabs.com) for survey attribution, you can take this a step further and analyze CAC:LTV on a per customer basis for more accurate results. 
