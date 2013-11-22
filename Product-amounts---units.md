## The Problem

Spree provides a rather flexible but clunky interface for defining variants of a product. The admin can define some system-wide option types (eg. Weight) and option values (eg. "1 kg"), apply the option types to a product, and then select values for these for the variants.

This interface is confusing to learn, and slow and difficult to use, which is a problem when we will have many  enterprise users adopting the system.

Additionally, the way that Spree manages stock control does not always reflect reality. Take the case where we, selling flour, have a bulk store of 1 ton, and we package it into 1 kg and 10 kg bags as we receive orders. Spree provides stock control at the variant level, but then each variant will have its own stock level instead of taking the stock from one central pool for the product.


## The Solution

### Setting units and amounts for a product

As an admin user, I go to the bulk product edit page. For each **product**, I see two new fields:

- Single resource? (checkbox)
- Units (select box)

"Single resource" controls whether the stock comes from a central pool (when enabled) or if each variant has its own stock level (when disabled). Single resource only appears as an option, and can only be selected when the product has variants. When single resource is selected, only the count on hand for the product is editable. The count on hand for its variants are calculated in real time (based on amount avail in total on hand), but uneditable. When single resource is not selected, only the count on hand for the variants are editable. The count on hand for the product is blank.

"Units" sets the units from the following options:
* Weight: g; kg; t
* Volume: mL; L; ML
* Items: type of items (eg. dozens, bunches, bags, loaves, packets, etc.)
* Animals: (?)
* [Could we have a Unit option that is 'Custom', which lets the user name it?]

and then I update "On Hand" or select "On Demand".

At this stage the user picks a scale of unit (ie. mL vs L). This is used for entering amounts for all variants. They may need to enter values crossing several scales (ie. .25 L, 1 L, 10 L). On the front end, these will automatically be converted to human-friendly values (ie. 250 mL, 1 L, 10 L).

### Creating new variants from the bulk product edit page

The user can click an Add Variants link on a product row, which adds a row for the variant. Each **variant** then has two additional fields.
- Variant (float - shared resource; text - if not shared resource)
- On Hand (int)

"Variant" then describes or quantifies the variant. If the product is a "shared resource" this field is a float field, enabling it to be used for calculations. In this case it can be fractional (eg. 0.5 dozen). If the product is NOT a "shared resource", variants are text fields enabling a high level of user flexibility.
 
"On Hand" sets the amount available for this variant if not a shared resource (ie. 5 kg). If it is a shared resource, this field can only be set at the product level.

When I set these fields for a variant and click Save, the option type and value for that unit/var is looked up (or created if absent) and assigned to the variant.

### TODO: Formula for converting amount/unit into an option type string


### Editing option types outside of this interface

If the admin wishes to add additional option types to a product, they can do so using the traditional Spree interface. [Full product editing to be refined later]


### The interface is available on product edit page, also

The interface described above for setting variant amounts and units will also be available from the non-bulk product edit screen.


### Setting minimum supply quantity (MSQ, formerly known as group buy)

If a product is a shared resource, the user can only set its MSQ at the product level. Otherwise, each variant can have a MSQ.


### Data migration

1. This might be a lot of work, since the existing products don't conform to this system.
2. This amount of work will only grow as we get more products on the system.
