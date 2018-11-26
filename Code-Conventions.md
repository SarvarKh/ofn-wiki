This pages lists conventions that are not verified by [Rubocop](http://batsov.com/rubocop/) but we agree to follow.

# Database Migrations

See this guide: [[Database-migrations]]

# Ruby

### Prefer `public_send` over `send`

As stated in the Ruby Style Guide:

> Prefer public_send over send so as not to circumvent private/protected visibility.

See https://github.com/rubocop-hq/ruby-style-guide#prefer-public-send

# HTML Templates

### Use `j` when inserting an arbitrary Ruby string in a JS or Angular expression

`j` is an alias for [`escape_javascript`](https://api.rubyonrails.org/classes/ActionView/Helpers/JavaScriptHelper.html#method-i-escape_javascript).

When you have to insert an arbitrary Ruby string to a Javascript or Angular expression, this will ensure that characters are properly escaped and will not cause parsing issues or syntax errors.

```ruby
# Bad
# JS syntax error when description contains a single-quote
"{description: '#{enterprise.description}'}"

# Good
# Single-quote is escaped
"{description: '#{j enterprise.description}'}"
```

# CSS

### Prefer % to vh
For positioning use % values, not viewport-units (vh, etc) (viewport-units are not supported by all the browsers we want to support, see [here](https://caniuse.com/#feat=viewport-units))

# Tests

### Prefer `to have_not` as a feature matcher style (instead of `to_not have`)
Reference: Capybara finders section of [this blog post](https://blog.codeship.com/faster-rails-tests/) for more details.

### Always use I18n.t in spec assertions
In specs, when asserting text that is translatable, always assert with a call to I18n.t, never use the translated string in english.

Bad
`expect(json_response['errors']).to eq "Hm, something went wrong. No order cycle data found."`

Good
`expect(json_response['errors']).to eq I18n.t('admin.order_cycles.bulk_update.no_data')`

