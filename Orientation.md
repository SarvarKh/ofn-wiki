This page is written to explain the basic technology, design assumptions and functionality to potential developers. It does contain some information that would help to orient potential users, however more detailed usage instructions will be available HERE [TBC. different wiki outside GitHub]

The system is designed to create space for ultra-flexibility in how local farms and food enterprises might self-organise to connect, trade and move food. It is being designed and tested to work with our network of farmers, food coops, CSAs and others in Melbourne - however we have taken care to implement structures that will give others maximum flexibility to adapt to local context and relationships elsewhere.

If you want to see where things are really up to, check out the staging servers
staging1.openfood.com.au (order cycles turned off)
staging2.openfood.com.au (order cycles turned on)

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

The latest summary of objects and their relationships is shown below.
[ERD diagram]

***
### Enterprises <a id="Enterprises">

'Enterprises' are business or community entities that play an active role in supplying and/or distributing food. 

The simplest Enterprise has the role Supplier - these supply products to the system and set the base price.

The other current Enterprise type is a Distributor (or Hub - these terms are used somewhat interchangeably) 
to describe an Enterprise that aggregates and disaggregates products between suppliers and buyers.

An Enterprise can: 
*  Provide an offer to buyers i.e. make products available under certain conditions
*  Take on one or more different roles e.g. a Supplier can also be a Distributor (e.g. aggregating produce from other farmers for transport) 
*  Operate at any scale - for example Hubs / Distributors can range from regional Food Hub (providing as many concurrent [order cycles](#ordercycles) as they like) to retail fronts, farmers' markets or ephemeral hubs such as childcare centre, school or front verandah
*  Hubs can easily coordinate distribution to other Hubs (i.e. if a Hub coordinates distribution to a range of collection or delivery points e.g. workplaces, schools etc)
*  [Not yet built] Purchase food from other Enterprises and move it into inventory for reselling, with the product history carried through and visible to later Buyers

![Enterprises](http://openfoodweb.org/foundation/wp-content/uploads/2013/02/Enterprises-1.png)

***
### Order Cycles <a id="ordercycles">

Order Cycles are a key organising principle. Once the order cycle structure is in place (and basic multi-tenancy implemented), any Enterprise will be able to coordinate an Order Cycle, which means:
*  Set date and time that orders will open and close
*  Select suppliers and products to make available in that order cycle
*  Select one or more outgoing Distributors (that determine the options customers have for collecting or getting their orders delivered)
*  NB. Even within the same order cycle, different Hubs may have different products available (for example if they are aggregating orders from a central food hub as well as some very local farmers)

Usage Notes:
*  A Hub / Distributor can have separate and concurrent order cycles e.g. for different collection and delivery dates or dealing with different classes of products (e.g. bulk dry goods, fresh produce etc)
*  An order cycle can be available through one or more Hubs (e.g. buyers can choose their Hub for collection or delivery)
 
### Products and Variants <a id="Products">

The product and variant system is mostly unchanged from the [Spree standard](http://guides.spreecommerce.com/products_and_variants.html). The only significant difference is that every product **must** be associated with a Supplier. We are also making some changes to the administration interface for more efficient management of products and variants e.g. summary screens and bulk actions. 

Variants enable the same product to be easily organised by size of package, quantity, quality, colour etc. This means that the same product (e.g. olive oil from Olivia Green) can be easily sold in different size bottles at different prices.

### Reports <a id="Reports">

General reports have been created that enable management of food enterprises within this model. They can be viewed on screen or exported as a csv file. These include:
* Orders and Distributors - general read out of order information, including customer details, products and distributors. Can be filtered in excel to do almost anything you want
* Order Cycle Reports - Totals for a given date range of
  - Suppliers, Products
  - Suppliers - Totals for each Product to each Distributor
  - Distributors - Totals of Each Product from each Supplier
  - Customers - Items in Order, sorted by Distributor
 
### Multi Cart - shopping in two order cycles at one time
[Multi cart](Multi cart)
