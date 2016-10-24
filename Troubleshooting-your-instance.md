### I got given a data dump from staging / production and I'm getting weird data errors!

Try running `rails r script/prepare_imported_db.rb` – this will set various flags to allow us to use staging/production data in a development instance. Make sure the `development` configuration in `config/database.yml` is pointed towards the database you want to fix.