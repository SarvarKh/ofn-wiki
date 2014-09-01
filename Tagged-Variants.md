## Creating tagged variants

### Interface

- This is done through the bulk product edit interface.
- Begin with the standard process to add a new variant.
- The tagged enterprise is specified in the variant row in the producer column.
- To differentiate between setting a producer or a tag, the tag field will have a "#" in front of it, and will have a coloured background in the same styling as tags elsewhere on the site.


### Which enterprises can a user tag?

As a hub user, I can tag a variant with enterprises that I manage, which also have the "add tagged variant" permission from the producer of the variant's product.

As a producer user, I can tag a variant with hubs that have a trading relationship with the producer of the variant's product.


### Who can edit a tagged variant?

Any manager of a hub can edit variants tagged with that hub.

When a manager of a hub creates a tagged variant, that variant is editable by any user who manages that hub (as implied by the previous rule).

When a manager of a producer creates a tagged variant, that variant is editable by any user who manages the tagged hub, and also by the producer who created the tagged variant for 1 hour after it is created. After that hour has elapsed, they will lose their ability to edit it. During the edit window, they will see text like "55 minutes left to edit".

[Kirsten: I know you guys talked about this quite a bit and so I assume there are good reasons why you settled on this approach, but it doesn't seem quite right to me . .

Seems like the case might be more that if created by the Producer, they retain 'ownership' of this variant, but the tagged Hub can use it, add to OCs etc. So if this is for the case where Producer is setting different prices etc, they maintain the ability to remove, adjust inventory etc?]

### Who can see which tagged variants?

- A producer can see all untagged and tagged variants for its products
- A hub can see untagged variants and its own tagged variants


## What is possible at different permission levels?

### User with only add to order cycle perms for an enterprise

- Cannot see products produced by this enterprise in BPE

[Kirsten: I think this user should be able to see but not edit products in BPE - so that they can easily see what broccoli (for example) is available and decide what they want to use. So this permission effectively become 'view and use but not edit' products]

### User with add tagged variants permission for an enterprise

[Kirsten: If above change is made, then this becomes incremental addition on Add to OCs permission]

- Can see the producer's products (ie. untagged variants), but cannot edit them (disabled fields). This view is provided for reference so they don't create duplicates.
- Can create a tagged variant
- Can see and edit their own tagged variants


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

Normally, a hub can only have a tagged variant when it has a trading relationship with the producer of the variant. If the producer of that product can be changed, there is the possibility that another user could change it to a producer that the hub does not have a trading relationship with. Because this is an invalid state for the system to be in, users will not be permitted to change the producer of a product after it has been created.


## Outstanding concerns

### Creating a tagged variant from a product that has only a master variant

This will invalidate the master as a variant that can be purchased from the shop, and thus break any open order cycles. We'll need to work around this in some way.

We could:

1. Make products with and without variants behave more consistently
2. When the tagged variant is created, create an extra variant to replace master, and substitute it in order cycles.

Approach 2 is strongly coupled. If we make a new feature that references variants, we would need to know to change out its references when tagged variants are created. Yuck. Therefore, go with approach 1.

With approach 1, we would just need to change product creation, since a product with one variant is represented correctly on the system. In other words, when a product is created, it will be given both a master variant and a single initial variant with the same values as master was given.

### Revoking tag access

If an admin removes a trading relationship, are all the tagged variants dependent on it deleted.
