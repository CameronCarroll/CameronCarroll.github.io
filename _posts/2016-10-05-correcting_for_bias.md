---
title: Correcting Data for Bias
category: engineering
layout: back
month_year: Fall 2016
---
Sources
---------
http://electronics.stackexchange.com/questions/66958/what-does-the-term-bias-mean
http://fluidsurveys.com/university/how-to-know-the-difference-between-error-and-bias/
http://www.statisticalengineering.com/Weibull/precision-bias.html

Bias vs Error
--------------
Error includes all the flaws in a result, including random and systematic errors. Bias includes only the systematic error, comes from imperfectly calibrated equipment or similar, 'correctable' factor.The data is gathered in a way that makes it systematically offset.

More generally, bias is offset. In electrical, a biased voltage is 'centered' at that 'operating point.' A 6V AC signal biased at +1V would range from -2V to 4V. (Often used with respect to diodes and other nonlinear components.)

How do we actually calculate bias?
--------------------------------------
![Calculations for bias](/images/lsf.png)

Source: Thanks Professor Torres.
It came from taking the derivative of some LSF stuff. Bias can be calculated from bias = -b / a
