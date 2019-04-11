Here we explain how OFN adapts to this entity of Spree's data model.

## New data model

![Split shipments diagram](https://github.com/openfoodfoundation/openfoodnetwork/wiki/split_shipments_diagram.jpg)

This is the Spree's 2.0 data model area related to stock locations, tightly related to [shipments](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Tech-Doc:-OFN-Data-Model---Single-shipment-per-Order) as well. In v2, Spree adds the notion of location related to stock into the data model.

### Single 'default' stock location per instance

Although Spree allows creating stock locations we are not ready yet to make us of it as that would mean fitting all the new UI and logic into our existing codebase.

To make OFN work with this part of the database schema we **created a single 'default' stock location' per OFN instance**, meaning that each OFN database only has one location called `default`, with which all shipments will be associated automatically. So we:

* Have only a `default` stock location per instance.
* Removed any added UI that may allow users to create/read/update/delete stock locations.
* Provide the required customization on top of Spree so that all shipments use the same stock location.

So this part of the data model looks like this:

![Split shipments OFN diagram](https://github.com/openfoodfoundation/openfoodnetwork/wiki/split_shipments_ofn_diagram.jpg)

Note, highlighted in green, how from OFN we only have a single stock location which will be shared by all shipments in the instance.
