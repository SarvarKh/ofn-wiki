Front-End – if you’re looking for instructions on how to use the system go HERE [different wiki outside GitHub]

[ERD Diagram]

## Technology Stack <a id="TechStack">

*  Rails 3.x
*  Ruby >= 1.9.2
*  PostgreSQL database
*  See Gemfile for a list of gems required

### Spree E-Commerce
Built on [Spree E-Commerce Engine](http://spreecommerce.com/) (currently being upgraded to v1.3.0), with significant modifications as outlined below. Advantages of building on top of this include:
* Strong basic functionality taken care of: orders; product displays; payments; checkout etc
* Active community maintaining and improving the underlying code base and documentation
* Reasonable and growing library of extensions to add functionality (including interfaces to other platforms)

## Structural Modifications <a id="Structure">

This section explains the main structural changes we have made to the Spree core, which reflect our design for flexibility in use and how the system could evolve.

### Enterprises <a id="Enterprises">
***

'Enterprises' are business or community entities that play an active role in supplying and/or distributing food. An Enterprise can: 
*  Take on one or more different roles e.g. a Supplier can also be a Distributor (i.e. aggregating produce from other farmers for transport) 
*  operate at any scale - for example Hubs / Distributors can range from regional Food Hub (providing as many concurrent [order cycles](#ordercycles) as they like) to retail fronts, farmers' markets or ephemeral hubs such as childcare centre, school or front verandah
*  Hubs can easily coordinate distribution to other Hubs (i.e. if one of your hubs wanted to do deliveries to a range of workplaces, schools etc)
*  [Not yet built] Purchase food from other Enterprises and move it into inventory for reselling, with the product history carried through and visible to later Buyers

![Enterprises](http://openfoodweb.org/foundation/wp-content/uploads/2013/02/Enterprises-1.png)

### Order Cycles <a id="ordercycles">
***

Order Cycles are a key organising principle of the system. Once the basic order cycle structure is in place (est. end February 2013), any Enterprise will be able to coordinate an Order Cycle, which means:
*  Set date and time that orders will open and close
*  Select suppliers and products to make available in that order cycle
*  Select one or more outgoing Distributors (that determine the options customers have for collecting or getting their orders delivered).

Usage Notes:
*  A Hub / Distributor can have separate and concurrent order cycles e.g. for different collection and delivery dates or dealing with different classes of products (e.g. bulk dry goods, fresh produce etc)
*  An order cycle can be available through one or more Hubs 

For planned functionality, see [Spec-Order Cycles]

 
### Products and Variants <a id="Products">

### Reports <a id="Reports">
 