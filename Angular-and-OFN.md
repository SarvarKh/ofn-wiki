#Angular and OFN
OpenFoodNetwork.org uses Angular.js for the public-facing interface. Angular is a complex beast, so we've put together this article showing you how it all fits together.

Our application poses some uncommon issues and so contains some unusual design choices - deviating from the "Angular way" in specific cases.

## Things you should read
* [Understanding Scopes](https://github.com/angular/angular.js/wiki/Understanding-Scopes)
* [Coffeescript Cookbook](http://coffeescriptcookbook.com/)
* [Coffeescript.org](http://coffeescript.org)
* [A Practical Guide to Angular.js Directives](http://www.sitepoint.com/practical-guide-angularjs-directives/)
* [A Practice Guide to Angular.js Directives Part 2 (all the juicy bits)](http://www.sitepoint.com/practical-guide-angularjs-directives-part-two/)
* [Angular Directives for Foundation](http://madmimi.github.io/angular-foundation/)
* [Declaring Angular Services with Coffeescript](http://benhollis.net/blog/2014/01/17/cleanly-declaring-angularjs-services-with-coffeescript/)
* [80/20 Guide to Using Angular.JS filters](http://thecodebarbarian.wordpress.com/2014/01/17/the-8020-guide-to-writing-and-using-angularjs-filters/)

## Big picture

#### The pretty new layout is called 'Darkswarm'
* All the relevant Coffeescript is inside app/assets/javascripts/darkswarm
* The relevant layout is 'layouts/darkswarm'
* Anything outside Darkswarm should (and must) be redundant
* The name is a joke between Rohan and Will. Don't ask.

#### We use Sass, Haml, Coffeescript, Guard and LiveReload
All of these are fairly self-explanatory. We've got a nice workflow with Guard and Livereload that'll auto-refresh your browser; run:

    guard

#### Each page is a separate controller/view
For simplicity we've elected to keep each page separate, rendered server-side. We are **not** using Angular routes or views.

#### Angular templates are automatically compiled and injected
Anything inside app/assets/javascripts/templates will automatically be compiled, and injected by name into Angular's templateCache. Haml is supported. Templates can be accessed via templateUrl, e.g. 
```coffeescript
templateUrl: 'foo.html'
```

#### Data is injected on page load
We're using helpers (in InjectionHelper) to inject JSON data into pages onload. This is faster than an Ajax request, especially when data is required immediately.

```haml
:javascript
  angular.module('Darkswarm').value("#{name.to_s}", #{json})
```

The 'value' can be injected into a service. This will throw an exception if you inject a value that hasn't been defined: so make sure your data is in place before injecting services that require it.


#### Jasmine/Karma is being used for unit tests
Services, controllers and some aspects of directives should be tested. This is clunkier than using Rspec, but gets easier with practice!

For integration testing, we've elected to use Capybara with Rspec rather than Protractor. Many discussions have been had about this: in the end, it's the only option we've found viable.

#### Everything is done in Coffeescript
To make full use of Coffeescript you'll need to learn the idioms. [coffeescriptcookbook.com](http://coffeescriptcookbook.com) and [coffeescript.org](http://coffeescript.org) are your weapons of choice.

It's well-worth reading both these pages several times. Coffeescript has terse and elegant syntax; brain-bending but awesome. With practice you'll be able to avoid most of the ugly Javascript idioms.

## Technical exposition

### Angular Foundation
We're making heavy use of [Angular Foundation (AF)](http://madmimi.github.io/angular-foundation/). This provides us with various directives and tools to use Foundation components.

Currently [we're using a fork](github.com/willrjmarshall/angular-foundation) with some minor modifications. _These changes are slated for removal_; we'll be reverting to vanilla AF ASAP [once the badly-written tabs](https://github.com/openfoodfoundation/openfoodnetwork/blob/master/app/views/shopping_shared/_tabs.html.haml) are replaced with something more idiomatic

Since AF doesn't always precisely meet our needs, we've written our own directives that make use of and extend it. For example see the PriceBreakdown directive:

```coffeescript
Darkswarm.directive "priceBreakdown", ($tooltip)->
  tooltip = $tooltip 'priceBreakdown', 'priceBreakdown', 'click' 
  tooltip.scope = 
    variant: "="
  tooltip
Darkswarm.directive 'priceBreakdownPopup', ->
  restrict: 'EA'
  replace: true
  templateUrl: 'price_breakdown.html'
  scope: true
```
This directive uses AF's $tooltip service (which generates a directive object), then patches it (replacing the scope definition with an isolate scope). This allows us to use most of the logic inside $tooltip (show, hide, events, onscreen placement, etc) while using our own directive for the tooltip itself.

The priceBreakdownPopup directive (which controls our tooltip) is named according to a convention established in the $tooltip service, allowing us to use [our own template](https://github.com/openfoodfoundation/openfoodnetwork/blob/master/app/assets/javascripts/templates/price_breakdown.html.haml) and potentially our own behaviour.

### Active Model Serializers (AMS)
We use AMS to render JSON, although some older injection is still being done with Rabl templates. AMS supports key-cased caching, which we use with memcache to improve JSON rendering performance.

Serializers are scoped to the API namespace; serializers are automatically associated with models by name, causing changes to vanilla to_json serialization.
```ruby
class EnterpriseSerializer < ActiveModel::Serializer
end
Enterprise.new.to_json # automatically uses the serializer
```
vs
```ruby
module Api
 class EnterpriseSerializer < ActiveModel::Serializer
  end
end
Enterprise.new.to_json # The serializer must be invoked explicitly, otherwise serializing behaves as expected.
```
https://github.com/openfoodfoundation/openfoodnetwork/wiki/Angular-and-OFN/_edit#
### Injecting data into services
Many of our Angular services require data to be injected on page load. It's common to see exceptions indicating required data isn't available. For example:
```coffeescript
Darkswarm.factory 'Enterprises', (enterprises, CurrentHub, Taxons, Dereferencer)->
  new class Enterprises
```
This service will sometimes throw:
```
Error: [$injector:unpr] Unknown provider: enterprisesProvider <- enterprises <- Enterprises <- Hubs
```
Indicating that the "enterprises" value has not been set and cannot be injected into the Enterprises service. This can be resolved in the view:
```haml
= inject_enterprises
```

### Dereferencing
We're making heavy use of a technique called dereferencing. This is possible because Javascript supports pointers, allowing us to build circularly-referenced data structures.
```coffeescript
foo.bar.foo == foo # true
bar.foo.bar == bar # true
```
In many situations we inject a flat set of objects. Associations with other objects are represented with IDs:
```coffeescript
enterprise:
  associated_enterprises:
    id: 1
    id: 2
```
When such a service is loaded we _dereference_, replacing the IDs with pointers to corresponding objects. This generates a web of pointers between objects client-side. Duplication is avoided, so there's only a single object of a given type/ID.

```coffeescript
enterprise_1 =
  id: 1
  associated_enterprises:
    id: 2
enterprise_2 = 
  id: 2
  associated_enterprise:
    id: 1
```

after ingestion, we get:

```coffeescript
enterprise_1.associated_enterprises[0] == enterprise_2 # true
enterprise_2.associated_enterprises[0] == enterprise_1 # true
```

### The Cart
The cart is an elegant but counter-intuitive bit of code and requires some explanation. The various components are:

```coffeescript
Darkswarm.factory 'CurrentOrder', (currentOrder) ->
  new class CurrentOrder
    order: currentOrder
```
This represents the current order, pulled from the server (e.g. current_order). This is global, injected on page load, always-available and scoped to a single user.

```coffeescript
Darkswarm.factory 'Cart', (CurrentOrder, Variants, $timeout, $http)->
  # Handles syncing of current cart/order state to server
  new class Cart
    order: CurrentOrder.order
    # <snip>

```
This service wraps the CurrentOrder and adds some additional logic. It is responsible for the creation of LineItems and sending updates to the Cart to the server.

LineItems are where the magic happens. Each LineItem looks like this:
```coffeescript
line_item:
  variant: <pointer>
  quantity: <integer>
  max_quantity: <integer>
```
Pointers to variants are via dereferencing (see above). The initial set of LineItems in the Cart service is injected by the server.

```coffeescript
Darkswarm.factory 'Products', ($resource, Enterprises, Dereferencer, Taxons, Cart, Variants) ->
  new class Products
  # <snip>
```
The Products service represents all products available for purchase in the currently selected OrderCycle and Distributor. This logic is handled by the server; the service simply points to a JSON endpoint at /products.

The magic is in these two methods:
```coffeescript
registerVariants: ->
  for product in @products
    if product.variants
      product.variants = (Variants.register variant for variant in product.variants)
    product.master = Variants.register product.master if product.master
```
Registers every variant with the Variants service; either creating or returning an existing variant.

```coffeescript
registerVariantsWithCart: ->
  for product in @products
    if product.variants
      for variant in product.variants
        Cart.register_variant variant
    Cart.register_variant product.master if product.master
```

Registers every variant with the Cart service. This will create a new LineItem for any variant without one.
```coffeescript
register_variant: (variant)=>
  exists = @line_items.some (li)-> li.variant == variant
  @create_line_item(variant) unless exists 
    
create_line_item: (variant)->
  variant.line_item =
    variant: variant
    quantity: 0
    max_quantity: null
  @line_items.push variant.line_item

```


## Gotchas

#### Karma configuration 
There's a manifest file at spec/javascripts/application_spec.js. Relevant files must **also** be included here. Global stubs (e.g. disabling GMaps) should be put here as well.


#### Always render in Angular
It can be tempting to attempt to hybridize Rails and Angular, e.g. rendering with ERB, or injecting data using ng-init. **DON'T**.

When building an Angular page, Rails should be responsible for rendering basic HTML, injecting appropriate initialization data using the appropriate helpers, and providing any necessary Ajax endpoints. Angular must be responsible for all view logic, rendering, data-manipulation etc.

#### Minor things
* Some functions must explicitly return null, e.g.
    module ($provide)