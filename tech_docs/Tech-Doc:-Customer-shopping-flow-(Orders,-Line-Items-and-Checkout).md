This page is WIP for Luis - during the next week at least.

This page describes the flow of pages and the implementation details of the buying process in the frontoffice. The pages described include everything that touches Orders and Checkout in the frontoffice.
The pages described so far are: enterprise shop with list of products of a given shop, edit cart page cart and checkout.

### Enterprise shop with list of products
In this page the user can see a list of products/variants and edit the quantity of a given variant. If the quantity is != zero, the variant is included in the order as a line_item.

OFN Route '/:id/shop', to: 'enterprises#shop', as: 'enterprise_shop'

Controller EnterprisesController

View views/enterprises/shops

The list of products is rendered in views/shop/products/form and the list of products is fetched through ajax call to /shop/products implemented in ShopController.products rendered by ProductsRenderer with Api::ProductSerializer.

In the first visit to this page, a new empty order (without line_items) is created and stored in DB. This happens with current_order(true) this happens in EnterprisesController filter check_stock_levels and in ShopController filter set_order_cycle.
On the client, a session id cookie identifies the server side session that holds the order id and details with variants up to date that are stored in the DB.

Each time the user changes a quantity on this page, it's triggered a call to /orders/populate with the variants and respective quantities. This triggers the update of the order in the DB.
Example payload sent to /orders/populate {"variants":{"8":{"quantity":1,"max_quantity":null},"10":{"quantity":3,"max_quantity":null}}}

This [Spree Product article](https://guides.spreecommerce.org/developer/products.html) is important to read to undertand how products and variants work in Spree. In OFN, we have [inventory and variant_overrides](https://community.openfoodnetwork.org/t/variant-overrides-hub-can-override-stock-level-and-price-on-a-variant/31) on top.

Note: abandoned baskets do not affect stock fields in the DB, stock is not subtracted until the order is finalised.

### Edit Cart
In the Edit cart page the user can change quantities of the orders line items and also remove line items.

Spree Frontend Route get '/cart', :to => 'orders#edit', :as => :cart

Controller Spree::OrdersController.class_eval.edit

View views/spree/orders/edit (each item rendered in _bought.html.haml)

No ajax calls are made if user changes quantities. If quantities are changed and user clicks on update button, the form is submitted (the form url is the cart endpoint) with form-data containing the new quantities.
A PUT is issued to /cart with something like this:
   order[line_items_attributes][0][quantity]: 2
   order[line_items_attributes][0][id]: 5

Route put '/cart', :to => 'orders#update', :as => :update_cart

Controller Spree::OrdersController.class_eval.update (this renders the html cart)

A click on the delete line item icon issues the same call to cart update with line item quantity set to zero.

### Checkout
In this page, the user is able to add different order details like shipment, payment, etc. The user can no longer change the line items of the order.

OFN Route get '/checkout', :to => 'checkout#edit' , :as => :checkout

Controller CheckoutController (extends Spree::CheckoutController)

View /checkout/edit

Form submit issues a PUT request with the order to CheckoutController#update

### Other pages
Other possible usages of Order and Checkout are:
- backoffice pages
- embedded_shopfront

# Controllers structure
OFN
- /controllers/
    - LineItemsController < BaseController
    - /controllers/ CheckoutController < Spree::CheckoutController
    - /admin
        - Admin::BulkLineItemsController < Spree::Admin::BaseController
    - /open_food_network
        - OpenFoodNetwork::CartController < ApplicationController
    - /spree
        - checkout_controller_decorator >> Spree::CheckoutController.class_eval
        - orders_controller_decorator >> Spree::OrdersController.class_eval
        - /admin
            - line_items_controller_decorator >> Spree::Admin::LineItemsController.class_eval
            - orders_controller_decorator >> Spree::Admin::OrdersController.class_eval
        - /api
            - line_items_controller_decorator >> Spree::Api::LineItemsController.class_eval
            - orders_controller_decorator >> Spree::Api::OrdersController.class_eval (file with a comment only)
- and these interesting views:
    - /open_food_network
        - cart — rabl
        - line_items — rabl
        - orders — rabl

SPREE
- /api/app/controllers
    - /spree/api
        - Spree::Api::CheckoutsController
        - Spree::Api::LineItemsController (class_eval in OFN)
        - Spree::Api::OrdersController (class_eval in OFN)
- /frontend/app/controllers
    - /spree
        - Spree::CheckoutController (extended and class_eval in OFN)
        - Spree::OrdersController (class_eval in OFN)
- /backend/app/controllers
    - /spree/admin
        - Spree::Admin::LineItemsController (class_eval in OFN)
        - Spree::Admin::OrdersController (class_eval in OFN)

# Database structure

This is a simplified version of the data model:
![_](https://github.com/openfoodfoundation/openfoodnetwork/wiki/tech_docs/checkout_order_data_model.jpg)
