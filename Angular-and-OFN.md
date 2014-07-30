#Angular and OFN
Following is a breakdown of our approach to Angular.js integration with Rails. This application poses some uncommon issues and so contains some unusual design choices - deviating from the "Angular way" in specific cases.

Many of the design decisions were made by Will Marshall, who has moved to the US. If you've got questions about decisions he's made, contact him!

## Big picture
Following is a quick outline of our big-picture design decisions.

#### The new pretty layout is called 'Darkswarm'
* All the relevant Coffeescript is inside app/assets/javascripts/darkswarm
* The relevant layout is 'layouts/darkswarm'
* Anything outside Darkswarm should (and must) be redundant

#### We use Sass, Haml, Coffeescript, Guard and LiveReload
All of these are fairly self-explanatory. We've got a really nice workflow with Guard and Livereload set up that'll auto-refresh your browser when doing client-side work. To get it working install LiveReload in your browser and run:

    guard

####Each page is a separate controller/view
For simplicity we've elected to keep each page separate, rendered server-side. We are **not** using Angular routes or views. The pages are:
* _/_ - Hubs and homepage (HubsCtrl)
* _/producers_ - Producers listing (ProducersCtrl)
* _/groups_ - Groups page (GroupsCtrl)
* _/shop_ - Products page (ProductsCtrl)
* _/cart_ - Cart page (not currently in Angular)
* _/checkout_ - Checkout, finalize order (CheckoutCtrl, AccordionCtrl)

####Angular templates are automatically compiled and injected
Anything inside app/assets/javascripts/templates will automatically be compiled, and injected by name into Angular's templateCache. Haml is supported. Templates can be accessed via templateUrl, e.g. 

    templateUrl: 'foo.html'

####Data is injected on page load
We're using helpers (in InjectionHelper) to inject JSON data (via script tags) into pages onload. This is faster than triggering an Ajax request, and is useful in situations where data is required immediately to render the page.

####Jasmine/Karma is being used for unit tests
Services, controllers and some aspects of directives should be tested. This is clunkier than using Rspec, but gets easier with practice!

For integration testing, we've elected to use Capybara with Rspec rather than Protractor. Many discussions have been had about this: in the end, it's the only option we've found viable.

####Everything is done in Coffeescript
You can copy idioms and approaches from the existing code: e.g. I've worked out an efficient way to use Coffeescript when building services. Copy the idioms as much as possible; it'll help to read Coffeescript.org and master the nuances.

There are some idiomatic issues with Coffeescript and Angular; if you look to the code we've written for guidance, you should be fine!

## Technical notes

#### Angular Foundation
We're making heavy use of [Angular Foundation (AF)](http://madmimi.github.io/angular-foundation/). This provides us with various helpful directives and tools to build.

Currently [we're using a fork](github.com/willrjmarshall/angular-foundation) with some minor modifications. _These changes are slated for removal_; we'll be reverting to vanilla AF ASAP [once the badly-written tabs](https://github.com/openfoodfoundation/openfoodnetwork/blob/master/app/views/shopping_shared/_tabs.html.haml) are replaced with something more idiomatic

Since AF doesn't always precisely meet our needs, we've written our own directives that make use of and extend it. For example see the PriceBreakdown directive:

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

This directive uses AF's $tooltip service (which generates a directive object), then patches it (replacing the scope definition with an isolate scope). This allows us to use most of the logic inside $tooltip (show, hide, events, onscreen placement, etc) while using our own directive for the tooltip itself.

The priceBreakdownPopup directive (which controls our tooltip) is named according to a convention established in the $tooltip service, allowing us to use [our own template](https://github.com/openfoodfoundation/openfoodnetwork/blob/master/app/assets/javascripts/templates/price_breakdown.html.haml) and potentially our own behaviour.

#### Global controllers and widgets
Some Angular controllers (e.g. CartCtrl) are used globally and are available on every page. Some consideration must be given to dependencies, and data injection must be speedy: e.g. once-off controllers such as CheckoutCtrl can depend on CartCtrl, but not vice-versa.

#### Always render in Angular
It can be tempting to attempt to hybridize Rails and Angular, e.g. rendering with ERB, or injecting data using ng-init. **DON'T**.

When building an Angular page, Rails should be responsible for rendering basic HTML, injecting appropriate initialization data using the appropriate helpers, and providing any necessary Ajax endpoints. Angular must be responsible for all view logic, rendering, data-manipulation etc.

#### Dereferencing
We're making heavy use of a technique called Dereferencing. Javascript allows circular references:

    foo.bar.foo == foo # true
    bar.foo.bar == bar # true

In many situations (e.g. in the Enterprises service) we inject a flat set of objects. Associations to other objects are represented with IDs:

    enterprise:
      associated_enterprises:
        id: 1
        id: 2

When such a service is loaded we _dereference_, replacing the IDs with pointers to appropriate objects. This generates a (circular) web of pointers between objects client-side. This avoids code duplication, only having a single object representing a given Enterprise:

    enterprise_1 =
      id: 1
      associated_enterprises:
        id: 2
    enterprise_2 = 
      id: 2
      associated_enterprise:
        id: 1

after ingestion, we get:

    enterprise_1.associated_enterprises[0] == enterprise_2 # true
    enterprise_2.associated_enterprises[0] == enterprise_1 # true
      

## Gotchas

#### Karma configuration 
There's a manifest file at spec/javascripts/application_spec.js. Relevant files must **also** be included here. Global stubs (e.g. disabling GMaps) should be put here as well.

#### Minor technical things
* Some functions must explicitly return null, e.g.
    module ($provide)