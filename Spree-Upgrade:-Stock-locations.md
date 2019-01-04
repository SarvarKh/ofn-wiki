What follows is the the agreement reached to deal with [stock locations](https://guides.spreecommerce.org/release_notes/spree_2_0_0.html#split-shipments), feature introduced by Spree 2.0.

We explain how OFN will adapt to this new entity of Spree's data model.

## New data model

![Split shipments diagram](https://github.com/openfoodfoundation/openfoodnetwork/wiki/split_shipments_diagram.jpg)

This is the Spree's 2.0 data model area related to stock locations, tightly related to [shipments](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Spree-Upgrade:-Migration-to-multiple-shipments) as well. Now Spree adds the notion of location related to stock into the data model.

### Single 'default' stock location per instance

Although Spree allows creating stock locations we are not ready yet to make us of it as that would mean fitting all the new UI and logic into our existing codebase. We only want to do the minimum it takes to use a supported version of Spree while choosing ourselves our feature set rather than Spree imposing those.

To make OFN work with this new part of the database schema we will *create a single 'default' stock location' per OFN instance*, meaning that each OFN database we only have one location called `default`, with which all shipments will be associated automatically. So to do that we need to:

* Have only a `default` stock location per instance.
* Remove any added UI that may allow users to create/read/update/delete stock locations.
* Stick to our current UI as much as possible.
* Provide the rquired customization on top of Spree so that all shipments use the same stock location.

So this part of the data model will then look like:

![Split shipments OFN diagram](https://github.com/openfoodfoundation/openfoodnetwork/wiki/split_shipments_ofn_diagram.jpg)

Note, highlighted in green, how from OFN we'll only have a single stock location which will be shared by all shipments in the instance. To know more about the changes related to `Shipment` check [Migration to multiple shipments](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Spree-Upgrade:-Migration-to-multiple-shipments).
