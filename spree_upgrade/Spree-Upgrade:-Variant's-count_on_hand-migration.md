Spree data migration https://github.com/spree/spree/commit/2c48bfbaad98b8af35a5e95ba268a057c6973b47 by default would create only one Stock Location for all the Enterprises and create a Stock Item for each Variant.

In this way, we end up with all the stock information for all the Enterprises in one place.

To do that, as Spree v2.0.0 updates the frontend to allow for stock location creation, we should diverge and stick to our current frontend to hide such option. This way we could postpone the effort of making StockLocations work with our Enterprises.

* We'll need to remove the `STOCK MANAGEMENT` link from the product admin page.
* The stock location page will have to be inaccessible or removed somehow. Added
    in https://github.com/spree/spree/commit/129d27f9615d0614b5cfc64f57522d9352c325d1
* The migration will be run automatically and current variants will be associated with the `default` stock location.
* We'll have to develop ourselves the code that ensures new variants get associated with that `default` stock location as well, to comply with the new DB schema. The user won't be given the option to choose a stock location, for now, and Spree expects the user to do that, meaning that it won't associate it to `default` itself.

Regarding the data model, as a summary:

* Status Quo: Every variant has exactly one `count_on_hand` value.
* After Spree 2.0 migrations: Every variant has one `count_on_hand` value via `StockItem` at the `default` location. This data model is completely backward compatible and reversible.
* Possible with Spree 2.0: Creating more stock locations so that each variant can have a `count_on_hand` value at each stock location.

