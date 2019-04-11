## Data model

![Split shipments diagram](https://github.com/openfoodfoundation/openfoodnetwork/wiki/split_shipments_diagram.jpg)

This is the Spree's 2.0 data model area related to shipments.

### Single shipment per order

Although Spree enables having multiple shipments per order we are not ready to embrace this new feature and change our app to fully support it.

So to make OFN work with this underlying database schema while not adding any new features **we have a single shipment per order and hide the support for multiple ones from the user**. To do that we:

* Have only a single shipment per order.
* Hide any added UI that allows users to deal with multiple shipments.
* Provide the required customization on top of Spree to ensure just a shipment is created.
* Make calls to `order.shipping_method` go through `order.shipments.first.shipping_method`.

So this part of the data model will then look like:

![Split shipments OFN diagram](https://github.com/openfoodfoundation/openfoodnetwork/wiki/split_shipments_ofn_diagram.jpg)

Note, highlighted in green, how from OFN we only allow a shipment per order.