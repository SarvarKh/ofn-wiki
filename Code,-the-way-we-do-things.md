*This page is WIP, these still need to be discussed/decided with everyone*
*
* .



As in all code bases with some years we have different approaches to solve similar problems. Usually developers find better ways of doing things and go for it as they are implementing new features. This page lists and clarifies, for some topics, what are the existing ways of doing something in OFN and what is the recommended way of doing it.

# API First
### How to build web pages - rails views vs angular templates
If we want to get closer to having an API based application, we need to stop building screens using rails views accessing the rails models. We need to move into making mandatory the use of the API for every part of the app. Either by having frontend code (like angular) accessing the API (or by having the rails views depending only on the API but not the OFN data models).
Spree screen are not built in this way BUT this logic can be used for all the spree screens that we replace/rebuild to adapt to our needs.
This topic is being discussed here: https://community.openfoodnetwork.org/t/how-we-build-web-pages-rails-views-vs-angular-templates/1378

### JSON data on the browser - injection into the DOM (AMS) vs Angular MODEL accessing the API
Also to support the development of an API, we need to stop using AMS to inject json into the dom and start using the API through a (currently non-exiting) Angular model layer. Also applicable to spree screens that are rebuilt.

### Angular data layer
We should create an Angular DataLayer. Even if data is getting to Angular through AMS and angular modules with data (see availableCountries for example), we need to create a data layer where ALL access to data and server APIs are placed. No angular controller should make HTTP calls to the server. If a angular controller needs to access the server to post some data for example, that should not be done in the controller, it should always be done through the data layer or service.

### Object Serialisation - RABL vs AMS (ActiveModelSerializer)
To support the development of the API we need to define really well how to implement new endpoints on the API. Below are a few decisions related to that:

- do we use serializer with RABL as in app/views/json OR we prefer app/serializers/api?
- why is controllers/ShopController not in controllers/API if it is rendering json?
- why is views/api not in controllers/api
    possible rule: every view or controller that is rendering JSON goes into controller/api

Folders with RABL files in the OFN code base:
./app/views/api
./app/views/open_food_network
./app/views/json
./app/views/admin/json
./app/views/spree/api (spree API overrides)
This topic is being discussed here: https://community.openfoodnetwork.org/t/road-map-for-ofns-api/931/5

# Integration with Spree
### Spree model decorators - class_eval vs spree class extension vs using SimpleDelegator
Spree Class extension is better than using class eval as with it we end up with an OFN class that extends the Spree class: we can test this OFN class and our code doesn't directly depend on a spree class.

### (re)-building Spree views
When we want to change a Spree view, we used [Deface](https://github.com/spree/deface) to get our custom behaviour in the view. This has been painful to maintain and we now decided that all Spree views that are changed should be copied into our repository and changed as necessary without using Deface. This way we have a OFN view that depends on Spree code but does not depend on the Spree view.

# Making the application mode Modular
### Patterns to remove LOGIC from views
Both rails and angular views should not contain any business logic. OFN's business logic should be contained in rails services and/or angular services. This is also valid for angular controllers that should not contain business logic but rather depend on angular services.

### Refactor ActiveRecord Data Layer
The ActiveRecord layer should not include any business logic. Instead of having the controllers and views depend directly on the data layer directly, a services layer is created to contain all the business logic.
Another pattern we use when data layer is complex is to decouple ActiveRecord complexity in separate components still inside the data layer.

### Refactor database to resolve database structure issues
TODO: document database issues and approaches, and plan for refactoring
