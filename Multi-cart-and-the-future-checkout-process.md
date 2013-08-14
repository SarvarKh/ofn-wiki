Work has started to support a cart with multiple orders so that users can shop in more than one order cycle at a time and potentially, more than one distributor. The Cart contains multiple Spree::Order and a user of the front end should only have one current cart.

The concept of a cart which can have multiple simultaneous orders is very similar to how etsy manage buying products from multiple stores at once.

### Adding products to the cart
Basically, a multi cart is a cart that contains multiple orders which have not yet been checked out. Every time a user adds a product to their cart for a different order cycle (or distributor) it will create a new order with a new line item. The

### Checking out an order from the cart
Choosing an order from the cart to checkout / purchase.