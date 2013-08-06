To allow an 'Enterprise' user to access a part of the spree admin interface, you need to look into the following things:

1. Ability decorator
   app/models/spree/ability_decorator.rb
Define the cancan ability for the model in the AbilityDecorator (you probably need to include :admin for any pages in the admin backend)

2. Override controller method for index / searching functions
   e.g. app/controllers/spree/admin/products_controller_decorator.rb
You may need to manually filter out the items they can see by overriding the index or search actions in the spree controller. In theory, this should happen automatically by granting the :read action permission, but I cannot get it to work.
