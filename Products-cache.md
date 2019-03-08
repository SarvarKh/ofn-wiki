As the list of products to display in a shopfront for a particular order cycle and distributor is rather slow to fetch OFN caches it. Then, subsequent requests are served from cache, which is stored in Memcached in production.

## Cache refreshing

Cache "refreshing" is a unique OFN concept. Instead of caching the product list when a cache miss occurs, we instead "refresh" it by means of a background job. The responsible job class is [app/jobs/refresh_products_cache_job.rb].

Said job generates the entire JSON API response and writes it to the Rails cache. The hypothesis (I wasn't here when it was implemented) is that, as generating this response is highly inefficient and unaffordable to perform in a request's lifetime, a background job avoids this overhead and keeps it up to date.

This of course, comes at the expense of it depending on Delayed Job. If no worker is running, the cache will become stale.

If you want to dig deeper on how this JSON API response is generated, take a look at [lib/open_food_network/products_renderer.rb]. It's this class' output what the background job writes to the cache.

## Cache invalidation

The mechanism OFN uses to trigger this job is `after_save` callbacks from the models that invalidate the cache. These are:

* app/models/coordinator_fee.rb
* app/models/enterprise_fee.rb
* app/models/exchange.rb
* app/models/exchange_fee.rb
* app/models/inventory_item.rb
* app/models/order_cycle.rb
* app/models/producer_property.rb
* app/models/spree/classification_decorator.rb
* app/models/spree/image_decorator.rb
* app/models/spree/option_type_decorator.rb
* app/models/spree/option_value_decorator.rb
* app/models/spree/preference_decorator.rb
* app/models/spree/price_decorator.rb
* app/models/spree/product_decorator.rb
* app/models/spree/product_property_decorator.rb
* app/models/spree/property_decorator.rb
* app/models/spree/taxon_decorator.rb
* app/models/spree/variant_decorator.rb
* app/models/variant_override.rb

All of them end up calling the [ProductsCache] class, whose methods are responsible for invalidating the appropriate JSON API responses. While updating some models like order cycle may invalidate a single one of these cached products responses, some others like variant will need to invalidate all the ones where the variant is involved.

## Development

Remember that to actually use the cache in development mode you need to have a worker running so that the [RefreshProductsCacheJob] jobs can be processed. If not, the job will be enqueued but never processed. However, after the first one belonging to a distributor and order cycle pair is enqueued no other will be afterward since we check for their uniqueness in [ProductsCacheRefreshment]. You can boot a worker running `bundle exec rake jobs:work`.

Although doing so will enable cache writes reads also need to be enabled. The class responsible for that is [CachedProductsRenderer], which is called from [app/controllers/shop_controller.rb]. To enable them, replace `#use_cached_products?` implementation to return `true`.

:warning: *Note*: as the cache store that we have configured in development is `memory_store` you will run into issues. Said implementation stores the data into the process' memory and therefore, the writes performed by the delayed job worker won't be seen by the rails server's process reads.

## Troubleshooting

What follows is a description of the things to check when the cache is not working properly. You will notice that if the products listed in the shopfront do not match the ones in the order cycle page, for instance. Check [Cache invalidation](#cache-invalidation) to see the full list of models that might not be up to date in your shopfront.


### Delayed Job malfunctioning

Most of the times the root cause of the cache being stale is Delayed Job. This could go from it being down or the single delayed job worker in production being too busy. Until any precedent tasks are not fulfilled, the cache won't be refreshed.

#### Process

One way to check Delayed Job is working is by checking the running processes by executing the following

```
openfoodnetwork@scw-ecce22:$ ps axuf | grep delayed
openfoo+ 24339  1.8 13.4 1276352 275004 ?      Sl   feb12 618:04 delayed_job
```

you should see a similar output, always including `delayed_job`.

#### Logs

You can also check its logs but running

```
openfoodnetwork@scw-ecce22:$ tail -f log/delayed_job.log
2019-03-07T19:25:07+0000: [Worker(delayed_job host:scw-ecce22 pid:24339)] Job SubscriptionPlacementJob (id=251447) RUNNING
2019-03-07T19:25:07+0000: [Worker(delayed_job host:scw-ecce22 pid:24339)] Job SubscriptionPlacementJob (id=251447) COMPLETED after 0.0459
2019-03-07T19:25:07+0000: [Worker(delayed_job host:scw-ecce22 pid:24339)] 3 jobs processed at 13.9109 j/s, 0 failed
```

Note this will output any log lines as they happen.

#### Cache settings page

Lastly, navigating to `/admin/cache_settings/edit` you will see a debugging UI that shows the difference between what is stored in the cache and the actual products JSON API responses for each distributor and order cycle.

It is not very advisable to check it in production since the amount of distributor and order cycle is potentially very large, which can increase a lot your servers load and likely time out.

### Memcached

Although we have never experienced any problem with Memcached and it is very unlikely to be the root cause of a problem, being the actual cache store, it needs to be taken into account.

[app/jobs/refresh_products_cache_job.rb]: https://github.com/openfoodfoundation/openfoodnetwork/blob/master/app/jobs/refresh_products_cache_job.rb
[lib/open_food_network/products_renderer.rb]: https://github.com/openfoodfoundation/openfoodnetwork/blob/master/lib/open_food_network/products_renderer.rb
[ProductsCache]: https://github.com/openfoodfoundation/openfoodnetwork/blob/master/lib/open_food_network/products_cache.rb
[RefreshProductsCacheJob]: https://github.com/openfoodfoundation/openfoodnetwork/blob/master/app/jobs/refresh_products_cache_job.rb
[ProductsCacheRefreshment]: https://github.com/openfoodfoundation/openfoodnetwork/blob/f12568601677c93b12be5b379616d2fc05763de7/lib/open_food_network/products_cache_refreshment.rb#L21
[CachedProductsRenderer]: https://github.com/openfoodfoundation/openfoodnetwork/blob/master/lib/open_food_network/cached_products_renderer.rb
[app/controllers/shop_controller.rb]: https://github.com/openfoodfoundation/openfoodnetwork/blob/master/app/controllers/shop_controller.rb
