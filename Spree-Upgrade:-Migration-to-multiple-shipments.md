What follows is the the agreement reached to deal with [split shipments](https://guides.spreecommerce.org/release_notes/spree_2_0_0.html#split-shipments), feature introduced by Spree 2.0.

We explain how OFN will adapt to this new whole area of Spree's data model.

## New data model

![Split shipments diagram](https://github.com/openfoodfoundation/openfoodnetwork/wiki/split_shipments_diagram.jpg)

This is the Spree's 2.0 data model area related to shipments. What used to be simple got split into multiple tables.

### Single shipment per order

Although Spree now enables having multiple shipments per order we are not ready to embrace this new feature and change our app to fully support it. We only want to do the minimum it takes to get to a supported version of Spree and we're still struggling to find the resources to do so.

So to make OFN work with this new underlying database schema while not adding any new features **the goal is to have a single shipment per order and hide the support for multiple ones from the user**. To do that we need to:

* Have only a single shipment per order.
* Remove any added UI that allows users to deal with multiple shipments.
* Stick to our current UI as much as possible.
* Provide the required customization on top of Spree to ensure just a shipment is created.
* Make calls to `order.shipping_method` go through `order.shipments.first.shipping_method`.

So this part of the data model will then look like:

![Split shipments OFN diagram](https://github.com/openfoodfoundation/openfoodnetwork/wiki/split_shipments_ofn_diagram.jpg)

Note, highlighted in green, how from OFN we'll only allow a shipment per order. To know more about the changes related to `StockLocation` check [Variant's count_on_hand migration](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Variant%27s-count_on_hand-migration).
