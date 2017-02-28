I am working on setting OFN Greece. Here is how I deployed openfoodnetwork on hekoru from ubuntu. I am ruby/rails newbie, be indulgent :

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

Now I/You need to setup everything to be not only running but usable by my/your local producers. I will report back with instructions on this step upon completion.  
