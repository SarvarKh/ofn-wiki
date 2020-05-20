This page describes Open Food Network's API.

### API DOCUMENTATION
We use swagger and json-api to document the OFN API. You can see the documentation here:
https://app.swaggerhub.com/apis/luisramos0/the-open_food_network/0.1

#### Authorization

To access the API, you need an API key. This can be found in the admin
backend at the right of the edit user page. That can be passed with the
`X-Spree-Token` request header, or the `token` query parameter.
The user api key can be found in the OFN backoffice: click on the menu Users, select the user that will use the API, see "API KEY".

### API DEVELOPMENT
If you want to improve/develop the OFN API have a look at [the API development wiki page](https://github.com/openfoodfoundation/openfoodnetwork/wiki/API-Development).
