# Install on Heroku
I am working on setting OFN Greece. Here is how I deployed openfoodnetwork on hekoru from ubuntu. I am ruby/rails newbie, be indulgent, so my deploy is not perfect but works :

1. Install first locally on ubuntu (on your machine/virtual machine) according to the instructions : 
> https://github.com/openfoodfoundation/openfoodnetwork/blob/master/README.md 

2. Open a heroku account

3. Download again a clone of this git, unpack

4. add this version to heroku : 
> git init

> git add .

> git commit -m "example"

> heroku create

> heroku addons:create heroku-postgresql

> git push heroku master

5. You may stumble on the same error than I did, this is how I solved them

   a. copying config/application.yml.example into config/application.yml and setting heroku config:set TIMEZONE=UTC solved the following error : `remote: Value assigned to config.time_zone not recognized.Run "rake -D time" for a list of tasks for finding appropriate time zone names.`

   b. removed all dalli from env & gem files to solve an error trying to connect to dalli service during service installation. You can enable dalli in heroku by using credit card as verification of account, but I did not want to (yet).

  c. next I stumbled upon the following error, due to not setting the db content before asset precompilation : `remote: ActiveRecord::StatementInvalid: PG::Error: ERROR:  relation "spree_countries" does not exist`. What I did first is to recompile locally the assets (with the db we setup in 1.) "set TIMEZONE=UTC" and "rake assets:precompile". I got a `Peer authentication failed for user "ofn"`, so I changed /etc/postgresql/9.1/main/pg_hba.conf peer to md5. I got this next : `PG::Error: FATAL:  database "open_food_network_prod" does not exist`, I solved with "rake assets:precompile RAILS_ENV=test". Finally I got `NoMethodError: undefined method `iso' for nil:NilClass`, application.rb (or .yml ?) was missing + rake db:seed solved the problem. 

   d. now "git push heroku master", and you see he find the manifest and precompiled assets. The git pushed fine and deployed the app, great !

   e. the database is still not set so the application cannot run correctly. I tried to execute "heroku rake openfoodnetwork:dev:load_sample_data", like in local, but it never made it. So what I did is simply to copy the local database into the remote. You must replace localhost by your full url (use/password included) :
>  heroku pg:push localhost postgresql-pointy-35385


I am sure this is not the exact right way, but it works. So I hope you'll help me improve the steps. 

# Post-install setup

* **Country :** To open a shop/producer account, I needed to add my country. I added my country directely to my heroku app db (tables spree_countries ans spree_states), using an sql request with jackdb, using data from this table : 
https://github.com/spree/demo/blob/master/db/default/spree/countries.yml. 

* **Producer/Shop OFN setup :** The user guide is easy to follow. Validated my email (validate in db, untill email config, see further). Then, after creating the shop, adding product require to create a taxonomy with admin account. Then I created a product. In entreprises define delivery, payment (update twice), create a order cycle. Then the product and the shop became visible.

