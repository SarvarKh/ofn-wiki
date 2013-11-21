## The Problem

Spree provides a rather flexible but clunky interface for defining variants of a product. The admin can define some system-wide option types (eg. Weight) and option values (eg. "1 kg"), apply the option types to a product, and then select values for these for the variants.

This interface is confusing to learn, and slow and difficult to use, which is a problem when we will have many  enterprise users adopting the system.

Additionally, the way that Spree manages stock control does not always reflect reality. Take the case where we, selling flour, have a bulk store of 1 ton, and we package it into 1 kg and 10 kg bags as we receive orders. Spree provides stock control at the variant level, but then each variant will have its own stock level instead of taking the stock from one central pool for the product.


## The Solution

### Setting units and amounts for a product

As an admin user, I go to the bulk product edit page. For each **product**, I see two new fields:

- Single resource? (checkbox)
- Units (select box)

"Single resource" controls whether the stock comes from a central pool (t) or if each variant has its own stock level (f). When single resource is selected, only the count on hand for the product is editable. The count on hand for its variants are calculated in real time (based on amount avail in total on hand), but uneditable. When single resource is not selected, only the count on hand for the variants are editable. The count on hand for the product is blank.

"Units" sets the units from the following options:

Weight: g; kg; t

Volume: ml; L; ML

Items: type of items (eg. dozens, bunches, bags, loaves, packets, etc.)

Animals: (?)

[Later: We could have a Unit option that is 'Custom', which lets the user name it, and put in values themselves?]

Each **variant** then has two additional fields.

- Variant (float - shared resource; text - if not shared resource)
- On Hand (float)

"Variant" then describes or quantifies the variant. If the product is a "shared resource" this field is a float field, enabling it to be used for calculations. If the product is NOT a "shared resource", variants are text fields enabling a high level of user flexibility.
 
"Amount" sets the amount for this variant (ie. 5 kg). Amount can be fractional (eg. 0.5 dozen). 

When I set these fields for a variant and click Save, the option type and value for that unit/var is looked up (or created if absent) and assigned to the variant.

### TODO: Formula for converting amount/unit into an option type string

### Creating new variants from the bulk product edit page

The user can click an Add Variant link on a product row, which adds a row for the variant. The variant's options are set via the above interface.


### Editing option types outside of this interface

If the admin wishes to add additional option types to a product, they can do so using the traditional Spree interface.


### The interface is available on product edit page, also

The interface described above for setting variant amounts and units will also be available from the non-bulk product edit screen.