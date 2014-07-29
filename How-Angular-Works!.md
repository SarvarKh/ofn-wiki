#How Angular works
This is breakdown of our approach to Angular.js integration with Rails. Our particular case poses a handful of unique issues; we've made some non-standard design choices.

## Big picture
Here's a quick description of our design decisions, illustrating how to work with our Angular framework.

#### The new layout is called 'Darkswarm'
* All the relevant Coffeescript is inside app/assets/javascripts/darkswarm
* Views are (predictably) inside app/views/darkswarm

####Each page is a separate controller/view
For simplicity we've elected to keep each page separate, rendered server-side. We are **not** using Angular routes or views.

####Angular templates are automatically compiled and injected
Anything inside app/assets/javascripts/templates will automatically be compiled, and injected by name into Angular's templateCache. Haml is supported. Templates can be accessed via templateUrl.

####Data can be injected on page load
We're using the InjectionHelper to inject JSON data (via script tags) into pages onload. This is faster than triggering an Ajax request, and is useful in situations where data is required immediately.

####Jasmine/Karma is being used for unit tests
Services, controllers and some aspects of directives should be tested. This is clunkier than using Rspec, but gets easier with practice!

For integration testing, we've elected to use Capybara with Rspec.

####Everything is done in Coffeescript
You can copy idioms and approaches from the existing code: e.g. I've worked out an efficient way to use Coffeescript when building services. Copy the idioms as much as possible; it'll help to read Coffeescript.org and master the nuances.

## Technical notes

#### Dereferencing
We're making heavy use of a technique called Dereferencing. Javascript allows circular references:
    foo.bar.foo == foo # true
    bar.foo.bar == bar # true
In many situations (e.g. in the Enterprises service) we inject a flat set of objects. Associations to other objects are represented with IDs:

    enterprise:
      associated_enterprises:
        id: 1
        id: 2

When such a service is loaded we _dereference_; replacing the IDs with pointers to appropriate objects. This generates a (circular) web of pointers between objects client-side. This technique avoids code duplication, only having a single object representing a given Enterprise:

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
      

## Gotchas and notes
#### Karma configuration 
There's a manifest file at spec/javascripts/application_spec.js. Relevant files must **also** be included here. Global stubs (e.g. disabling GMaps) should be put here as well.

#### Minor technical things
* Some functions must explicitly return null, e.g.
    module ($provide)