## Purpose
To alter the data model around enterprises and their addresses to allow for greater flexibility in how the relationship between an address and an enterprise or multiple enterprises is configured, thus providing support for as-yet unsupported setups.

## Constraints and Immediate problems which must be solved
As a Producer, I want to offer my own produce for pickup from an existing Hub, but within my own order cycle, without creating a duplicate hub on the map, and without the need for a complex set of permissions around controlling or creating the Hubs order cycles.

As a Hub, I want to offer a series of pickup locations for my products, without necessarily creating a who new enterprise for each pickup location. This is useful when the pickups are 'nubs', ie. the only information which differentiates them is the physical location of the pickup.

Ability to perform all existing use cases must be preserved.

## Things to think about
* How enterprises would be able to share a 'location'
* Exposure of Payment and Shipping Methods on the front end, and where they sit in the data model
* Permissions around using a 'location'
* Impact on UI, front and back
* Naming conventions - does the use of 'locations' affect the use of [Enterprise Flags](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Enterprise-Flags)?
* Implications for order cycle interface - differing inventories for differing locations

## Proposal
Locations and Enterprises
* The address property of enterprises is pulled out into a new model called a 'location'.
* A 'location' may be shared between multiple enterprises, but has only one 'owner' enterprise
* Each enterprise must have at least one 'location', much the same way it currently must have an address
* Enterprises may have multiple locations
* The ONLY property or set of attributes that is shared between enterprises that share an address is the address itself. ie. Payment and Shipping methods, Products, Order Cycles are NOT shared.

Shipping and Payment Methods
* Are owned by a single enterprise (though permissions should exist for sharing payment and shipping methods with other enterprises)
* Are controlled at the location level. ie. Each location can utilise a subset of the parent enterprise's methods to use.

Order Cycles
* Locations now take the place of Hubs, in that the outgoing list shows the locations available for each selected hub, and products may be turned on and off at the location level for a particular hub.
* An option in the UI should be created (probably as part of a larger overhaul) to allow all locations pertaining to a particular hub to be populated with the same set of products, so that each does not have to be managed individually.