You can follow the generic OSX setup guide [here](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Development-Environment-Setup%3A-OS-X). This page contains version specific setup details.





## Step 8: Installing And Setting up Postgres
We now have ruby and rails installed, but we still require postgres, which can be installed with Homebrew:

    $ brew update
    $ brew install postgres

You should now be able to get launchd to start the postgres server (and restart as login) with:

    $ brew services start postgresql

Postgres can be stopped with: `$ brew services stop postgresql` but don't do this now.

## Step 9. Adding OFN users (roles) to postgres
Postgres should have been set up with your current user as the default postgres admin user, meaning that you should be able to set up new postgres users directly in the command prompt. YOU MAY NEED TO RESTART TERMINAL HERE. You are good to go if: 

    $ which createuser

gives you something like: `/usr/local/bin/createuser`

We need to add the database user(s) defined in the config/database.yml file in the OFN project repository. At the time of writing the user was `ofn` and the password was `f00d`. You can add a postgres user using:

    $ createuser --superuser --pwprompt ofn


Which will create a user named `ofn` and then prompt the user for the password when run. Make sure you enter the password exactly as it appears in the database.yml file. The reason that the --superuser is required is that we are using foreign keys which are registered in postgres as triggers, which in turn can only be added and removed by superusers.

Next add the development and test databases (open_food_network_dev and open_food_network_test at the time of writing):

    $ createdb open_food_network_dev --owner=ofn
    $ createdb open_food_network_test --owner=ofn

Once that is done, you can move on to cloning the OFN repository!

## Step 10: Cloning the OFN GitHub Repository
First you need to work out where on your hard drive the OFN project folder is going to live. I usually put all of mine in `~/projects`, but it really doesn't matter all much, as long as you know where it is.

Create a projects folder with something like: 

    $ mkdir -p ~/projects

and then navigate into it with:

    $ cd ~/projects

Now we are ready to clone the OFN repository into your projects folder:

    $ git clone https://github.com/openfoodfoundation/openfoodnetwork.git

Enter your newly cloned repository folder with:

    $ cd openfoodnetwork

You should see a message about RVM setting up some new gemsets for the OFN project.

## Step 11: Install the bundle

The version of bundler that rvm installed for me was quite old, so when I ran `bundle -v`, I got: `1.7.6`. I updated the version of bundler using:

    $ gem update bundler

And now, `bundle -v` gives me: `Bundler version 1.17.2`. 👍 

You may need to run `gem install bundler` to install the bundler in the first place.

You should now be able to install the required gems with: 

    $ bundle install

Once all gems have been installed, check that rails is ready to go:

    $ bundle exec rails -v

## Step 12: Configure your local OFN app

Make sure you have a valid application.yml file (you should edit locale, currency, etc as required):

    $ cp config/application.yml.example config/application.yml

## Step 13: Build the database and add some basic data

Set up the database(s):

    $ bundle exec rake db:schema:load

You will probably want to insert some seed data into the database so that the server has all the things it needs to start up:

    $ bundle exec rake db:seed

Fire up your server:

    $ bundle exec rails s

Go to [http://localhost:3000](http://localhost:3000) to play around!

## Step 14. Other things you should install

Headless Chrome: make sure Google Chrome is installed so that you can run feature tests.

ImageMagick: used by Spree to create and manipulate images

    $ brew update
    $ brew install imagemagick

Karma: a test runner for pure javascript (which we use to test our AngularJS)

[Karma Guide](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Karma)

## Troubleshooting

If you run into an issue with the libv8 gem after running the bundle command:

```
$ gem install libv8 -v ‘3.16.14.11’ -- --with-system-v8
```
or
```
$ gem install libv8 -v 3.16.14.11 -- --with-system-v8
```

If this doesn't solve the error, you can try these commands (with libv8 3.16.14.11 and therubyracer 0.12.0):

    $ gem uninstall libv8
    $ gem install therubyracer -v YOUR_RUBY_RACER_VERSION
    $ gem install libv8 -v YOUR_VERSION -- --with-system-v8

The previous commands were useful in OFN v1 with therubyracer and OSX HighSierra.
In OFN v2 we use mini_racer and problems with mini_racer installation (usually on ruby upgrade or osx update) can be solved by following this guide:
https://blog.driftingruby.com/updated-to-mojave/
or this stack overflow post:
https://stackoverflow.com/questions/34617452/how-to-update-xcode-from-command-line
If this doesnt work you can try to download Command Line Tools from
https://developer.apple.com/download/more/ (search for "Command Line Tools")
and install them manually.