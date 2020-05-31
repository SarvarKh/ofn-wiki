You can follow the generic OSX setup guide [here](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Development-Environment-Setup%3A-OS-X). This page contains version specific setup details.






## Step 8: Installing And Setting up Postgres
We now have ruby and rails installed, but we still require postgres, which can be installed with Homebrew:

    $ brew update
    $ brew install postgres

The internet recommended that I use [Lunchy](https://github.com/eddiezane/lunchy), to help manage postgres:

    $ gem install lunchy

When that is done, make sure you have a LaunchAgents folder:

    $ mkdir -p ~/Library/LaunchAgents

And then populate it with symbolic links back to the postgres config file(s) installed by homebrew:

    $ ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents

You should now be able to start the postgres server with:

    $ lunchy start postgres

Postgres can be stopped with: `$ lunchy stop postgres` but don't do this now.

## Step 9. Adding OFN users (roles) to postgres
Postgres should have been set up with your current user as the default postgres admin user, meaning that you should be able to set up new postgres users directly in the command prompt. YOU MAY NEED TO RESTART TERMINAL HERE. You are good to go if: 

    $ which createuser

gives you something like: `/usr/local/bin/createuser`

We need to add the database user(s) defined in the config/database.yml file in the OFN project repository. At the time of writing the user was `ofn` and the password was `f00d`. You can add a postgres user using:

    $ createuser --superuser --pwprompt ofn


Which will create a user named `ofn` and then prompt the user for the password when run. Make sure you enter the password exactly as it appears in the database.yaml file. The reason that the --superuser is required is that we are using foreign keys which are registered in postgres as triggers, which in turn can only be added and removed by superusers.

Next add the development and test databases (open_food_network_dev and open_food_network_test at the time of writing):

    $ createdb open_food_network_dev --owner=ofn
    $ createdb open_food_network_test --owner=ofn

Once that is done, you can move on to cloning the OFN repository!

## Step 10: Cloning the OFN GitHub Repository
First you need to work out where on your hard drive the OFN project folder is going to live. I usually put all of mine in `~/projects`, but it really doesn't matter all much, as long as you know where it is.

If you don't already have a project folder use: 

    $ mkdir ~/projects

to create one and then navigate into it with:

    $ cd ~/projects

Now we are ready to clone the OFN repository into your projects folder:

    $ git clone https://github.com/openfoodfoundation/openfoodnetwork.git

Enter your newly cloned repository folder with:

    $ cd openfoodnetwork

You will probably get a message about RVM setting up some new gemsets for the OFN project, that is fine.

## Step 11: Final steps

Apple has stopped maintaining the OpenSSL headers in OS X, and so these must be installed with Homebrew, seeing as we have a bunch of gems that need up-to-date openssl headers. Homebrew had already installed OpenSSL as a [keg-only dependency](http://stackoverflow.com/questions/17015285/understand-homebrew-and-keg-only-dependencies) on my system, and so all I needed to do was to add appropriate symlinks using:

    $ brew link --force openssl

Homebrew complains very forcefully about this, so an alternative way is to configure bundler with the appropriate compiler flags (shown in the output of the brew link command above) for the gems that need it - for me this was only eventmachine (Steve, November 2016):

    $ bundle config build.eventmachine --with-cppflags="-I$(brew --prefix openssl)/include" --with-ldflags="-L$(brew --prefix openssl)/lib"

Now you should be able to install all of the gems required for the openfoodnetwork project using:

    $ bundle install

Make sure you have a valid application.yml file (you should edit locale, currency, etc this as required):

    $ cp config/application.yml.example config/application.yml

Set up the database(s):

    $ rake db:schema:load

You will probably want to insert some seed data into the database so that the server has all the things it needs to start up:

    $ rake db:seed

Fire up your server:

    $ rails s

Go to [http://localhost:3000](http://localhost:3000) to play around!

## Step 12. Other things you should probably install

PhantomJS: to support JS testing.
NOTE: At the time of writing, a PhantomJS release supporting El Capitan is yet to be released (see [#42249](https://github.com/Homebrew/homebrew/issues/42249) and [#12970](https://github.com/ariya/phantomjs/issues/12970)), though one is expected in the near future. Until that time, I recommend the use of TravisCI (which the OFN repository comes configured to use) for testing purposes. When a PhantomJS release does become available, all that should be required is to:

    $ brew install phantomjs

ImageMagick: used by Spree to create and manipulate images

    $ brew update
    $ brew install imagemagick

[Zeus](https://github.com/burke/zeus): Rails preloader, so you don't have to reload the entire Rails stack all the time

    $ gem install zeus

Karma: a test runner for pure javascript (which we use to test our AngularJS)

[Karma Guide](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Karma)