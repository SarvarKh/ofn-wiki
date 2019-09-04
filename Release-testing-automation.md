This document is the outcome of https://github.com/openfoodfoundation/openfoodnetwork/issues/4127 and describes what level of automation has, if any, each test case of the manual release testing in our current automated test suite. This is based on the [Testing Handbook v2](https://docs.google.com/document/d/15EdXKb4B3BS8gekD6_9PDdPTZsjUCAVS2011zRf55lM/edit#heading=h.alpdtdl2tc74).

## First steps

### Check signing up for a customer or a producer

Covered in: spec/features/consumer/authentication_spec.rb:47

It goes into too much detail. The code branches should go into the model test.

### Check that the producer account is correctly displayed on the map

Can't find any test. Not even controller tests. Probably because it only renders
the view for the index action and the enterprises are injected later on.

### Complete your enterprise setting and check that both payment method and shipping method work for stripe and paypal

This needs clarification. What exactly is "compelte your enterprise settings"? checking payment and shipping methods work I assume means placing an order with them?

#### Stripe

spec/requests/checkout/stripe_connect_spec.rb: worth reviewing the mocks are up to date.
`#set_order` needs to be reviewed as it uses allow_any_instance.

Covered in: spec/features/consumer/shopping/checkout_spec.rb:160

#### PayPal

spec/requests/checkout/paypal_spec.rb: It misses some layers of the stack by manually creating the payment and not
going through the controller. This covers the integration with the PayPal API
though. If it passes is because the requests was successful.

### Products: create, edit (add an image for example), duplicate (check that image is also duplicated), delete

Creation covered in: spec/features/admin/products_spec.rb
Duplication covered in: spec/features/admin/bulk_product_update_spec.rb:539
Image deletion covered in: spec/features/admin/products_spec.rb:207
Image addition and update covered in:
spec/controllers/api/product_images_controller_spec.rb

### Can you create on OC? Does it show in front office? Can you duplicate / edit / delete it? If you delete products do they still appear in the shopfront?

Edit OC covered in: spec/features/admin/order_cycles_spec.rb:273,416,491,512,799
Creation of OC covered in: spec/features/admin/order_cycles_spec.rb:705
Cloning OC covered in: spec/features/admin/order_cycles_spec.rb:574,826
Simplified UI for enterprises selling their produce covered in: spec/features/admin/order_cycles_spec.rb:972

Probably some of these tests go into too much detail.

Not sure about "If you delete products do they still appear in the shopfront?"
needs more investigation.

## Checkout

### Select an open shop and add item to the cart

Covered in: spec/features/consumer/shopping/shopping_spec.rb:289

### Press continue

### See your cart, but go back “continue shopping” in order to add another product

### Delete one of the first product you’ve added on the cart and change the quantity on another one

Covered in: spec/features/consumer/shopping/shopping_spec.rb:276

### Proceed to checkout, and login

Login covered in: spec/features/consumer/authentication.rb
Login at checkout covered in: spec/features/consumer/shopping/checkout_auth_spec.rb

Not sure about a test covering the "proceed to checkout" button works.

### Select delivery and cash payment option (repeat for stripe, paypal and cash)

Shipping methods covered in: spec/features/consumer/shopping/checkout_spec.rb:247
Payment methods covered in: spec/features/consumer/shopping/checkout_spec.rb:317

Check above for Stripe and PayPal integration tests.

### Check you get a confirmation and a confirmation email

Covered in: spec/features/consumer/shopping/checkout_spec.rb:148,317

We don't check we actually get the email I believe.

### Check your order is appearing correctly in /admin/orders

Covered in: spec/features/admin/orders_spec.rb

We don't check it's the outcome of a checkout but it's fine.

### Order for producer: can you see it? Can you filter on the page? Can you ship it? Can you capture payments? Can you create an order as an admin?

### Customers : add a customer and edit its credentials afterwards

## Bulk Order Management

### Make several order for an ongoing OC, but for the same product
### Login with super admin (and/or the producer / shop where you put the orders)
### Go to /admin/orders/bulk_management
### Are you able to change weight and was the order total recalculated accordingly?
### Are you able to change quantity and was the order total recalculated accordingly ?
### Are you able to cancel an order
### Are you able to add columns?
### Are you able to search for a product?
### Are you able to change the dates and see results accordingly?

## Subscriptions

### Can enable from superadmin?

### Can create schedule?

Covered in: spec/features/admin/schedules_spec.rb:21

### Can trigger subs?

### Get emails?

### Out of stock messaging works?

### Editing sub, pausing sub?

Covered in: spec/features/admin/subscriptions_spec.rb:28

## Inventory

### Override price
### Override stock levels
### Check that override don't have an effect on the producer stock level
### Reset stock levels

## Reports

### Orders and fulfillment reports
### Packing reports
### Customers
### Xero
### Sales tax

## Other things

### Sitemap test

spec/features/consumer/sitemap_spec.rb should be route and view tests perhaps.
We don't need to boot a browser to check that.

### requests/shop_spec.rb

Isn't this duplicating existing tests?
