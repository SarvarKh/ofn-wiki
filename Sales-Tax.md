## Background

We're modelling Australian GST and UK VAT.
Item prices, some fees and shipping fees are inclusive of tax.
We want to charge the correct tax, show this value at the checkout, and report total sales tax charged  to the enterprise manager.


## What we need on top of Spree's tax system

Spree's tax system ( https://guides.spreecommerce.com/developer/taxation.html ) can model GST and VAT for a single shopfront. However, it's missing several features that we need, described below.


### Per-enterprise taxation

Some enterprises will be registered for GST, some will not.
Some of these will share products from the same producer, so differentiating by product is not sufficient.
We need to be able to enable/disable GST inclusivity per-enterprise.
This won't change the prices, just whether GST is recorded as included in the price.



### Recording tax charged on shipping

Spree has a config option to choose whether tax is charged on shipping.
This tax is price-inclusive.
@Matt-Yorkley has made a branch where the shipping tax rate is configurable - `Matt-Yorkley-reports_vat`.
However, when an order is placed, the sales tax charged does not include that charged on the shipping,
and no record is made in the order that sales tax was charged. We need both of these things.
Additionally, the sales tax report should pull the shipping tax from this recorded figure and not re-calculate it at report-time.


### Tax on fees

Individual items and whole orders are charged a variety of fees, categorised as admin, sales, packing and transport fees.
I think that we need to charge sales tax on some of these fees, particularly fees that apply to the whole order.
I think we also need to charge tax on any fee on an item that itself attracts tax.
