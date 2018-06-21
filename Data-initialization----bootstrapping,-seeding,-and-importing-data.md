This page is for collecting thoughts, questions, possibilities, and links for implementing data initialization.  It is part of the Developer Guide.

**THIS PAGE IS A WORK IN PROGRESS.**  Edit and add to it to record issues, questions, and decisions made. Be Bold!

***


Data Initialization covers the functionality for:
* loading the initial data required to get the system to run (the **boostrapping data**),
* importing and creating **seed data**,
* loading different sets of **sample data**


### Contents:

#### [Goals](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Data-initialization----bootstrapping,-seeding,-and-importing-data#goals-for-importing-data)
#### [Definitions](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Data-initialization----bootstrapping,-seeding,-and-importing-data#definitions)
#### [Open Questions](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Data-initialization----bootstrapping,-seeding,-and-importing-data#open-questions)
#### [Miscellaneous Notes (not yet organized)](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Data-initialization----bootstrapping,-seeding,-and-importing-data#misc-notes-not-yet-organized-noted-here-so-the-thoughts-wont-be-lost)
#### [Ideas for later versions](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Data-initialization----bootstrapping,-seeding,-and-importing-data#ideas-for-later-versions)

***

## Goals for importing data:
* db agnostic
* be able to read in different data sets so that:
    * different countries can load data applicable to them (currently the system loads Australia-specific data)
    * different demos can be run (showing different scenarios, setups; for different audiences)
    * different use-cases can be represented
      * for development and testing
* initially, will be done via command-line or rake task; will assume the user has enough knowledge to do that (later versions might have a UI; see below)

### Referential Integrity
**The core complexity is handling *referential integrity* of the database tables:**  ensuring that each entry that should refer to another does so with a valid id (e.g. a state always refers to a country, and the country it refers to exists in the country table).

**DECIDING WHETHER OR NOT TO DO REFERENTIAL INTEGRITY VALIDATION is the key to developing this functionality.**

[ActiveModel](http://guides.rubyonrails.org/v3.2.13/active_record_validations_callbacks.html#presence) does this when it does validation. Specifically, referential integrity is checked when an association has been declared with [validates ...  :presence => true](http://guides.rubyonrails.org/v3.2.13/active_record_validations_callbacks.html#presence), and validation is called during some action. Here's an example of a declaration (assume there is some ActiveModel for Order that has an orderId:
```
class LineItem < ActiveRecord::Base
  belongs_to :order
  validates :order_id, :presence => true
end
```

Doing the validation for referential integrity as data is loaded will definitely make the process slower, but the advantage is that we can be assured that the data read in will be valid.  (None, some, or all validations could be run on the data as it is read in.)

Advantages of ignoring referential integrity during data import:  
* much quicker to import; much simpler to develop

Disadvantages of ignoring referential integrity during import: 
* if invalid data is entered, it may not be apparent immediately -- it may cause some failure of the system that may not obviously be traced to invalid data


#### Some gems **do** validation as data is read in (and saved), some do not.
* Gems that do validation/referential integrity checking:
  * [ActiveRecord Import](https://github.com/zdennis/activerecord-import)

* Gems that do **not** do validation/referential integrity checking:
  * [Seed-fu](https://github.com/mbleigh/seed-fu) -- but there is [a pull request that does it](https://github.com/mbleigh/seed-fu/pull/39) (see Seed-Fu notes below)
  * [SeedBank](https://github.com/james2m/seedbank)
  * [ActiveImporter](https://github.com/continuum/active_importer)



## Definitions:
Bootstrapping:
The system has to load a minimal set of data just so it can be started.  This minimal amount of data is the boostrapping data.   


## Open Questions:


Because some data is dependent on others, some data definitely has to be read in/exist before others.  ex:  The country has to exist before the states can be loaded; the states info has to exist before the suburbs can be loaded.
* what are the dependencies ?  What is the ordering?
  * how much do we have to worry about it? 
    * may depend on the the gems & classes we use  ?
    * if we let ActiveRecord do validating, then the dependency checking is handled for us during validation


Where should the data be kept?  What directory should be the standard/convention?
* some gems and systems use folders under "db":  seed-fu uses db/fixtures, seedBank uses db/seeds
* some use lib/seed_data: OFN landing page images are kept there; read in by db/seed.rb
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



## Misc notes (not yet organized; noted here so the thoughts won't be lost)

* existing spree_country table includes the 3 letter ISO-3166-1 (alpha-3) abbrv. for the country (see http://en.wikipedia.org/wiki/ISO_3166-1_alpha-3)

* Ashley will write up some notes about what she's learned so far about the different gems and classes that could be used, including the approaches and assumptions they use,  and other issues she's come across so far.


* Should lib/tasks/dev.rake be completely replaced by this?  Or should this be called by dev.rake?

### Notes about Gems that do data importing:
* [seed-fu](https://github.com/mbleigh/seed-fu)
  * does not do validation, but there is [a pull request that has implemented validation](https://github.com/mbleigh/seed-fu/pull/39) (We need to test it to see how well it does/doesn't work)
* [SeedBank](https://github.com/james2m/seedbank)
  * does a nice job of handling the files and directories for importing. Perhaps part of this logic/code could be emulated
  * just does an `eval` of a file (using its own DSL); this limits what can be done and has caused problems for some (see the issue queue for SeedBank).  Looks like using eval like this is not a good approach
* [ActiveRecord-Import](https://github.com/zdennis/activerecord-import)
  * for bulk updating large numbers of records
  * can do validations
  * _AE/wsd is learning more about this gem to see what it could provide_
* [ActiveImporter](https://github.com/continuum/active_importer)
  * really just reads info from a CSV file, but validation methods could be called for each `row` that's read in
  * would be good to look at if/when CSV importing is done
* [taps](https://github.com/ricardochimal/taps)
  * does not really apply; deals with pulling and pushing entire (remote) databases
  * has not been updated in 2 years
* [data_miner](https://github.com/seamusabshere/data_miner)
  * WILL NOT RUN ON WINDOWS  This uses binaries and calls specific to Unix. 
  * Here's the description of the gem: `Download, unpack from a ZIP/TAR/GZ/BZ2 archive, parse, correct, convert units and import Google Spreadsheets, XLS, ODS, XML, CSV, HTML, etc. into your ActiveRecord models. Uses RemoteTable gem internally.`
  * It might have some logic to look at and emulate




## Ideas for later versions:

*  work with different file formats in addition to YML (CSV, JSON, etc.).  
    * CSV = allows users to work with the data in spreadsheets -- tools that non-tech people might be comfortable with
    *  JSON = more secure than YML (can't inject code as can be done with YML))
    * should be developed now so that this could be possible later

* would be good to be able to generate empty CSV files based on the current db schema -- so that users have some kind of structure they can fill in.  (a sort of simple template for each table/file needed)
    * dump data from current db into YAML/CSV.  Good not only to keep historical snapshots during dev/testing/demos, etc. -- also so that the data can be used as a starting place for creating a different data set.
* output to a directory that is given as an option
  * how is this different from rake db:schema:dump ? (is certainly related...)
      * should be db agnostic
