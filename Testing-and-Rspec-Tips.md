Please add to this collection of tips and tutorials as you find them.

<h2>Conventions and Tips</h2>
<h5>Let</h5>
The "let" statements here are a particular syntax for defining data in rspec. For a description of how "let" statements work, see the RSpec docs here: https://www.relishapp.com/rspec/rspec-core/v/2-11/docs/helper-methods/let-and-let

<h5>Retry</h5>
Retries like here:
   feature "Cookies", js: true, retry: 3 do
should be a last resort when there is a good reason why we can't make the test deterministic. You should either remove the retry or add a comment explaining which part is failing (and why).

<h5>Sleep</h5>
We should avoid using <code>sleep</code> whenever possible. The RSpec <code>expect</code> syntax should wait until a condition is met by default.

<h5>Clearing app config changes from cache</h5>
When you change the App Config for your test, you need to reset the config or clear the cached values. Because while the database is reset, the cache is not.

     scenario "privacy policy link should..." do
       Spree::Config[:privacy_policy_url] = nil

The best solution so far for this is re-setting the value to it's original value: https://github.com/openfoodfoundation/openfoodnetwork/blob/c8397024e4a7f6e626983dc45d7f336441fd2f66/spec/features/consumer/shopping/cart_spec.rb#L106-L111

Or clear the cached value as in: https://github.com/openfoodfoundation/openfoodnetwork/blob/c8397024e4a7f6e626983dc45d7f336441fd2f66/spec/support/performance_helper.rb#L26-L30

In the example above we could do:
    original_setting = Spree::Config[:privacy_policy_url]
    Spree::Config[:privacy_policy_url] = test_setting
    do_the_test
    Spree::Config[:privacy_policy_url] = original_setting

OR

    Spree::Config[:privacy_policy_url] = test_setting
    do_the_test
    Rails.cache.delete_matched("spree/app_configuration/privacy_policy_url")


<h3>Running Specs</h3>
bundle exec rake db:test:prepare..........to prepare the db<br>
rspec spec/path/to/spec.rb:#lineNum.......will run the tests from a specific line<br>

Finished in 4.55 seconds
1 example, 0 failures

This is a good start, but we don't know whether our test is actually telling us anything yet. For example, an empty test also passes. We want to know that when our code is broken, the test will fail, otherwise it's not an informative test. To gain that confidence, we deliberately break our code, then re-run the test, and it should fail.

<h2>Convert specs to Rspec3</h2>
As you make changes to the test code base, you are encouraged to convert the syntax to the rspec 3 syntax. You can do that by using the transpec tool: https://github.com/yujinakayama/transpec

<h1>Debugging  tests</h1>

<h3>Running the browser</h3>

It may be useful to see a real browser running the whole spec. You can also Debug things directly on the browser with this. This is how you can do it:

- open spec/spec_helper.rb
- remove the word `headless`
- done! you can add a "byebug" entry in your spec to get the test execution to stop at some point

<h3>Viewing a screenshot</h3>

You can also view a screenshot for a specific moment in the test using Capybara's helper method `save_and_open_screenshot`. Add that line in any point, run the test, and you'll see a nice screenshot of exactly what Capybara is seeing at that point in the test execution, eg:

```
visit shops_path
save_and_open_screenshot
click_link "Entperprise 1"
```

Remember to delete the line before commiting!