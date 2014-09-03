# Enterprise Flags

## Purpose
To design and implement a set of flags that are placed on each enterprise in the system, which combine to determine what role(s) that enterprise plays in the network.

## Desired outcomes
The ability to designate each enterprise as one of the following
* Producer Profile: Is listed as a producer, but cannot sell products
* Hub Profile: Is listed as a place where products are available for sale, but without an actual OFN shopfront
* Single Store: A producer which wishes to market its products directly, without offering products of any other producers for sale.
* Hub (Aggregator): An enterprise which may or may not produce its own products, but which offers the products of other producers for sale.

The model should be flexible and readily alterable in the future to take on additional requirements.

## Proposed Data Model
### Producer Flag (is_producer)
Designates whether an enterprise produces the products it is selling or not (this is currently handled using the is_producer and is_distributor flags).

### Shopfront Flag (is_trader)
Determines whether the enterprise is listed has a open shopfront. ie. can it offer products for sale?

### Network Flag (i_can_has_network)
When set, allows the enterprise to offer the products produced by other enterprises.


## Proposed usage of flags to model desired enterprise types
* Producer Profile: is_producer: true, is_trader: false, i_can_has_network: false
* Hub Profile: is_producer: false, is_trader: false, i_can_has_network: false
* Single Store: is_producer: true, is_trader: true, i_can_has_network: false
* Hub (Aggregator): is_producer: (true or false), is_trader: true, i_can_has_network: true

## Specific Rules
Any trading producer (ie. with is_producer and is_trader flags) can offer products for sale through other enterprises which have networking permission, regardless of whether they themselves are able to network.
 