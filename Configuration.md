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

## Goals for importing data:
* db agnostic
* be able to read in different data sets so that:
    * different countries can load data applicable to them (currently the system loads Australia-specific data)
    * different demos can be run (showing different scenarios, setups; for different audiences)
    * different use-cases can be represented
      * for development and testing
* initially, will be done via command-line or rake task; will assume the user has enough knowledge to do that (later versions might have a UI; see below)

The core complexity is handling referential integrity of the database tables (ensuring that each entry that should refer to another does so with a valid id (e.g. a state always refers to a country, and the country it refers to exists in the country table)

Different gems handle it differently:
(TBD - list those that do referential integrity checks, those that don't)

Advantages of ignoring referential integrity during data import:  
* much quicker to import; much simpler to develop
Disadvantages of ignoring referential integrity during import: 
* if invalid data is entered, it may not be apparent immediately -- it may cause some failure of the system that may not obviously be traced to invalid data


### Open Questions:


Because some data is dependent on others, some data definitely has to be read in/exist before others.  ex:  The country has to exist before the states can be loaded; the states info has to exist before the suburbs can be loaded.
* what are the dependencies ?  What is the ordering?
  * how much do we have to worry about it? 
    * may depend on the the gems & classes we use  ?
    * if we let ActiveRecord do validating, then the dependency checking is handled for us during validation


Where should the data be kept?  What directory should be the standard/convention?
* some gems and systems use "db/seed"  [TBD need reference [AE/wsd])
* some use lib/ xxx (TBD - need reference [AE/wsd])
* we just need to make a decision to start with
  * perhaps the directory is passed in as an option, but having a convention for where it 'should' live will keep the system neat and clean(er)


Should the file names be standardized?  **Yes, at least initially.** [PM & AE/wsd 2 Apr 14]
* Rails/Fixtures (ActiveRecord::....) assumes that the file has the same name as the table it will be imported into.
  * We will adopt that standard.
     * potentially, a future version could allow for different file names that could be specified (e.g. given as  an option)


Where are the lines drawn between "initial bootstrap data," "seed data," and "sample data" ?
* no matter the category, it all should be able to be read in 
  * perhaps the initial bootstrap data is a config file, and everything else is YML? (or maybe the config file points to YML needed?)
  * Spree will dictate some of this
  * "initial bootstrap data" = the absolute minimum amount of information required to start the system
    * the default country and/or the country to use ?
  * are the differences between "seed data" and "sample data" conventions that we define, or is there some technical or domain-driven reason?



### Misc notes (not yet organized; noted here so the thoughts won't be lost)

* existing spree_country table includes the 3 letter ISO-3166-1 (alpha-3) abbrv. for the country (see http://en.wikipedia.org/wiki/ISO_3166-1_alpha-3)

* Ashley will write up some notes about what she's learned so far about the different gems and classes that could be used, including the approaches and assumptions they use,  and other issues she's come across so far.

* Will lib/tasks/dev.rake be completely replaced by this?


### Ideas for later versions:

*  work with different file formats in addition to YML (CSV, JSON, etc.).  
    * CSV = allows users to work with the data in spreadsheets -- tools that non-tech people might be comfortable with
    *  JSON = more secure than YML (can't inject code as can be done with YML))
    * should be developed now so that this could be possible later

* would be good to be able to generate empty CSV files based on the current db schema -- so that users have some kind of structure they can fill in.  (a sort of simple template for each table/file needed)
    * dump data from current db into YAML/CSV.  Good not only to keep historical snapshots during dev/testing/demos, etc. -- also so that the data can be used as a starting place for creating a different data set.
* output to a directory that is given as an option
  * how is this different from rake db:schema:dump ? (is certainly related...)
      * should be db agnostic



----------------------------------


