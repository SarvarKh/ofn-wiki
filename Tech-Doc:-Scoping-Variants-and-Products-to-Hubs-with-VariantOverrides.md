In OFN we have [[Variant Overrides]] that "enable a stock level and price of a variant to be overridden for a specific Hub". At specific places we instrument the (mostly Spree) code with OpenFoodNetwork::ScopeVariantToHub and OpenFoodNetwork::ScopeVariantToHub to take into account hub level values (overides) instead of the standard ones at variant level.

In Spree v2, the variant.count_on_hand field was moved to a new table and entity called StockItem so that different stock levels can be managed for different stock locations. See more details about how we make OFN have a single stock locations [here](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Tech-Doc:-OFN-Data-Model---Stock-locations).

Currently in OFN v2 we adapt Variant class in VariantStock concern where we handle single stock location and thus **single stock item per variant** and then in ScopeVariantToHub we override the logic to keep using variant_overrides.count_on_hand value if it is an override that is being processed. This was a very simple solution to avoid having to create stock items for variant overrides!



### Pending questions

So far ScopeToHub code as been removed from app/models/spree/inventory_unit_decorator.rb:
https://github.com/openfoodfoundation/openfoodnetwork/pull/2574/commits/898e41acfb6ab7dada1eaa381a89910f3e94520b#diff-9f28cc0142720534ed585034f8a45745L1

and also from app/models/spree/line_item_decorator.rb:
https://github.com/openfoodfoundation/openfoodnetwork/pull/2574/commits/898e41acfb6ab7dada1eaa381a89910f3e94520b#diff-24e541b4ab015c71c58ee244899f4e7aL114

This monkey patching no longer works in Spree 2 and needs to be inserted somewhere else

This issue represents that work: https://github.com/openfoodfoundation/openfoodnetwork/issues/2559



