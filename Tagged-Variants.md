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


### Who can edit a tagged variant?

When a manager of a hub creates a tagged variant, that variant is editable by anyone who manages that hub or has "manage products" permission for that hub.

When a manager of a producer creates a tagged variant, that variant is editable by everyone in the previous paragraph, and also by the producer who created the tagged variant for 1 hour after it is created. After that hour has elapsed, they will lose their ability to edit it. During the edit window, they will see text like "55 minutes left to edit".


## What is possible at different permission levels?

### User with only add to order cycle perms for an enterprise

- [[**]] Can see products in BPE, but with all fields disabled. They cannot perform updates.


### User with add tagged variants permission for an enterprise

- Can see products from producer (ie. untagged variants), with disabled fields (so they don't create duplicates)
- Can create a tagged variant
- Can see their own tagged variants


### User with manage product permissions for an enterprise

- Can list, create and edit untagged variants
- Can create a tagged variant


### User who manages a producer

- Can see everything
- Can edit their untagged variants
- When they create a tagged variant, they can edit it for 1 hour, and then only the tagged enterprise can edit that variant.