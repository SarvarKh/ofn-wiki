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

'Enterprises' are business or community entities that play an active role in supplying and/or distributing food. An Enterprise: 
*  Any Enterprise can take on one or more different roles e.g. a Supplier can also be a Distributor (i.e. aggregating produce from other farmers for transport) 
*  A Distribution Enterprise can operate at any scale - from wholesale food hub (providing as many concurrent [order cycles](#ordercycles) as they like) to a childcare centre, school or front verandah - and can easily coordinate distribution to other hubs (i.e. if one of your hubs wanted to do deliveries to a range of workplaces, schools etc)
*  [Not yet built] An Enterprise will also be able to be a Buyer (i.e. able to purchase food from other Enterprises) and move it into inventory for reselling, however the product history will be carried through and visible to other Buyers

![Enterprises](http://openfoodweb.org/foundation/wp-content/uploads/2013/02/Enterprises-1.png)

### Order Cycles <a id="ordercycles">



### Products and Variants <a id="Products">

### Reports <a id="Reports">
 