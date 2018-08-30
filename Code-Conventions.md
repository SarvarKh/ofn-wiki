This pages lists conventions that are not verified by [Rubocop](http://batsov.com/rubocop/) but we agree to follow.

# Ruby

### Prefer `public_send` over `send`

As stated in the Ruby Style Guide:

> Prefer public_send over send so as not to circumvent private/protected visibility.

See https://github.com/rubocop-hq/ruby-style-guide#prefer-public-send

# CSS

### Prefer % to vh
For positioning use % values, not viewport-units (vh, etc) (viewport-units are not supported by all the browsers we want to support, see [here](https://caniuse.com/#feat=viewport-units))

# Tests

### Prefer `to have_not` as a feature matcher style (instead of `to_not have`)
Reference: Capybara finders section of [this blog post](https://blog.codeship.com/faster-rails-tests/) for more details.
