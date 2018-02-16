Feb 2018: If you just need to have loaded some sample data, run `bundle exec rake dev:load_sample_data` and it will do the trick, otherwise continue reading, please.

Following the defaults in this guide will result in an instance configured for Australia. While this is sufficient for most development work, it is often useful to have a development environment set up for another locale. Managers of other OFN instances may be able to provide dumps of sample data that can be used. These can be set up as follows:

1. Obtain a database dump e.g. `foo.sql.gz`.
2. From a terminal, in the OFN repository root directory, run `script/restore.sh foo.sql.gz`
3. Run `rails runner script/prepare_imported_db.rb` to disable some production payment settings.
4. Run `rake db:migrate` to migrate any database changes since the dump.
5. If you don't have admin credentials for the data you can set up your own super-admin user by running `rake spree_auth:admin:create` and following the prompts.

Finally, if the data are from a non-Australia locale, change `config/application.yml` appropriately, e.g.:

```{yml}
TIMEZONE: London
# Default country for dropdowns etc. See for codes: http://en.wikipedia.org/wiki/ISO_3166-1
DEFAULT_COUNTRY_CODE: GB
# Locale for translation.
LOCALE: en-GB
# Spree zone.
CHECKOUT_ZONE: UK
# Find currency codes at http://en.wikipedia.org/wiki/ISO_4217.
CURRENCY: GBP
```
