## Intro
This is a rough guide to getting a development environment set up on a clean OSX El Capitan install. The process has only altered very slightly from that which works for [OSX Mavericks](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Development-Environment-Setup:-OS-X-(Mavericks)).

## Step 1. XCode / Command Line Tools
XCode is Apple's suite of OS X and iOS development tools, and provides a whole IDE geared for that purpose. My understanding is that the only component of XCode that is particularly pertinent to rails development on OSX is the GCC compiler system. It used to be the case that one had to download and install the entire XCode suite in order to get the useful bits, but now Apple provides a condensed 'useful bits' package in the form of Command Line Tools (CLT).

First, check to see if gcc is available on your system already:

    $ gcc --version

If it is not, you should be prompted to install Command Line Tools. Run through the prompts to download and install it.

When that process has finished, you should get a sensible answer out of:

    $ gcc --version

My output for this is something like:

    Configured with: --prefix=/Library/Developer/CommandLineTools/usr --with-gxx-include-dir=/usr/include/c++/4.2.1
    Apple LLVM version 7.0.2 (clang-700.1.81)
    Target: x86_64-apple-darwin15.0.0
    Thread model: posix


Now we can move on to installing the interesting things!

## Step 2. Homebrew
Homebrew is the 'missing package manager' for OS X. Some people don't like it because it encourages a lack of understanding about where and how packages are installed/configured, which can lead to difficulties in debugging if something goes wrong. That is a valid concern, but I tend to like it because it is quick and easy to use, and I generally have greater confidence in other people's ability to put together a Homebrew package in an intelligent way, than I do in my own ability to hack together a solution.

At the time of writing, the process to install Homebrew is listed on the [Homebrew Homepage](http://brew.sh/). If what I have below doesn't work, it may be worth checking back there to see if the procedure has changed. Anyway, this worked for me (note that you will be prompted for your password):

    $ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

If successful it should tell you to run `brew doctor`, so do that:

    $ brew doctor

When that is done you should get: `Your system is ready to brew`, YAY!

## Step 3. Git
Git is the version control system used to manage all the independent code contributions made by awesome people like you! So before installing anything with Homebrew you should update your package lists with:

    $ brew update

Now we can install Git with:

    $ brew install git

And check that everything is still ok with:

    $ brew doctor

When you check which version of git you are using:

    $ which git

You should see: `/usr/local/bin/git`

If that has all worked, you can now set up your git credentials with:

    $ git config --global user.name "Your Name Here"
    $ git config --global user.email "email@address.here"

## Step 4 (Optional). Disabling documentation installation

If you don't ever use docs for gems (I don't), you can disable installation of documentation with:

    $ echo "gem: --no-document" >> ~/.gemrc

## Step 5. RVM
Ruby Version Manager is a very useful tool which facilitates the management of ruby configuration on a per-project basis, allowing any number of setups using different versions of ruby, rails or gems to be used on the same machine relatively seamlessly.

At the time of writing, you must now use [gpg](https://en.wikipedia.org/wiki/GNU_Privacy_Guard) to verify the integrity of the RVM installer. Might be worth checking [here](https://rvm.io/) to ensure this is still the correct process to follow. You can install gpg using Homebrew:

    $ brew install gpg
    $ brew doctor

Download the public key for the RVM installer:

    $ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3

To install RVM and the latest version of ruby and rails, which is probably a good idea, use the following command. NOTE: if you REALLY don't want RVM to install anything expect RVM, remove the --rails flag. NOTE: the leading backslash is used to directly call the curl binary and ignore any alias for 'curl' you may have set up.

    $ \curl -sSL https://get.rvm.io | bash -s stable --rails --autolibs=enable

When that is done you should be able to restart your terminal OR load the rvm binary into your shell with:

    $ source /Users/rob/.rvm/scripts/rvm

Then run:

    $ type rvm | head -1

Which should give you `rvm is a function` if everything went well.

## Step 6. Verify environment

Run each of the following just to check that you get sensible numbers out of each of them:

    $ git --version
    $ rvm -v
    $ ruby -v
    $ rails -v

## Step 7. Installing the required version of ruby
Our setup will probably require a specific version of Ruby which is unlikely to be the one installed automatically by RVM above, so we will need to install it. At the time of writing, the the ruby version required was stored in a file in the root folder of the Open Food Network project repository called `.ruby-version`. The version I needed was `2.1.5`, so I installed it into RVM with:

    $ rvm install 2.1.5

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

Install the required gems using:

    $ bundle install

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