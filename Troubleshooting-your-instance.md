### I got given a data dump from staging / production and I'm getting weird data errors!

Try running `rails r script/prepare_imported_db.rb` – this will set various flags to allow us to use staging/production data in a development instance. Make sure the `development` configuration in `config/database.yml` is pointed towards the database you want to fix.

### I tried to run rake db:migrate for first time but I get errors with missing fields!

Try rake db:schema:load RAILS_ENV=development – Migrations provide forward and backward step changes to the database but some old migrations must be out-of-date with new changes and requirements.  When you need to create your database from scratch or drop it and create it again, better run rake db:schema:load to load schema. Make sure the `development` configuration in `config/database.yml` is pointed towards the database you want to load the schema or you will be deleting another database with all the data.