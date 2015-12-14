This page describes Open Food Network's API. It is based on Spree's API,
which has [extensive documentation](https://guides.spreecommerce.com/api/)
([source](https://github.com/spree/api.spreecommerce.com)). See also
[spree-api's routes.rb](https://github.com/spree/spree/blob/master/api/config/routes.rb).

The Open Food Network API is not finished. Entries that are ~~crossed out~~
are candidates for addition.


## Quick links

#### Spree

* [Products](https://guides.spreecommerce.com/api/products.html), their
  [properties](https://guides.spreecommerce.com/api/product_properties.html)
  and [Variants](https://guides.spreecommerce.com/api/variants.html);
* [Orders](https://guides.spreecommerce.com/api/orders.html),
  [Line items](https://guides.spreecommerce.com/api/line_items.html) and
  [Checkouts](https://guides.spreecommerce.com/api/checkouts.html);
* [Payments](https://guides.spreecommerce.com/api/payments.html),
  [Shipments](https://guides.spreecommerce.com/api/shipments.html) and
  [Return authorizations](https://guides.spreecommerce.com/api/return_authorizations.html);
* [Taxonomies](https://guides.spreecommerce.com/api/taxonomies.html),
  [Addresses](https://guides.spreecommerce.com/api/addresses.html),
  [Countries](https://guides.spreecommerce.com/api/countries.html) and
  [Zones](https://guides.spreecommerce.com/api/zones.html);
* [Stock Locations](https://guides.spreecommerce.com/api/stock_locations.html) and
  [Stock movements](https://guides.spreecommerce.com/api/stock_movements.html).

#### Open Food Network

* [Enterprises](#enterprises)
* [Order cycles](#order-cycles)
* The [_managed_ scope](#additional-scopes) for
  Products and Orders,
  Enterprises and
  Order cycles;
* A [_soft\_delete_ action](#soft-delete) on Products and Variants;

## Authorization

To access the API, you need an API key. This can be found in the admin
backend at the right of the edit user page. That can be passed with the
`X-Spree-Token` header, or the `token` parameter.
[Read more](https://guides.spreecommerce.com/api/summary.html#making-an-api-call).

If you want to verify programmatically that you have a valid token, the
`/api/users/authorise_api` endpoint will return on success:

```json
{
    "success": "Use of API Authorised"
}
```


## Enterprises

* ~~`GET /api/enterprises` - returns a list of all enterprises~~
* `GET /api/enterprises/managed` - returns a list of enterprises the user manages
* ~~`GET /api/enterprises/accessible` - returns a list of the user has access to~~
* ~~`POST /api/enterprises` - creates a new enterprise~~
* ~~`PUT /api/enterprises/:id` - updates an enterprise~~
* ~~`DELETE /api/enterprises/:id` - deletes an enterprise~~

### `GET /api/enterprises/managed`

Returns a list of enterprises the user manages.

```
[
  {
    id: 1,
    name: "Enterprise 1"
  }
  ...
]
```


## Order cycles

### `GET /api/order_cycles/managed`

Returns the list of order cycles of enterprises the user manages.

```
[
  {
    id: ​1,
    name: "Winter season",
    first_order: "2015-11-01",
    last_order: "2015-12-04",
    suppliers: [
      {
        id: ​2,
        name: "Enterprise 2"
      }
    ],
    distributors: [
      {
        id: ​15,
        name: "The Basket"
      }
    ]
  },
  ...
]
```

### `GET /api/order_cycles/accessible`

Returns the list of order cycles that are accessible to the user.
The optional parameter `as` can either `distributor` or `producer`
and will filter the list to the user's distributor- or producer-role
respectively.


## Additional scopes

In Open Food Network users can manage enterprises, and it can be useful to retrieve
a list of enterprises, orders, order cycles and products that the user accessing
the api manages. Open Food Network includes an additional scope for those:

* `GET /api/products/managed`
* `GET /api/orders/managed`
* `GET /api/order_cycles/managed`
* `GET /api/enterprises/managed`

They work just like the regular index, except filter the results to those the user
can manage. The products index has some additional scopes:

* `GET /api/products/bulk_products` - products that require [group buying](http://openfoodnetwork.org/platform/user-guide/advanced-features/group-buy/)
* `GET /api/products/overridable` - **TODO** explain


## Soft delete

Products and variants can also be soft-deleted in Open Food Network.
This is done with the endpoints

**TODO** Explain what and why of soft-deletion.

* `DELETE /api/products/:product_id/soft_delete`
* `DELETE /api/products/:product_id/variants/:variant_id/soft-delete`
