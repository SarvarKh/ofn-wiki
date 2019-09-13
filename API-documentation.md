This page describes Open Food Network's API (this page has been rebuilt in Sept 2019 as part of the issue #3001).

OFN API is no longer based on Spree's API and only includes OFN specific endpoints.

### API DOCUMENTATION
We use swagger and json-api to document the OFN API. You can see the documentation here:
https://app.swaggerhub.com/apis/luisramos0/the-open_food_network/0.1

#### Authorization

To access the API, you need an API key. This can be found in the admin
backend at the right of the edit user page. That can be passed with the
`X-Spree-Token` request header, or the `token` query parameter.
The user api key can be found in the OFN backoffice: click on the menu Users, select the user that will use the API, see "API KEY".

#### Some basic API implementation details for developers
The OFN API is implemented in a specific namespace /api. There are quite a few endpoints that are not under /api that render json, particularly in /admin (where they are mixed with html rendering endpoints). We do not consider these endpoints as part of the OFN API.

AMS is the serialization solution used in the OFN API, that means we stopped using rabl files in OFN.
In OFN codebase, there should be no API code under app/views because AMS serializers should all be under /app/serializers.

