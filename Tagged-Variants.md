## Data model

A variant can be tagged by an enterprise.


## Creating tagged variants

This is done through the bulk product edit interface.
Begin with the standard process to add a new variant.
The tagged enterprise is specified in the variant row in the producer column.
To differentiate between setting a producer or a tag, the tag field will have a "#" in front of it, and will have a coloured background in the same styling as tags elsewhere on the site.


### Which enterprises can a user tag?

As a hub, I can tag a variant with enterprises that I manage, which also have the "add tagged variant" permission from the producer of the variant's product.

As a producer, I can tag a variant with hubs that have a trading relationship with the producer of the variant's product.


## What is possible at different permission levels?

### User with only add to order cycle perms for an enterprise

- [[**]] Can see products in BPE, but with all fields disabled. They cannot perform updates.


### User with add tagged variants permission for an enterprise

- Can see products from producer (ie. untagged variants), with disabled fields (so they don't create duplicates)
- Can create a tagged variant
- Can see their own tagged variants
