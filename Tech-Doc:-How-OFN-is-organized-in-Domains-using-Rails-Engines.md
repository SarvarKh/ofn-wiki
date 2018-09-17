In OFN we use Rails Engines to organize our application in separate Business domains.
The discussion originated in the 2018 Global Gathering in Barcelona and continued [here](https://community.openfoodnetwork.org/t/breaking-ofn-down-into-domains/1377) in discourse.

### Business Domains
For now, we started by extracting the Cookies feature into the new Content domain ([see the PR here](https://github.com/openfoodfoundation/openfoodnetwork/pull/2521)). The vision in terms of domains is the following (although we will adapt and change as we go):

* FrontOffice _ **Content** ___ Content CMS for the ecommerce website
* FrontOffice _ **Shop** ______ Product Search and Display features on the ecommerce website
* FrontOffice _ **Checkout** _ Cart and Checkout flow features (includes User account and User registration)
* BackOffice _ **Catalog** ___ Product/Inventory management
* BackOffice _ **Orders** ____ Order Management System: Orders, packing and delivery management (includes Order Cycle management, Invoicing and Accounting)
* BackOffice _ **Entities** ___ Entity Management: Enterprises and Customers management (CRM)

#### Rails Engines
Rails Engines can be considered miniature applications, for more info see the [Rails Guides entry for Rails Engines](https://guides.rubyonrails.org/engines.html).

The OFN main application is under /app and each engine's code is under /engines/<engine_name>/app.
The migration to engines will consist basically of moving files from /app/<relative_file_path> to /engines/<engine_name>/app/<relative_file_path>.

Each engine and the main app will have different ways of accessing/depending on (other) domains code:
* requiring another domain's modules and classes directly, for example, the main app FooterLinksHelper requires the Content domain CookiesConsent service simply by using `require 'content/cookies_consent'`
* the best way to depend on another engine is through its routes (ideally the API routes) - like Spree, each domain's routes are mounted on the base url with `mount Content::Engine, :at => '/'`