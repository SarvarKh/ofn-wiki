This page describes Open Food Network's API.

### API DOCUMENTATION
We use swagger and json-api to document the OFN API. You can see the documentation here:
https://app.swaggerhub.com/apis/Open-Food-Network/Open-Food-Network-API/0.2

#### Authorization

See also (Authorization)[Authorization]
To access the API, you need an API key. This can be found in the admin
backend at the right of the edit user page. That can be passed with the
`X-Spree-Token` request header, or the `token` query parameter.
The user api key can be found in the OFN backoffice: click on the menu Users, select the user that will use the API, see "API KEY".
If you are not an admin of the instance you are trying to access, you will need to request your API token to an admin of that instance.

You can also fetch your API key in the html content of some pages in the BackOffice.
- Login, go to the BackOffice and load the Products page
- Open dev tools
- Search for "SpreeApiKey" in the html content: that will be your API key
 
#### Basic Access Example
Using the command line, you can use curl to get some data from the OFN API.

List all the products you can see in that particular instance:

DOMAIN can be for example http://openfoodnetwork.org.uk

API KEY can be for example 41eae622fabbc8ce3f51d5b986b87bcb94498618fe52895e

> curl -X GET "http://openfoodnetwork.org.uk/api/products/bulk_products" -H "X-Spree-Token: 41eae622fabbc8ce3f51d5b986b87bcb94498618fe" -H "accept: application/json"

OR

> curl -X GET "http://openfoodnetwork.org.uk/api/products/bulk_products?token=41eae622fabbc8ce3f51d5b986b87bcb94498618fe52895e" -H "accept: application/json"

### API DEVELOPMENT
If you want to improve/develop the OFN API have a look at [the API development wiki page](https://github.com/openfoodfoundation/openfoodnetwork/wiki/API-Development).
