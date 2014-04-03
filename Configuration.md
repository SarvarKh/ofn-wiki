**THIS IS A WORK-IN-PROGRESS.**

## Provisioning
Support for using [ansible](http://www.ansible.com/home) is being developed.

## Environment

App-specific configuration settings can be defined in `config/initializers/open_food_network.rb`. This can be copied from `config/initializers/open_food_network.rb.example`.

```ruby
OpenFoodNetwork.config do |config|
  config.country = 'au'
end
```
Settings can be accessed as `OpenFoodNetwork.config.country`.

## Seed data
Data files are selected based on the a directory matching the country configuration, e.g.

```
/db/default/spree/uk
  /states.yml
  /suburbs.yml
```

## OFN configuration data

### Products taxonomy
Defined in:
`/lib/seed_data`

## Sample data

Sample data includes:
* addresses
* enterprises
* users
* fees
* payment methods
* products

YAML files for this data are defined in sub-directories to group them. This could be by country but also might be other test configurations for different scenarios:
```
/lib/seed_data
  /au
  /uk
  /usa
```