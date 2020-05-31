You can follow the generic OSX setup guide [here](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Development-Environment-Setup%3A-OS-X). This page contains version specific setup details.

## Installing RVM in OSX v10.9 Mavericks

To install RVM and the latest version of ruby and rails, which is probably a good idea, use the following command. NOTE: if you REALLY don't want RVM to install anything expect RVM, remove the --rails flag.

    $ curl -L https://get.rvm.io | bash -s stable --rails --autolibs=enable

When that is done you should be able to restart your terminal and run:

    $ type rvm | head -1

Which should give you `rvm is a function` if everything went well.

## Manage postgres with Lunchy

You can manage postgres with [Lunchy](https://github.com/eddiezane/lunchy):

    $ gem install lunchy

When that is done, make sure you have a LaunchAgents folder:

    $ mkdir -p ~/Library/LaunchAgents

And then populate it with symbolic links back to the postgres config file(s) installed by homebrew:

    $ ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents

You should now be able to start the postgres server with:

    $ lunchy start postgres

Postgres can be stopped with: `$ lunchy stop postgres` but don't do this now.






## Step 10. Adding OFN users (roles) to postgres
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

## Step 11: Cloning the OFN GitHub Repository
First you need to work out where on your hard drive the OFN project folder is going to live. I usually put all of mine in `~/projects`, but it really doesn't matter all much, as long as you know where it is.

If you don't already have a project folder use: 

    $ mkdir ~/projects

to create one and then navigate into it with:

    $ cd ~/projects

Now we are ready to clone the OFN repository into your projects folder:

    $ git clone https://github.com/openfoodfoundation/openfoodnetwork.git

Enter your new cloned repository folder with:

    $ cd openfoodnetwork

You will probably get a message about RVM setting up some new gemsets for the OFN project, that is fine.

## Step 12: Final steps

Install the required gems using:

    $ bundle install

Set up the database(s):

    $ rake db:schema:load

You will probably want to insert some seed data into the database so that the server has all the things it needs to start up:

    $ rake db:seed

Fire up your server:

    $ rails s

Go to [http://localhost:3000](http://localhost:3000) to play around!

## Step 12.5 Resolving bundler problems

With OSX Mavericks, you may encounter an error installing the libv8 gem. This can be solved by using the system v8 library rather than compiling from scratch, as happens by default. To do so, use these commands, replacing the version for libv8 below with the one displayed in the error message from bundler:

    gem uninstall libv8
    brew install v8
    gem install therubyracer
    gem install libv8 -v '3.16.14.3' -- --with-system-v8


## Step 13. Other things you could think about installing

ImageMagick, used by Spree to create and manipulate images:

    $ brew update
    $ brew install imagemagick

[Zeus](https://github.com/burke/zeus), Rails preloader, so you don't have to reload the entire Rails stack all the time:

    $ gem install zeus

Karma, a test runner for pure javascript (which we use to test our AngularJS):

[Karma Guide](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Karma)

[Mail Setup]
One of the key setup required is to setup the mail server, to get the confirmation mail, so that the enterprises become active, and we do the end to end setup.
Any easy way of setting this up ? I tried using postfix, but having issues configuring gmail to send and receive mails.