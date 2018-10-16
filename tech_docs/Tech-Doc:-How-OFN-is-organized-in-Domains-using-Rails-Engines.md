In OFN we use Rails Engines to organize our application in separate Business domains.
The discussion originated in the 2018 Global Gathering in Barcelona and continued [here](https://community.openfoodnetwork.org/t/breaking-ofn-down-into-domains/1377) in discourse.

## Business Domains
For now, we started by extracting the Cookies feature into the new Web domain ([see the PR here](https://github.com/openfoodfoundation/openfoodnetwork/pull/2521)). The vision in terms of domains is the following (although we will adapt and change as we go):

* FrontOffice _ **Web** ___ Content CMS for the ecommerce website (including homepage, landing pages, map, menu and footer)
* FrontOffice _ **Shop** ______ Product Search and Display features on the ecommerce website
* FrontOffice _ **Checkout** _ Cart and Checkout flow features (includes User account and User registration)
* BackOffice _ **Catalog** ___ Product/Inventory management (including Order Cycle management)
* BackOffice _ **Orders** ____ Order Management System: Orders, packing and delivery management (including Invoicing and Accounting)
* BackOffice _ **Entities** ___ Entity Management: Enterprises and Customers management (CRM)

### FrontOffice _ **Web**
Content CMS for the ecommerce website (including homepage, landing pages, map, menu and footer)

![_](https://github.com/openfoodfoundation/openfoodnetwork/wiki/tech_docs/domains_web.png)

Example API endpoints:
- /home - homepage
- /home/consumer - homepage for consumers
- /home/producer - homepage for producers
- /menu - app menu
- /map - map

### FrontOffice _ **Shop**
Product Search and Display features on the ecommerce website

![_](https://github.com/openfoodfoundation/openfoodnetwork/wiki/tech_docs/domains_shop.png)

Example API endpoints:
- /shops - list of shops
- /shops/enterprise/1 - shop for enterprise 1 
- /shops/enterprise/1/product/10 - product 10 details
- /shops/enterprise/1/product?description=carrot - search for carrots in enterprise 1

### FrontOffice _ **Checkout**
Cart and Checkout flow features (includes User account and User registration)

![_](https://github.com/openfoodfoundation/openfoodnetwork/wiki/tech_docs/domains_checkout.png)

Example API endpoints:
- /cart - add, display and change the cart
- /checkout - collect details and submit purchases
- /user - user account and associated endpoints
- /user/registration - user registration process and data

### BackOffice _ **Catalog**
Product/Inventory management (including Order Cycle management)

![_](https://github.com/openfoodfoundation/openfoodnetwork/wiki/tech_docs/domains_catalog.png)

Example API endpoints:
- /product - similar to /shop but with more management capabilities, for example, bulk update actions

### BackOffice _ **Orders**
Order Management System: Orders, packing and delivery management (including Invoicing and Accounting)

![_](https://github.com/openfoodfoundation/openfoodnetwork/wiki/tech_docs/domains_orders.png)

Example API endpoints:
- /orders - list orders and bulk actions
- /orders/1 - view and change order details

### BackOffice _ **Entities**
Entity Management: Enterprises and Customers management (CRM)

![_](https://github.com/openfoodfoundation/openfoodnetwork/wiki/tech_docs/domains_entities.png)

Example API endpoints:
- /users
- /enterprises
- /customers
- /groups

## Rails Engines
Rails Engines can be considered miniature applications, for more info see the [Rails Guides entry for Rails Engines](https://guides.rubyonrails.org/engines.html).

The OFN main application is under /app and each engine's code is under /engines/<engine_name>/app.
The migration to engines will consist basically of moving files from /app/<relative_file_path> to /engines/<engine_name>/app/<relative_file_path>.

Each engine and the main app will have different ways of accessing/depending on (other) domains code:
* requiring another domain's modules and classes directly, for example, the main app FooterLinksHelper requires the Web domain CookiesConsent service simply by using `require 'web/cookies_consent'`
* the best way to depend on another engine is through its routes (ideally the API routes) - like in Spree, each domain's routes are mounted on the base url with `mount Web::Engine, :at => '/'`

## Technical structure
Each domain structure will mimic the existing structure on the main app. each domain will have it's on controllers, services, models, views, assets and serializers.

![_](https://github.com/openfoodfoundation/openfoodnetwork/wiki/tech_docs/domains_structure.png)

