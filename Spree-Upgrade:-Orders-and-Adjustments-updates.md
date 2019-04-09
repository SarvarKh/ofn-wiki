Updating adjustments can be a bit tricky in Spree due to a web of callbacks. Adjustments can be in three different states (simplified): open, closed and finalised. An adjustment can be updated either open or closed.

But when an adjustment is associated to an order, and update triggers the after_save callback `update_adjustable` which calls `update!` on the order and that eventually triggers a reset of the adjustment unless it's closed. Here is the rough call trace:

1. `adjustment.update_attributes` triggers `adjustable.update!`.
2. `Order#update!` triggers the `Spree::OrderUpdater`.
3. The updater updates all shipments and adjustments.
4. `Adjustment#update!` doesn't do anything if the adjustment is closed.
5. When the adjustment is open, `originator.update_adjustment` is called.
6. `ShippingMethod#Adjustment` recalculates the amount and calls `adjustment.update_attribute_without_callbacks(:amount, compute_amount(calculable))`.

All this is Spree code. It's not something we accidentally caused with OFN overrides. So Spree defines this way:

- Open shipping adjustments are always recalculated -> can't be changed to a custom value.
- Closed shipping adjustments are not recalculated -> can be changed to a custom value.