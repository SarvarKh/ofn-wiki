## Authentication in OFN

#### Email + password in a browser session
Most shop managers and customers on OFN only ever authenticate this way. OFN uses [Devise](https://github.com/heartcombo/devise) for authentication and [CanCanCan](https://github.com/CanCanCommunity/cancancan) for authorization.

Being logged in to OFN also means that you can make API v0 requests (or the front end code can make them on your behalf). For example, if you're on the admin products page, the page loads and then does a GET to `/api/v0/products/bulk_products.json` to load the products. 

#### API v0 Authentication via API Key
For API v0 endpoints that require auth, we check the request's `X-Spree-Token` header. Each user record in the database has an associated API key in the `spree_api_key` field. If the token passed in the header matches a user's key, we authenticate the request as coming from that user.

A user can obtain their personal API key by inspecting the source of any admin page while they are logged in. Alternatively, a user who has permissions to edit users on the instance (a superadmin) is able to view API keys for all users on the admin/users/edit page. 

Some API v0 endpoints do not require any auth. If a request is made to an endpoint without an API key, we create a new `Spree::User` (but don't persist it) with no authorization roles attached to it and use that as the `current_api_user`. If the endpoint requires auth, the request will fail, otherwise it returns the requested data.

#### Stripe Authorization
OFN is set up as an OAuth client of Stripe; when a shop owner wants to authorize OFN to take payments through Stripe, we use the OAuth flow (included with the Stripe gem) to do so. 