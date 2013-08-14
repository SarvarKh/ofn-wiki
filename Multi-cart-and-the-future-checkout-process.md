Work has started to support a cart with multiple orders so that users can shop in more than one order cycle at a time and potentially, more than one distributor. The Cart contains multiple Spree::Order and a user of the front end should only have one current cart.

The concept of a cart which can have multiple simultaneous orders is very similar to how etsy manage buying products from multiple stores at once.

### Adding products to the cart
When a product is added to the cart, if there is an order that it is compatible with (an order for the same distributor and order cycle), then a new line item for that product will be added to that order. Otherwise a new order will be created for the new line item.

For example, if a customer adds Oranges from Northcote Bulk Foods Coop to be picked up on the 1/1/2013 
Cart then initially the cart will look like:

    Cart
      -- Order, 1/1/2013
         -- Line item - orange

If they then subsequently add bannanas product to the cart for Northcote Bulk Foods Coop, but to pick up on the 7/1/2013 then a second order will be created for the second pickup date.

    Cart
      -- Order, 1/1/2013
         -- Line item - orange
      -- Order, 7/1/2013
         -- Line item - bannanas

### Checking out an order from the cart
Choosing an order from the cart to checkout / purchase.

### Current status
