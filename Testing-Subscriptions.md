The Subscriptions module is particularly hard to test manually because there are time-based processes: when the Order Cycle opens the orders in a subscription are _placed_ and when the Order Cycle closes the orders in a subscription are _confirmed_.

To make this easier for devs and testers with access to the server where they are testing, here are some commands to be executed in the rails console that will make repeat testing in different conditions much faster:

DO NOT DO THIS IN PRODUCTION

Open console:
- Login to the server
- Go to the app folder for example "cd app/openfoodnetwork/current"
- Open rails console with "bundle exec rails c"

Setup Order Cycle, Schedule and Subscription for an Order Cycle that starts in the future.
Edit the Order Cycle and take the Order Cycle ID from the URL.
Edit the subscription and take the subscription ID from the URL.


Type these commands to place subs orders (beginning of the OC):
- open the Order Cycle (replace 2 with the OC ID below)
`OrderCycle.find_by_id(2).update_attribute(:orders_close_at, Time.now + 1000)`

- reset proxy order (remove existing order) - (replace 3 with the subscription ID below)
`ProxyOrder.find_by_subscription_id(3).update_attributes(order_id: nil, confirmed_at: nil, placed_at: nil)`
`SubscriptionPlacementJob.new.perform`

Check emails for the placement of the order....

Type these commands to confirm subs orders (end of the OC):
- close orde cycle
`OrderCycle.find_by_id(2).update_attribute :orders_close_at, Time.now - 1000`

- Confirm Job
`SubscriptionConfirmJob.new.perform (edited) `