## Intro
This is a rough guide to getting a development environment set up on a clean maxOS Sierra install. The process has only altered slightly from that which works for [OS X El Capitan](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Development-Environment-Setup:-OS-X-(El-Capitan)).

## Step 1. XCode / Command Line Tools
XCode is Apple's suite of OS X and iOS development tools, and provides a whole IDE geared for that purpose. My understanding is that the only component of XCode that is particularly pertinent to rails development on OSX is the GCC compiler system. It used to be the case that one had to download and install the entire XCode suite in order to get the useful bits, but now Apple provides a condensed 'useful bits' package in the form of Command Line Tools (CLT).

First, check to see if gcc is available on your system already:

    $ gcc --version

If it is not, you should be prompted to install Command Line Tools. Run through the prompts to download and install it. This took about 4 minutes for me.

When that process has finished, you should get a sensible answer out of:

    $ gcc --version

My output for this is something like:

    Configured with: --prefix=/Library/Developer/CommandLineTools/usr --with-gxx-include-dir=/usr/include/c++/4.2.1
    Apple LLVM version 8.1.0 (clang-802.0.42)
    Target: x86_64-apple-darwin16.6.0
    Thread model: posix
    InstalledDir: /Library/Developer/CommandLineTools/usr/bin


Now we can move on to installing the interesting things!

## Step 2. Homebrew
Homebrew is the 'missing package manager' for OS X. Some people don't like it because it encourages a lack of understanding about where and how packages are installed/configured, which can lead to difficulties in debugging if something goes wrong. That is a valid concern, but I tend to like it because it is quick and easy to use, and I generally have greater confidence in other people's ability to put together a Homebrew package in an intelligent way, than I do in my own ability to hack together a solution.

At the time of writing, the process to install Homebrew is listed on the [Homebrew Homepage](http://brew.sh/). If what I have below doesn't work, it may be worth checking back there to see if the procedure has changed. Anyway, this worked for me (note that you will be prompted for your password):

    $ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

When that is done, run `brew doctor` to check the status of your homebrew install:

    $ brew doctor

If successful, you should get: `Your system is ready to brew`, YAY!

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

At the time of writing, the process to download and install RVM uses [gpg](https://en.wikipedia.org/wiki/GNU_Privacy_Guard) to verify the integrity of the RVM installer. It might be worth checking the [RVM homepage](https://rvm.io/) to ensure this is still the correct process to follow. You can install gpg using Homebrew:

    $ brew install gpg
    $ brew doctor

Download the public key for the RVM installer (again, probably worth checking the [RVM homepage](https://rvm.io/) for up to date instructions):

    $ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

If you get `gpg: keyserver receive failed: No keyserver available`, try a different server. eg:

    $ gpg --keyserver hkp://pgp.mit.edu --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

To install RVM without any rubies or gems (which is what I wanted to do) use the following command. NOTE: RVM can globally install the latest versions of ruby and rails for you here, see the [RVM install docs](https://rvm.io/rvm/install) for more information. NOTE: the leading backslash is used to directly call the curl binary and ignore any alias for 'curl' you may have set up.

    $ \curl -sSL https://get.rvm.io | bash -s stable --autolibs=enable

When that is done you should be able to restart your terminal OR load the RVM binary into your shell with:

    $ source ~/.rvm/scripts/rvm

Then run:

    $ type rvm | head -1

Which should give you `rvm is a function` if everything went well.

## Step 6. Installing the required version of ruby
The OFN will require a specific version of Ruby. At the time of writing, the the ruby version required was stored in a file in the root folder of the Open Food Network project repository called [.ruby-version](https://github.com/openfoodfoundation/openfoodnetwork/blob/master/.ruby-version). The version I needed was `2.1.5`, so I installed it into RVM with:

    $ rvm install 2.1.5

## Step 7. Verify environment

Run each of the following just to check that you get sensible numbers out of each of them:

    $ git --version
    $ rvm -v
    $ ruby -v

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

And now, `bundle -v` gives me: `Bundler version 1.15.1`. 👍 

You should now me able to install the required gems with: 

    $ bundle install

Once all gems have been installed, check that rails is ready to go:

    $ rails -v

## Step 12: Configure your local OFN app

Make sure you have a valid application.yml file (you should edit locale, currency, etc as required):

    $ cp config/application.yml.example config/application.yml

## Step 13: Build the database and add some basic data

Set up the database(s):

    $ rake db:schema:load

You will probably want to insert some seed data into the database so that the server has all the things it needs to start up:

    $ rake db:seed

Fire up your server:

    $ rails s

Go to [http://localhost:3000](http://localhost:3000) to play around!

## Step 14. Other things you should install

PhantomJS: to support JS testing (unit tests and rails feature specs). The following should perform a install version `2.1.1` into your path:

    $ brew update
    $ brew install phantomjs

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