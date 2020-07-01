---
layout: post
title: Are your customers only on the coasts? A query to help you measure. 
author: matt_bahr
date: '2020-06-30 12:00:00'
categories: sql
intro_paragraph: >
  We'll use entropy (information theory) to determine how "coastal" your brand is.
description: A SQL query customer distribution across America.
---

Brands that cater solely to the coasts and therefore lack a more even distribution across America typically have less upside. As a brand matures, can they take the success they've had in cities like NYC and LA and expand to a broader audience in middle America? This concept is quite important in understanding the intrinsic value of a DTC brand at any given time. Tim Armstrong's company, DTX, launched [Unbox](), a service built for the sole purpose of helping brands expand beyond major coastal cities. 

Historically, there has been no sole metric to help a brand owner see which way they’re trending–coastal or non-coastal. In this week's query, we'll take concepts from information theory to create a value that allows a brand owner or marketer to understand how "coastal" they are and if their sales are becoming more or less distributed across the country.

Information entropy, as wikipedia defines it, is "a basic quantity in information theory associated to any random variable, which can be interpreted as the average level of "information", "surprise", or "uncertainty" inherent in the variable's possible outcomes." In our case, entropy is a great property to use because 1 is maximally distributed, while 0 is totally uniform. Since our goal is to have orders be more distributed across states - we are always trying to get to 1. That makes sense. The symmetric property of the entropy function allows us to do that. 

Hypothetically, we can use other calculations like `probability_coastal/probability_noncoastal` and `probability_coastal/(probability_coastal+probability_noncoastal)`, but each have their shortcomings (i.e. divide by zero and optimizing for even distribution at .5). Entropy allows us to optimize for a more straightforward goal, 1. 

We often hear brands laud when they've shipped to all 50 states, but how distributed are they really? Let's find out. 

We'll start by creating a table containing the population of each state and if they’re coastal. I’ve simply gone through and manually labeled the states on each coast–you could also create your own labeling system if you’d like to measure a different variable (i.e east vs west). To add this data to your database simply run [this query](). We need population data to normalize our data set so we don’t over value states with greater or lesser populations (i.e. California compared to Wyoming). 

We’ll then join this data with our order history by month and calculate the normalized distribution of coastal vs. non-coastal orders. The output will look like the below:

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
    <th class="tg-0lax">probability_coastal</th>
    <th class="tg-0lax">probability_noncoastal</th>
  </tr>
  <tr>
    <td class="tg-0lax">2019-01</td>
    <td class="tg-0lax">0.3954958878</td>
    <td class="tg-0lax">0.6045041121</td>
  </tr>
  <tr>
    <td class="tg-0lax">2019-02</td>
    <td class="tg-0lax">0.3685794854</td>
    <td class="tg-0lax">0.6314205145</td>
  </tr>
  <tr>
    <td class="tg-0lax">2019-3</td>
    <td class="tg-0lax">0.4273078433</td>
    <td class="tg-0lax">0.5726921566</td>
  </tr>
</table><br>

We’ll then take this data and run it through the entropy function to get our monthly value. 
`-1*(probability_coastal * ln(probability_coastal) + probability_noncoastal * ln(probability_noncoastal)) entropy`

<script src="https://gist.github.com/mattrbahr/3a3e7b2195dfd8c13fcf2b793a73b179.js"></script>

The output of this table will look like the below.

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{border-color:black;border-style:solid;border-width:1px;font-size:14px;
  overflow:hidden;padding:10px 5px;word-break:normal;text-align:center;}
.tg th{border-color:black;border-style:solid;border-width:1px;font-size:14px;
  font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;text-align:center}
.tg .tg-0lax{text-align:center;vertical-align:top;}
</style>
<table class="tg">
 <tr>
    <th>date</th>
    <th>entropy</th>
  </tr>
  <tr>
    <td class="tg-0lax">2019-01</td>
    <td class="tg-0lax">0.623921333266955897226</td>
  </tr>
 <tr>
    <td class="tg-0lax">2019-02</td>
    <td class="tg-0lax">0.629322771340638607563</td>
  </tr>
 <tr>
    <td class="tg-0lax">2019-03</td>
    <td class="tg-0lax">0.643204368215507303963</td>
  </tr>
 <tr>
    <td class="tg-0lax">2019-04</td>
    <td class="tg-0lax">0.671933961048111597456</td>
  </tr>
 <tr>
  <td class="tg-0lax">2019-05</td>
  <td class="tg-0lax">0.679926560813717844681</td>
</tr>
 <tr>
  <td class="tg-0lax">2019-06</td>
  <td class="tg-0lax">0.663926000714458669676</td>
</tr>
 <tr>
    <td class="tg-0lax">2019-07</td>
    <td class="tg-0lax">0.701231553512111363341</td>
  </tr>
</table><br>



We can then chart this data to more easily see changes over time.
<center>
<iframe width="600" height="371" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/e/2PACX-1vQ0soiRxqiPmiY3MgWH-7COjN3K2OtKAaKmInLx671DAHpZfkJRC5yLwgcueGlHPzcGk5_F3rsJAqXo/pubchart?oid=1552290352&amp;format=interactive"></iframe></center>