Please add to this collection of tips and tutorials as you find them.

<h3>Online Tutorials</h3>
The "let" statements here are a particular syntax for defining data in rspec. For a description of how "let" statements work, see the RSpec docs here: https://www.relishapp.com/rspec/rspec-core/v/2-11/docs/helper-methods/let-and-let

<h3>Running Specs</h3>
bundle exec rake db:test:prepare..........to prepare the db<br>
rspec spec/path/to/spec.rb:#lineNum.......will run the tests from a specific line<br>

Finished in 4.55 seconds
1 example, 0 failures

This is a good start, but we don't know whether our test is actually telling us anything yet. For example, an empty test also passes. We want to know that when our code is broken, the test will fail, otherwise it's not an informative test. To gain that confidence, we deliberately break our code, then re-run the test, and it should fail.