As in all code bases with some years we have different approaches to solve similar problems. Usually developers find better ways of doing things and go for it as they are implementing new features. This page lists and clarifies, for some topics, what are the existing ways of doing something in OFN and what is the recommended way of doing it.

# API First
### How to build web pages - rails views vs angular templates
If we want to get closer to having an API based application, we need to stop building screens using rails views accessing the rails models. We need to move into making mandatory the use of the API for every part of the app. Either by having frontend code (like angular) accessing the API (or by having the rails views depending only on the API but not the OFN data models).
Spree screen are not built in this way BUT this logic can be used for all the spree screens that we replace/rebuild to adapt to our needs.

### JSON data on the browser - injection into the DOM (AMS) vs Angular MODEL accessing the API
Also to support the development of an API, we need to stop using AMS to inject json into the dom and start using the API through a (currently non-exiting) Angular model layer. Also applicable to spree screens that are rebuilt.

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

# Integration with Spree
### Spree model decorators - class_eval vs spree class extension vs using SimpleDelegator