* **Google Maps :** Then I had to get an api key from [google dev page.](https://console.developers.google.com/flows/enableapi?apiid=maps_backend%2Cgeocoding_backend%2Cdirections_backend%2Cdistance_matrix_backend%2Celevation_backend%2Cplaces_backend&reusekey=true&hl=Fr) and add it to heroku config with "heroku config:set GOOGLE_API_KEY=MY_API_KEY".

* **Confirmation email :** This one is difficult. No email sent, but no error message in heroku log. It took me days of tinkering around. The spree standard mail method page could not send test or confirmation emails, neither locally nor on heroku. I don't understand what is wrong with it, but even the interface is bugging, e.g. dont keep changes after you update. I couldn't figure out how to find logs of the mail sending operation neither, to see if the setup I did is effectively used. But here's a workaround. You can disable Spree and use actionmailer default config : 
>  * make sure your smtp mail is usable :  I activated the unsecure apps , use the gmail captcha unlock. 

>  *  I tried locally in development.rb, something like this and it worked very well (I tried 4 smtp sever all working) :
"config.actionmailer.performdeliveries = true
config.actionmailer.raisedeliveryerrors = true
config.overrideactionmailerconfig = false
ActionMailer::Base.default :from => 'ofngr@yahoo.com'
config.actionmailer.deliverymethod = :smtp
  config.actionmailer.smtpsettings = {
  address:              'smtp.mail.yahoo.com',
  domain:               'yahoo.com',
  port:                 25,
  username:           'ofngr@yahoo.com',
  password:             'password',
  authentica`ti--n:       'plain'
  }"

>  * But when deployed, it stopped strangely to send emails.  Even when user/pass are wrong, no error show up, the browser confirm sucessfully and heroku mention "email sent to ...". Gmail even detect an attempt to connect if you do not activate the unsecure apps. My mistake was to think that "config.override_actionmailer_config = false" is sufficient to disable spree from overriding my actionmailer config. No, I also had to remove all mail methods defined in the standard interface. Strangely enough, locally it wasn't necessary and I could even send test mails when clicking the button (that used the smtp config of my development.rb and not the one defined in the mail method list).

>  * There may be a required additional step, I am not sure, credit card verify the account with heroku and enable worker dyno. I hate that our info and bank card is used like this, but before finding out about removing all mail spree methods, in dispair, I gave a try to the verification, and enabled sendgrid plugin. It didn't help at the time, but now that removed spree methods and the emails are received I see the logs, the email is effectively sent by a worker dyno, that was not available, nor activable without credit card verification, even when I use no sendgrid plugin (with other smtp server than sendgrid). I guess the emails is sent in a delayed job, but I am not sure the unverified account with no worker can do it. 

>  * Now you may have a problem with FROM field. As spree find no mail method, OFN use the default smtp config but cannot find a "from" address. So I had to add "ActionMailer::Base.default[:from]" to app/mailers/spree/base_mailer_decorator.rb, so that it uses also actionmailer default "from" field.

>  * My final config in development.rb with sendgrid on heroku is : 
config.action_mailer.perform_deliveries = true
config.action_mailer.raise_delivery_errors = true
config.override_actionmailer_config = true

ActionMailer::Base.default :from => '"OpenFoodNetwork-Greece" <openfoodnetwork-greece@planet.earth>'
  config.action_mailer.delivery_method = :smtp
  config.action_mailer.smtp_settings = {
  address:              'smtp.sendgrid.net',
  domain:               'heroku.com',
  port:                 587,
  user_name:           ENV['SENDGRID_USERNAME'],
  password:             ENV['SENDGRID_PASSWORD'],
  authentication:       'plain',
  enable_starttls_auto: true
  }


Now I/You need to customize everything to be not only running but usable by my/your local producers.


# Customization:

The next step to do is to customize the app to my local needs. For the intellectual part of this customization, you may want to be aware, that there is these resources to run the platform properly, promote it, and support your users :
1. Existing community forum : https://community.openfoodnetwork.org/t/welcome-to-the-open-food-network-community-forum/81
2. User, hub and producer guides : https://openfoodnetwork.org/user-guide/
3. Faq : https://openfoodnetwork.org/user-guide/faqs/customer-faqs/
4.Case studies you may want to put forward : https://openfoodnetwork.org/user-guide/case-studies/
5.Features list : https://openfoodnetwork.org/feature-list/
6. Other OFN regional instances, more or less active and customized
I also found that technical concepts are difficult to understand and possibly to translate, such as hub (a place/location where you sell permanently/temporarily) 

For the technical part, I updated the text, the pictures, created taxons, change the filter size button. If you run into trouble, do not under-estime reproducing the problem locally (or on your virtual box linux), looking in the db (e.g. with jackdb) it helped me a lot.

One day, I would like also to create a product search page through all shops (like in amazon, or any platform), a so much missing feature with maps results, but my RoR level is not yet there. Now, the user has to enter each shop/producer to see what products they have, they cannot see a list of “tomato” products of different shops.

## Update the text
To update the text, I created a en-GR.yml, not wanting to translate all to greek, but only partially, turn it to greeklish. 
* Export Locale as env variable in heroku
* Set locale in application.yml
* Now, I can change en-GR.yml text, and see it updated but...
* If you opted for precompiling assets locally like me, you may notice that some pages text do not update. You need to clean that and let heroku precompile each time you update
* You may not want to miss translating ACN BCN (tax numbers)
* Create your taxons, or tags
* The first time I created the zones, I missed something that prevented the checkout with this message: “No shipping methods available for selected location, please change your address and try again.” You have to go back to your zones and check that all zones are country or state based, meaning a group of countries or a group of states. You also have to add in each zone which countries and states are part of it, in the zone edit panel. It is not sufficient to add the states to the country in the states panel, you have to add them in the zone panel too.
## Pictures
I also customized the logos and pictures, to create identity  :
* First, I replaced the logo of spree in admin area by adding in spree.rb : config.admin_interface_logo = "http://res.cloudinary.com/hkzkjlyxw/image/upload/v1/public/images/content_configurations/logos/original/logo-ellada.png.png"
* Main logos can be updated from the regular content admin panel but on heroku you cannot keep your files
* To keep images you need a s3 account or a cloudinary heroku plugin, not on heroku itself. I already gave my card to heroku, and amazon wanted my credit card info also even for free account (limited to 1year 5go. I opted for cloudinary as part of heroku that had already my info, for only 500Mo but with no time limit. It gives less time pressure, and privacy intrusiveness that way. In Spree based openfoodnetwork platform, they use paperclip we can configure to use s3 account.
* So here is how you configure cloudinary (see also heroku doc on cloudinary plugin) :
* First, I added the plug in to heroku from web interface
* I then added the env variables
* I added gem paperclip-cloudinary in gem file
* Get your cloundinary.yml from settings page in cloudinary website 
* Add in production.rb cloudinary config :
 ` config.paperclip_defaults = {`
	  `storage: :cloudinary,`
	  `path: 'public/images/:class/:attachment/:id/:style/:filename.:extension',`
	  `url: '/images/:class/:attachment/:id/:style/:filename.:extension',`
	  `cloudinary_credentials: Rails.root.join("config/cloudinary.yml"),`
	  `secure: true, `
  `}`
* The logo and bg picture should work then. But for entrepise picture to work, you have as well to add to spree.rb cloudinary config. I later removed it, and it didn’t stop entreprise images from uploading and showing correctly : 
`attachment_config={`
  `storage:     :cloudinary,`
  `url:         "/public/images/:class/:attachment/:id/:style/:filename.:extension",`
  `path:        "public/images/:class/:attachment/:id/:style/:filename.:extension",`
  `default_url: "/public/images/:class/:attachment/:id/:style/:filename.:extensionn",`
  `secure: true`
  `}`
  `attachment_config.each do |key,value|`
   `Spree::Image.attachment_definitions[:attachment][key.to_sym] = value`
  `end`
* For product pictures to show up in shop page, I also had to Change image_serializer.rb and replace “false” by “:timestamp => false”
## Others
If you have few producers, the filter button (with “organic” taxons, and so on) is more important than name/location search, so I decreased the size of the search label in _hubs.html.haml line 6. I also restored Dalli, now that I had to put my CB info. Faster working.
* First add the plugin from web interface
* Then export env variable to heroku (they should be already there)
* Add this to your production.rb
   config.cache_store = :dalli_store,
                    (ENV["MEMCACHIER_SERVERS"] || "").split(","),
                    {:username => ENV["MEMCACHIER_USERNAME"],
                     :password => ENV["MEMCACHIER_PASSWORD"],
                     :failover => true,
                     :socket_timeout => 1.5,
                     :socket_failure_delay => 0.2,
                     :down_retry_delay => 60
                    }
## Almost ready to go public
That’s very exciting, It’s almost ready to go public with 0 euro cost (but with CB info identification). Now I have to setup the first shop and start inviting the small producers/shops/hubs. Although the process of managing product is still complex but highly refined, it is a unique opportunity for them to start an e-shop for free with little technical knowledge, as well as to stick together as producer community. Hope these instructions helps, don’t hesitate to improve them, this is a wiki after all.

 I will report back with more tips on promoting, recruiting and helping the new users.

## Heroku stopped to support ruby 2.1.5 in march-april 2017
Ok, this is bad news, because it is the ruby version of the main OFN branch. So you'll have to switch the gem to ruby 2.2.5 (and add minitest and upgrade pd to 0.15). But this comes with many unexpected errors (undefined method `name' for nil:NilClass), I lost so much time diagnosing before remembering about the ruby version change. Cause is incompatibilities of Ruby 2.2.5 with Rails 3.2.21. You have several solutions, the simplest being to switch to [rails branch 3-2-stable](https://github.com/rails/rails/tree/3-2-stable) and for that you have to add the github to the gem file. I hope they wont do this every 3 months, stop supporting old ruby versions. I am not sure it will be possible to keep running OFN if they do that.