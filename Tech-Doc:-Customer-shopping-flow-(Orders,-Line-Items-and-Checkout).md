This page describes the flow of pages and the implementation details of the buying process in the frontoffice. The pages described include everything that touches Orders and Checkout in the frontoffice.
The pages described so far are: enterprise shop with list of products of a given shop, edit cart page cart and checkout.

### Enterprise shop with list of products
In this page the user can see a list of products/variants and edit the quantity of a given variant. If the quantity is != zero, the variant is included in the order as a line_item.

Route '/:id/shop', to: 'enterprises#shop', as: 'enterprise_shop'
View views/enterprises/shops

TODO: explain what controllers/views are involved in rendering the product/variant list and how the empty order is created in this process.

In the first visit to this page, a new empty order (without line_items) is created and stored in DB.
On the client, a session cookie holds the order_id and, on the server side, the session holds the order details with variants up to date that are stored in the DB.

Each time the user changes a quantity on this page, it's triggered a call to /orders/populate with the variants and respective quantities. This triggers the update of the order in the DB.
Example payload sent to /orders/populate {"variants":{"8":{"quantity":1,"max_quantity":null},"10":{"quantity":3,"max_quantity":null}}}

TODO: explain difference between line_items and finalised_line_items in currentOrder.
TODO: explain how on_hand works and how, for example, an abandoned basket affects the stock fields in the DB.

### Edit Cart
In the Edir cart page the user can change quantities of the orders line items and also remove line items.

TODO: add tech description

### Checkout
In this page, the user is able to add different order details like shipment, payment, etc. The user can no longer change the line items of the order.

TODO: add tech description

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
        - Spree::Api::LineItemsController
        - Spree::Api::OrdersController
- /frontend/app/controllers
    - /spree
        - Spree::CheckoutController
        - Spree::OrdersController
- /backend/app/controllers
    - /spree/admin
        - Spree::Admin::LineItemsController
        - Spree::Admin::OrdersController

# Database structure

TODO: upload diagram to wiki