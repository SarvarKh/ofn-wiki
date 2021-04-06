The Subscriptions module is particularly hard to test manually because there are time-based processes: when the Order Cycle opens the orders in a subscription are _placed_ and when the Order Cycle closes the orders in a subscription are _confirmed_.

To make this easier for devs and testers with access to the server where they are testing, here are some commands to be executed in server terminal or in the rails console that will make repeat testing in different conditions much faster.

You'll need to set up a subscription in forehand - see [the dedicated section](https://guide.openfoodnetwork.org/basic-features/subscriptions#set-up-subscriptions-step-by-step-guide) on our user guide.

DO NOT DO THIS IN PRODUCTION

**Procedure 1** (recommended)
- Login to the server
- Go to the app folder for example "cd app/openfoodnetwork/current"

Edit the Order Cycle and take the Order Cycle ID from the URL.

- on the console, run:

`bundle exec rake ofn:subs:test:repeat_placement_job`

We should be prompt to insert the Order Cycle ID from the corresponding Schedule/Subscription. This command opens the OC (if closed) and runs the subscription placement job.

After this:
- run the command below and insert the same Order Cycle ID:

`bundle exec rake ofn:subs:test:force_confirmation_job`

This should confirm the subscription job.

These commands should create and confirm the associated orders, and trigger the respective emails.

Further reading:
- [Subscription related Emails](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Emails)
- [This pull-request](https://github.com/openfoodfoundation/openfoodnetwork/pull/6291)



**Procedure 2** (deprecated):

Open console:
- Login to the server
- Go to the app folder for example "cd app/openfoodnetwork/current"
- Open rails console with "bundle exec rails c"

Setup Order Cycle, Schedule and Subscription for an Order Cycle that starts in the future.
The subscription start date should be in the past so that orders are generated.
Edit the Order Cycle and take the Order Cycle ID from the URL.
Edit the subscription and take the subscription ID from the URL.


Type these commands to place subs orders (beginning of the OC):
- open the Order Cycle (replace 2 with the OC ID below)

`OrderCycle.find_by_id(2).update_attribute(:orders_open_at, Time.now - 1000)`

- reset proxy order (remove existing order) - (replace 3 with the subscription ID below)

`ProxyOrder.find_by_subscription_id(3).update_attributes(order_id: nil, confirmed_at: nil, placed_at: nil)`

`SubscriptionPlacementJob.new.perform`

Check emails for the placement of the order....

Type these commands to confirm subs orders (end of the OC):
- close orde cycle

`OrderCycle.find_by_id(2).update_attribute :orders_close_at, Time.now - 1000`

- Confirm Job

`SubscriptionConfirmJob.new.perform`


With these commands you should be able to:
- open the OC, run the placement job and receive the order placement email immediately
- close the OC, run the confirm job and receive the order confirmation email immediately
