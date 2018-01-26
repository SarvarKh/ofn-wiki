At OFN we run our long-running test suite in parallel in our CI, Travis.

To do that parallelization we use https://github.com/ArturT/knapsack. 

> Knapsack splits tests across CI nodes and makes sure that tests will run comparable time on each node.

> Parallel tests across CI server nodes based on each test file's time execution. Knapsack generates a test time execution report and uses it for future test runs.

In order for this to work, as stated in their docs:

> This report should be updated only after you add a lot of new slow tests or you change existing ones which causes a big time execution difference between nodes.

In any case, Knapsack prints a time offset warning at the end of the rspec results to let us know when it's a good time to regenerate the report.

## How to regenerate the report

Edit the [.travis.yml](https://github.com/openfoodfoundation/openfoodnetwork/blob/master/.travis.yml) file replacing the line that executes the test suite with:

```
"KNAPSACK_GENERATE_REPORT=true bundle exec rspec spec"
```

Please, double check [Knapsack's docs](https://github.com/ArturT/knapsack#common-step) as this might have changed since this wiki was written.

Once CI executes this, you'll see something like this in the tests output:

```
Knapsack report was generated. Preview:
{
  "spec/controllers/admin/accounts_and_billing_settings_controller_spec.rb": 5.231297254562378,
  "spec/controllers/admin/bulk_line_items_controller_spec.rb": 22.596107006072998,
  "spec/controllers/admin/business_model_configuration_controller_spec.rb": 0.2459728717803955,
  "spec/controllers/admin/column_preferences_controller_spec.rb": 0.20803070068359375,
  "spec/controllers/admin/customers_controller_spec.rb": 1.376739263534546,
(...)
```

Just copy the whole JSON and replace the whole [knapsack_rspec_report.json](https://github.com/openfoodfoundation/openfoodnetwork/blob/master/knapsack_rspec_report.json) with it.

Once you commit and push this change, Travis should have parallel builds more evenly balanced.

