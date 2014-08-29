## Creating tagged variants

### Interface

- This is done through the bulk product edit interface.
- Begin with the standard process to add a new variant.
- The tagged enterprise is specified in the variant row in the producer column.
- To differentiate between setting a producer or a tag, the tag field will have a "#" in front of it, and will have a coloured background in the same styling as tags elsewhere on the site.


### Which enterprises can a user tag?

As a hub, I can tag a variant with enterprises that I manage, which also have the "add tagged variant" permission from the producer of the variant's product.

As a producer, I can tag a variant with hubs that have a trading relationship with the producer of the variant's product.


### Who can edit a tagged variant?

When a manager of a hub creates a tagged variant, that variant is editable by anyone who manages that hub or has "manage products" permission for that hub.

When a manager of a producer creates a tagged variant, that variant is editable by everyone in the previous paragraph, and also by the producer who created the tagged variant for 1 hour after it is created. After that hour has elapsed, they will lose their ability to edit it. During the edit window, they will see text like "55 minutes left to edit".

### Who can see which tagged variants?

- A producer can see all tagged variants for its products
- A hub can see untagged variants and its own tagged variants


## What is possible at different permission levels?

### User with only add to order cycle perms for an enterprise

- Cannot access bulk product edit.


### User with add tagged variants permission for an enterprise

- Can see the producer's products (ie. untagged variants), but cannot edit them (disabled fields). This view is provided for reference so they don't create duplicates.
- Can create a tagged variant
- Can see their own tagged variants


### User with manage product permissions for an enterprise

- Can list, create and edit untagged variants


### User who manages a producer

- Can do all of the above
- Can edit tagged variants they create, with the limitations described in "Who can edit a tagged variant?"


## How do tagged variants appear in the system?

The tag is displayed:
- Next to variants when adding them to an order cycle
- In the standard Spree product management pages (as well as BPE)

Tag colour will be derived from the tagged enterprise's ID, to provide a consistent colour throughout the system.


## Changing the producer of a product

Normally, a hub can only have a tagged variant when it has "create tagged variants" permission from the producer of the variant. If the producer of that product can be changed, there is the possibility that another user could change it to a producer that the hub does not have "create tagged variants" permission from. Because this is an invalid state for the system to be in, users will not be permitted to change the producer of a product after it has been created.


## Outstanding concerns

What happens when we create a tagged variant from a product that has only a master variant? This will invalidate the master as a variant that can be purchased from the shop, and thus break any open order cycles. We'll need to work around this in some way.