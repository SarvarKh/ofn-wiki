This page contains important information if you are a dev planning to improve the OFN API in some way.

The OFN API is implemented in a specific namespace /api. There are quite a few endpoints that are not under /api that render json, particularly in /admin (where they are mixed with html rendering endpoints). We do not consider these endpoints as part of the OFN API.

AMS is the serialization solution used in the OFN API, that means we stopped using rabl files in OFN.
In OFN codebase, there should be no API code under app/views because AMS serializers should all be under /app/serializers.

### How to build a new API endpoint (with examples)

#### Routes
To build a new API endpoint, first, you need to check what endpoint or action you what to add.
There's a API specific routes file for API routes [here](https://github.com/openfoodfoundation/openfoodnetwork/blob/master/config/routes/api.rb).

We will use api/products/bulk_products as our "good" example, you can see this endpoint is a specific GET endpoint in the api routes file [here](https://github.com/openfoodfoundation/openfoodnetwork/blob/46353be9a37f7054485a2d83712c50d8066f995d/config/routes/api.rb#L5).

Some comments about this example api/products/bulk_products endpoint:
- this could be the default index endpoint in the main /products route but because it's returning "products the user can edit" and not "all products the user can see", it's acceptable to have it in a different url, in this case api/products/bulk_products
- the name bulk refers to the client page (bulk products edit page) that uses this endpoint, ideally this should not be the case, the endpoint should only refer to the data it returns, not its clients. In this case a better name would be for example api/products/editable.

#### Controllers
All API controllers are now under app/controllers/api [here](https://github.com/openfoodfoundation/openfoodnetwork/tree/master/app/controllers/api).

We can find our example bulk_products action in the api/products_controller.rb [here](https://github.com/openfoodfoundation/openfoodnetwork/blob/46353be9a37f7054485a2d83712c50d8066f995d/app/controllers/api/products_controller.rb#L51).

#### Common Actions
In this products controller you will also find typical actions implemented:
- show - GET /api/products/{product_id} - gets one product
- create - POST /api/products - creates a product with given details
- update - GET /api/products/{product_id} - updates a product with given details
- destroy - DELETE /api/products/{product_id} - deletes a product with given Id
- index (not present in this case) - GET /api/products - lists products

#### Serializers - Output Payloads
In order to create the results for your endpoint, you should use AMS Serializers, you can re-use existing ones, they are all under app/serializers/api [here](https://github.com/openfoodfoundation/openfoodnetwork/tree/master/app/serializers/api), or you can create new ones in the same folder.

Note about serializers usage: a lot of these serializers are used outside the API: they are used to render json data that is injected in the DOM of the pages rendered. For example, a list of enterprises is injected in the checkout page's DOM [here](https://github.com/openfoodfoundation/openfoodnetwork/blob/46353be9a37f7054485a2d83712c50d8066f995d/app/views/checkout/edit.html.haml#L5) using the Api::EnterpriseSerializer.

Note about the admin namespace: currently we have two namespaces app/serializers/api and app/serializers/api/admin. The api/admin folder contains serarializers for the admin side of the OFN app but it's not consistent... there's no admin namespace in the API itself and the API uses both serializers from serializers/api/ and serializers/api/admin...

A note about managing Serializers: we should keep the number of serializers to a minimum, make sure you really need a new serializer when you create one and that you cannot adapt an existing one to your needs. In some cases, you will need a specific serializer for the same entity, we dont have conventions for this yet but try to make these generic if possible, see for example enterprise_thin_serializer, the enterprise_serializer and the basic_enterprise_serializer. This is not simple but necessary for enterprises. There's a discussion about this challenge [here in discourse](https://community.openfoodnetwork.org/t/representing-resources-via-api/1302).

#### Pagination
In OFN we use [kaminari](https://github.com/kaminari/kaminari) for pagination.

You can check the [products/bulk_product pagination code](https://github.com/openfoodfoundation/openfoodnetwork/blob/46353be9a37f7054485a2d83712c50d8066f995d/app/controllers/api/products_controller.rb#L61) for reference.
Basically, the query string can include ?page=1&per_page=15
and the result object can be filtered with the .page and .per methods, see another example in [the orders index search](https://github.com/openfoodfoundation/openfoodnetwork/blob/46353be9a37f7054485a2d83712c50d8066f995d/app/services/search_orders.rb#L32).

The pagination data in the payload should follow [this structure](https://github.com/openfoodfoundation/openfoodnetwork/blob/46353be9a37f7054485a2d83712c50d8066f995d/app/services/search_orders.rb#L11).

#### Parameters - Input Payloads
For filtered lists we use [ransack](https://github.com/activerecord-hackery/ransack).
This search will be done on the index action typically.
The filters will be sent through the query parameters and encoded under the q parameter, like this example:
`?q%5Bbill_address_firstname_start%5D=Luis&q%5Bbill_address_lastname_start%5D=Ramos&q%5Bcompleted_at_not_null%5D=true&q%5Bemail_cont%5D=luisramos@mail.com&q%5Bs%5D=completed_at+desc`

This will enable us to simply filter results using ransack and params[:q] like [here](https://github.com/openfoodfoundation/openfoodnetwork/blob/46353be9a37f7054485a2d83712c50d8066f995d/app/services/search_orders.rb#L26).

### API development tips
All PRs that make a change to the API should also make a change to the swagger doc so that the documentation evolves with the code. To do this you can:
- use [swagger](https://app.swaggerhub.com/) to open the swagger doc: https://github.com/openfoodfoundation/openfoodnetwork/blob/master/swagger.yaml
- update the docs according to the changes in your PR
- export the changes to a file and include that new swagger.yml file with your changes in the PR    

