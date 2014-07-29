#How Angular works
This is breakdown of our approach to Angular.js integration with Rails. Our particular case poses a handful of unique issues, and so we've made some non-standard design choices.

## Some basic design decisions:
Here's a quick description of our design decisions, illustrating how to work with our Angular framework.

###Each page is a separate controller/view
For simplicity we've elected to keep each page separate, rendered server-side. We are **not** using Angular routes or views.

###Angular templates are automatically compiled and injected
Anything inside app/assets/javascripts/templates will automatically be compiled, and injected by name into Angular's templateCache. Haml is supported. Templates can be accessed via templateUrl.

###Data can be injected on page load
We're using the InjectionHelper to inject JSON data (via script tags) into pages onload. This is faster than triggering an Ajax request, and is useful in situations where data is required immediately.

###Everything is done in Coffeescript
You can copy idioms and approaches from the existing code: e.g. I've worked out an efficient way to use Coffeescript when building services. Copy the idioms as much as possible; it'll help to read Coffeescript.org and master the nuances.