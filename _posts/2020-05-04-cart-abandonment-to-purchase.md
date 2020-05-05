---
layout: post
title: What factors lead to higher checkout abandonment?
author: matt_bahr
date: '2020-05-05 12:00:00'
categories: sql
intro_paragraph: >
  A deep dive into checkout abandonment user behavior.
---

Across all ecommerce sites, cart abandonment rates are roughly 70%, meaning if 10 users add items to their cart, 7 leave without making a purchase. You can calculate your cart abandonment rate in Shopify on your analytics page by dividing your "Sessions converted" by "Added to Cart" numbers located within the "Online store conversion rate" module. There are countless reasons carts go abandoned, from price, shipping costs, to the lack of a less than stellar UI on mobile.

Although it's difficult to determine why a user abandoned a cart without explicitly asking them, we can gleam meaningful insights by analyzing the observational data around cart abandonment. In this post, we'll be diving into a few different SQL queries you can run to learn more about your site's cart abandonment and what actions you can take to reduce it. We'll cover:

**Is the user a new or returning customer?** Are new users more likely to abandoned carts? If so are your cart abandonment emails segmented by new vs returning customers? 

**How long does it take your customers to finally complete their order?** How can you reduce this? 

**What percentage of abandoned carts have discount codes? What % of final orders have discount codes?** Is the user looking for a code? Do you offer of a code in your cart abandonment email? In the data we analyzed, there was a 100% increase in discount code usage from checkout abandonment to order creation. 

These queries will use Shopify's `abandoned_checkouts` table schema––new entries are created once a user starts a checkout and enters an email or phone number. We won't be looking at cart abandonment emails––we'll save that for a later date. 

### Master Query
We'll use the results of the below as our master table to query against. This query simply joins abandoned checkouts with the most recent order placed after the user abandoned their cart. 

<script src="https://gist.github.com/mattrbahr/0ce764b73b55db94c97758b3987f58db.js"></script>

<br>To run the following queries, replace the code on lines 43-44 with the below.

#### Compare New Vs. Returning Cart Abandonment
Not all abandoned checkouts are created by new customers. In our example data set, 25% were returning, and were most likely looking for that push to get them to make their next purchase.
<script src="https://gist.github.com/mattrbahr/c3f5e0d811fd21a2cd0cdb13154d40f6.js"></script> 

Output: 
<table style="border-collapse:collapse;border-spacing:0" class="tg"><tr><th style="border-color:inherit;border-style:solid;border-width:1px;font-family:inherit, sans-serif;font-size:14px;font-weight:normal;overflow:hidden;padding:10px 5px;text-align:left;vertical-align:top;word-break:normal">count</th><th style="border-color:black;border-style:solid;border-width:1px;font-family:inherit, sans-serif;font-size:14px;font-weight:normal;overflow:hidden;padding:10px 5px;text-align:left;vertical-align:top;word-break:normal">new_customers</th><th style="border-color:black;border-style:solid;border-width:1px;font-family:inherit, sans-serif;font-size:14px;font-weight:normal;overflow:hidden;padding:10px 5px;text-align:left;vertical-align:top;word-break:normal">percent_new</th></tr><tr><td style="border-color:black;border-style:solid;border-width:1px;font-family:inherit, sans-serif;font-size:14px;overflow:hidden;padding:10px 5px;text-align:right;vertical-align:top;word-break:normal">2801</td><td style="border-color:black;border-style:solid;border-width:1px;font-family:inherit, sans-serif;font-size:14px;overflow:hidden;padding:10px 5px;text-align:right;vertical-align:top;word-break:normal">2139</td><td style="border-color:black;border-style:solid;border-width:1px;font-family:inherit, sans-serif;font-size:14px;overflow:hidden;padding:10px 5px;text-align:right;vertical-align:top;word-break:normal">.76</td></tr>
</table>
<br>

#### How long does it take a user to convert?
In our initial query, I added an `hours_dif` column to calculate the amount of hours between the abandonment and the order. To start, I would suggest plotting your data to see visual representation. Next, you can use the below code to create a histogram dividing your data into 5 buckets. You can remove outliers by adding `AND hours_dif < 336` to line 37. This will only query orders placed within 336 hours (14-days) of a cart abandonment.
<script src="https://gist.github.com/mattrbahr/7933d496b3c3e980ec04024ee7ea963d.js"></script>

Output: 
<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{border-color:black;border-style:solid;border-width:1px;font-family:inherit, sans-serif;font-size:14px;
  overflow:hidden;padding:10px 5px;word-break:normal;}
.tg th{border-color:black;border-style:solid;border-width:1px;font-family:inherit, sans-serif;font-size:14px;
  font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:top}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-0pky">bucket</th>
    <th class="tg-0lax">range</th>
    <th class="tg-0lax">freq</th>
  </tr>
  <tr>
    <td class="tg-0lax">1</td>
    <td class="tg-0lax">[0,82)</td>
    <td class="tg-0lax">2102</td>
  </tr>
  <tr>
    <td class="tg-0lax">2</td>
    <td class="tg-0lax">[83,165)</td>
    <td class="tg-0lax">136</td>
  </tr>
  <tr>
    <td class="tg-0lax">3</td>
    <td class="tg-0lax">[165,247)</td>
    <td class="tg-0lax">63</td>
  </tr>
  <tr>
    <td class="tg-0lax">4</td>
    <td class="tg-0lax">[250,329)</td>
    <td class="tg-0lax">52</td>
  </tr>
  <tr>
    <td class="tg-0lax">5</td>
    <td class="tg-0lax">[330,331)</td>
    <td class="tg-0lax">1</td>
  </tr>
</table>
<br>

#### What percentage of abandoned carts have discount codes? What % of final orders have discount codes?
By analyzing the below, we found there was a 100% increase in discount code usage from cart abandonment to final checkout. 
<script src="https://gist.github.com/mattrbahr/2d0341d59357393b70be2474066da6bf.js"></script>

Output: 

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
  overflow:hidden;padding:10px 5px;word-break:normal;}
.tg th{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
  font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg">
  <tr>
    <th class="tg-0lax">carts</th>
    <th class="tg-0lax">checkouts_with_disc</th>
    <th class="tg-0lax">perc_checkout_w_disc</th>
    <th class="tg-0lax">orders_with_disc</th>
    <th class="tg-0lax">perc_order_w_disc</th>
  </tr>
  <tr>
    <td class="tg-0lax">2355</td>
    <td class="tg-0lax">635</td>
    <td class="tg-0lax">.26</td>
    <td class="tg-0lax">1284</td>
    <td class="tg-0lax">.54</td>
  </tr>
</table>


<br>

#### Other questions you can solve with this query:
* Do higher AOVs lead to higher abandonment?
* Do certain products lead to higher cart abandonment?
* Is payment gateway a factor in abandonment?
