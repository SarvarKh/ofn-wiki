The Checkout workflow is a state machine that makes orders transition between states (address, payment, etc). This is not seen from the UI because in OFN we have a single step checkout process but the code does make the workflow progress and different data processes happen as this workflow is executed.

What follows in this page is an analysis done to the checkout workflow in preparation for the upgrade to Spree 2-0. It may be useful in the future:

##### Order decorator

What changed in Spree::Order:

- We add some callbacks:
  - `update_distribution_charge!` whenever an order changes
  - `shipping_address_from_distributor` before validation. When delivered to a food host, the user doesn't enter a shipping address. So we fill in the address of the food host.
  - Update shipping and payment fees on complete orders.
- We change the state machine:
  - skip the confirm step, we don't use it
  - go to complete without conditions like required payment
  - **part of our copy of the state machine has changed in Spree, we need to update that**
  - We added `remove_variant` which got added and depricated in Spree. We need to use the OrderContents now.
  - We overrode `add_variant` which needs updating to use OrderContents. https://github.com/openfoodfoundation/openfoodnetwork/issues/2523
  - We overrode `process_payments!` which we don't need any more (is a copy of Spree 2): https://github.com/openfoodfoundation/openfoodnetwork/issues/2520


##### CheckoutController

`Spree::CheckoutController` has one action: `update`. We override it and do a few things differently:

- call `check_order_for_phantom_fees`
- call `redirect_to_paypal_express_form_if_needed`
- retry `order.next` up to three times in case of `StaleObjectError`
- display other errors than `payment_processing_failed`, e.g. errors reported by a payment gateway
- on error, reset the state machine and clear the shipping address
- set default billing and shipping address
- move some logic to `ResetOrderService`
- respond in JSON format

Spree has updated the following:

- `apply_coupon_code`: This doesn't require any change in our code.
- display other errors, similar to OFN, but not exactly: This doesn't require any change in our code.

##### Other customisations

We added enterprise fees and payment method fees. We have to make sure that they get updated properly, but this logic is independent of the checkout process.

#### Review of the checkout process

We don't use most of the state machine. When people check out, it's updating the order attributes, processing a payment if required and then redirecting to the order confirmation when complete. We don't use the address and delivery state and even the payment form is on the same page. But with the Spree logic in place, I don't see an easy way to simplify this. I have some ideas, but they would probably end up being quite laboursome:

- Don't use the state machine and implement our own logic.
- Implement our own Order model that has a Spree::Order for its functionality of calculating fees and creating shipments etc.
