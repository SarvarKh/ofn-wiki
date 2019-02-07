# Automated Testing Gotchas

## Multiple consecutive spaces in HTML

Multiple consecutive spaces in HTML are rendered as a single space in the browser, and consequently also processed that way by `has_text?` and `has_content?` methods in Capybara. But they retain the original number of spaces when using `contains()` in CSS selectors and when inspecting strings in Javascript and Ruby.

This can cause format-related intermittent spec failures, such as when dealing with space-padded strings. An example is the day of month when `Date` is in `:long` format:

```ruby
# Two spaces before day of month
Date.new(2019, 1, 2).to_formatted_s(:long)
# => "January  2, 2019"

# One space before day of month
Date.new(2019, 1, 20).to_formatted_s(:long)
# => "January 20, 2019"
```

## Design tests to pass regardless of time of execution

A test may be run at any time of day, any day of month, and any calendar month. Make sure that the test will always pass.

Some things to watch out for:

* `now + 1.hour` can still be part of today, or it can be tomorrow.
* `today - 1.day` can be in the current calendar month, or last month
* `today + 1.day` can be part of this calendar year, or next year

If needed, you may also use `Timecop` to mock the time at which the test thinks it is running.