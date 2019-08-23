---
title: Manual Processing (Credit Card)
author: AndreiMaz
uid: user-guide/configuring/settingup/payments/methods/manual-processing
---

# Manual Processing (Credit Card)

This is a special gateway that allows all orders to be successfully entered on the site, but it does NOT charge the customer or make any calls to any live gateway. It is recommended to use this payment method if you want to perform one of the following:

* Process all orders offline
* Process them manually via another back-office system
* Test the site end-to-end before going live
    
    ![manualprocessing](_static/manual-processing/manualprocessing.png)

* Define how the payment will be displayed in the after **checkout mark of payment** as field

* Define **the additional fee** for using this method
* Define whether **to apply a percentage additional fee** to the order. If not enabled, a fixed value is used.