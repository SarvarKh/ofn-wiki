Some code changes require database updates as well. We make use of [migrations](https://guides.rubyonrails.org/active_record_migrations.html) to deliver necessary data changes to all our servers. Here are some guidelines we like to follow.

### Include all needed ActiveRecord models in your migration file

> Sometimes a migration needs to do more than add or remove a column. When you must convert existing data, migrations can get nasty because SQL is all you can use. You can never call models in a migration because your models will evolve away from your migrations...
https://blog.makandra.com/2010/03/how-to-use-models-in-your-migrations-without-killing-kittens/

### Always provide a rollback method

Simple migrations have a `change` method that can perform the migration and can be used to reverse it as well. But not everything can be reversed that easily. In that case you can declare the `up` and `down` methods, one for migrating and the other for rolling back. If the `up` method is destructive, you can't reverse it. For example, if you delete a whole column, the data is lost and can't be restored ... unless you keep a backup. No matter how simple the change is, we have some pretty complex production data and the simplest change can have nasty side effects. Therefore we should always write a backup file for the data we are destroying and provide a `down` method that can reverse the migration. The [first example](https://github.com/openfoodfoundation/openfoodnetwork/blob/master/db/migrate/20181123012635_associate_customers_to_users.rb) can give you an idea of how that works. Let's make it a bit more beautiful and re-usable next time we need a reversible destructive migration.