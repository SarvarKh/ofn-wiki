### I got given a data dump from staging / production and I'm getting weird data errors!

Try running `rails r script/prepare_imported_db.rb` – this will set various flags to allow us to use staging/production data in a development instance. Make sure the `development` configuration in `config/database.yml` is pointed towards the database you want to fix.

### I tried to run rake db:migrate for first time but I get errors with missing fields!

Try rake db:schema:load RAILS_ENV=development – Migrations provide forward and backward step changes to the database but some old migrations must be out-of-date with new changes and requirements.  When you need to create your database from scratch or drop it and create it again, better run rake db:schema:load to load schema. Make sure the `development` configuration in `config/database.yml` is pointed towards the database you want to load the schema or you will be deleting another database with all the data.

### My instance runs but I get an error 'missing relation'

If you are not sure about your db's state you can always recreate it:
`bundle exec rake db:reset`

This recreates it and runs the `db:seed` task as well.

When it succeeds you can load some sample data with:
`bundle exec rake dev:load_sample_data`

If you still having problems with your database state you can always delete it and re-create it.
Make sure that the psql user has the superuser role and then run `bundle exec rake db:setup db:test:prepare`. That should create the database and load the current schema.
