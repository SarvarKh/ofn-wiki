This page lists all teh sources of routes in the OFN app.

You can see the full list by running ´rake routes´

These are the routes files that generate this list.
In OFN:
- the main routes file [config/routes.rb](https://github.com/openfoodfoundation/openfoodnetwork/blob/master/config/routes.rb)
- the admin routes file [config/routes/admin.rb](https://github.com/openfoodfoundation/openfoodnetwork/blob/master/config/routes/admin.rb) 
- the ofn custom spree routes
[config/routes/spree.rb](https://github.com/openfoodfoundation/openfoodnetwork/blob/master/config/routes/spree.rb) 

In the core routes above you can see that spree and web engines routes are also included:
https://github.com/openfoodfoundation/openfoodnetwork/blob/8ff5f9055b70de7ae229f2588e194001ee6c8ff5/config/routes.rb#L121

That includes the web engine routes [engines/web/config/routes.rb](https://github.com/openfoodfoundation/openfoodnetwork/blob/master/engines/web/config/routes.rb) 

And all the spree routes files:
- [spree - api/config/routes.rb](https://github.com/spree/spree/blob/master/api/config/routes.rb) 
- [spree - backend/config/routes.rb](https://github.com/spree/spree/blob/master/backend/config/routes.rb) 
- [spree - frontend/config/routes.rb](https://github.com/spree/spree/blob/master/frontend/config/routes.rb) 

To understand how rails routes work you can read this guide:
https://guides.rubyonrails.org/v3.2.8/routing.html
A very important point is "2.2 CRUD, Verbs, and Actions" where you can see how one entry in routes can generate 7 different routes.

Happy navigations!