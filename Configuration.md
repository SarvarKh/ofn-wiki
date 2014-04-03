**THIS IS A WORK-IN-PROGRESS.**

## Provisioning
Support for using [ansible](http://www.ansible.com/home) is in progress.

## Environment

`config/initializers/open_food_network.rb` defines app-specific configuration settings. This should be copied from `config/initializers/open_food_network.rb.example`.

```ruby
OpenFoodNetwork.config do |config|
  config.country = 'au'
end
```
Settings can be accessed as `OpenFoodNetwork.config.country`.

## Seed data
Data files are selected based on the a directory matching the country configuration, e.g.

```
/db/default
