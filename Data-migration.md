## Data export

Particularly suitable for data migration between versions of OFN, one option is to use the [yaml_db](https://github.com/ludicast/yaml_db) gem to export. This will dump the database tables direct to yaml. Then in import, we could selectively load data from the tables as needed.

When migrating data from other systems into OFN, the incoming database format is likely to be quite different. In this case, it may be neater to define our own data migration format. For this, I'd prefer YAML over CSV for readability.


## Atomicity

I'd like a data migration process to be atomic. We could accomplish this by wrapping the import process in a transaction and rolling back if we hit any problems.


## Idempotency

I'd also like the process to be idempotent. Running it twice would produce the same result, and additionally, if you were to perform a second import with updated data, the existing records in the new system should be updated rather than duplicate records being created.

This could be accomplished by assigning a UUID to each record (user, product, etc) when that record is exported, if the record does not already have a migration id.

When the record is imported, we first look for a record with the same migration ID. If it is found, the record is updated. Otherwise, the record is created and given that migration ID.

Migration ids could be stored in their own table with a polymorphic association to avoid cluttering all tables with a migration_id column.

The benefit of this is that we could continue to migrate fresh data to an OFN system as we're preparing for switchover, rather than having to wipe and freshly prepare the image every time. Additionally, data could be syncronised between two systems by migrating back and fourth as needed.
