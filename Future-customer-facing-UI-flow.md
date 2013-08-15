# High level description

The decision was made to decouple the user interface from Spree and just use Spree API that feed the custom user interfaces.  The flow begins with the landing page, where the user is presented with the ability to enter the suburb (via the suburb select auto complete box).  After the user selects the suburb and performs the search, the system performs a geo distance search for the nearest hubs.  It presents the user with the list of 10 closest hubs within a 100km radius.  The system also presents a map view for the hubs in the list.  After the item in the list is clicked, the user is taken to chosen shopfront.

The functionality is accessible by URL:
[https://staging2.openfood.com.au/enterprises/search](https://staging2.openfood.com.au/enterprises/search)

# Technologies Summary
Mostly the standard existing technology stack is used to implement the new UI.  Currently there are no single page application, however [AngularJS](http://angularjs.org/) (since it already utilized for the admin section) may be considered in the future.  The following list includes technologies (that is worth mentioning) that were introduced so far in the development process. 

* [Zurb Foundation](http://foundation.zurb.com/)- responsive design
* [spin.js](http://effinroot.eiremedia.netdna-cdn.com/repo/plugins/misc/spin.js/index.html) - spinners
* [gmaps4rails](https://github.com/apneadiving/Google-Maps-for-Rails) - maps integration
* [geocoder](http://www.rubygeocoder.com/) - geo location and search

# Implemented Pages - Current State
## Layout
Currently the layout _landing_page.html.haml_ consists of the top bar and the content section.  The login and sign up links reveal the off canvas sliding menu (zurb's off canvas layouts).  There is also escape patch with the logo in to top left corner, as well as links to "distributors" and "farmers" in the top right.  Clicking on those links reveals the modal with the form, contents of which should be emailed to openfoodweb admin users, once the functionality is wired in.

### Outstanding tasks
* Wire in to the form in "Distributors" and "Farmers" to be emailed to admin users
* Make "Sign Up" form to be an Ajax form (same as "Login")
* Force SSL side wide (for "Login" and "Signup" forms to work when landing page is not accessed via HTTPS explicitly)  

## Landing Page
The landing page consists of the centered search box (which suburb/postcode jQuery autocomplete field) with the Open Food Network logo on top.  It has a background of a random image.

### Outstanding tasks
* Replace random images that were seeded from random Google images with real OFN images. 

## Hub List (Search Results)
The hub lists consists of 10 closest hubs with the map view.  Clicking on the hub link would bring the user to the shop front for that hub.

## Shop Front
There are only bare bone page for shop front with not much on it.  It would have to be implemented based on provided wireframes.
 