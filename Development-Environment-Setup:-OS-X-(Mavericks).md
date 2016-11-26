## Intro
This is a rough guide to getting a development environment set up on a clean OSX Mavericks install. I make no claim whatsoever that this is the best or only way to go about this process, but I thought that I would document the strategy that worked for me in case it is of use to others. While I have made an attempt to make this guide as accessible as possible, users really need to be quite comfortable with the use of command line before attempting to follow these steps.

## Step 1. XCode / Command Line Tools
XCode is Apple's suite of OS X and iOS development tools, and provides a whole IDE geared for that purpose. My understanding is that the only component of XCode that is particularly pertinent to rails development on OSX is the GCC compiler system. It used to be the case that one had to download and install the entire XCode suite in order to get the useful bits, but now Apple provides a condensed 'useful bits' package in the form of Command Line Tools (CLT).

First, check to see if gcc is available on your system already:

    $ gcc --version

If not, you'll have to install CLT. Attempting to install a package which depends on CLT is one way to prompt CLT to install: a example commonly used on the interwebs is xcode-select:

    $ xcode-select --install

You should be prompted to install Command Line Tools. Run through the prompts to download and install it.

When that process has finished, you should get a sensible answer out of:

    $ gcc --version

My output for this is something like:

    Configured with: --prefix=/Library/Developer/CommandLineTools/usr --with-gxx-include-dir=/usr/include/c++/4.2.1
    Apple LLVM version 5.1 (clang-503.0.40) (based on LLVM 3.4svn)
    Target: x86_64-apple-darwin13.3.0
    Thread model: posix

Now we can move on to installing the interesting things!

## Step 2. Homebrew
Homebrew is the 'missing package manager' for OS X. Some people don't like it because it encourages a lack of understanding about where and how packages are installed/configured, which can lead to difficulties in debugging if something goes wrong. That is a valid concern, but I tend to like it because it is quick and easy to use, and I generally have greater confidence in other people's ability to put together a Homebrew package in an intelligent way, than I do in my own ability to hack together a solution.

At the time of writing, the produce to install Homebrew is listed on the [Homebrew Homepage](http://brew.sh/). If what I have below doesn't work, it may be worth checking back there to see if the procedure has changed. Anyway, this worked for me (note that you will be prompted for your password):

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

If you get an error (as I did) that says: `Warning: /usr/bin occurs before /usr/local/bin`, it means that you probably have installed the full XCode suite and the default version of Git that ships with XCode is stilling in /usr/bin, meaning our new version of Git that we are installing with Homebrew will not be used because as the error tells us: /usr/bin occurs before /usr/local/bin in the PATH. The following command will fix that by adding a line to ~/.bash_profile.

    $ echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.bash_profile

Refresh your .bash_profile by entering:

    $ . ~/.bash_profile

So now when you check which version of git you are using:

    $ which git

You should see: `/usr/local/bin/git`

If that has all worked, you can now set up your git credentials with:

    $ git config --global user.name "Name_Goes_Here"
    $ git config --global user.email "Email_Address_Goes_Here"

## Step 4 (Optional). Disabling documentation installation

If you don't ever use docs for gems (I don't), you can disable installation of documentation with:

    $ echo "gem: --no-document" >> ~/.gemrc

## Step 5. RVM
Ruby Version Manager is a very useful tool which facilitates the management of ruby configuration on a per-project basis, allowing any number of setups using different versions of ruby, rails or gems to be used on the same machine relatively seamlessly.

To install RVM and the latest version of ruby and rails, which is probably a good idea, use the following command. NOTE: if you REALLY don't want RVM to install anything expect RVM, remove the --rails flag.

    $ curl -L https://get.rvm.io | bash -s stable --rails --autolibs=enable

When that is done you should be able to restart your terminal and run:

    $ type rvm | head -1

Which should give you `rvm is a function` if everything went well.

## Step 6. Verify environment

Run each of the following just to check that you get sensible numbers out of each of them:

    $ git --version
    $ rvm -v
    $ ruby -v
    $ rails -v

## Step 7. Installing the required version of ruby
Our setup will probably require a specific version of Ruby which is unlikely to be the one installed automatically by RVM above, so we will need to install it. At the time of writing, the the ruby version required was stored in a file in the root folder of the Open Food Network project repository called .ruby-version. The version I needed was `1.9.3-p392`, so I installed it into RVM with:

    $ rvm install 1.9.3-p392

note : As of 26/11/2016 the current version is 2.1.5.

Unfortunately I am not 100% across what happened next, but basically my system decided that it needed to compile that version of ruby for itself. I suspect that this was because that version is longer officially supported, and so no precompiled binary of that ruby version was available. Whatever the cause, the compilation process took about 4 hours to complete on my MacBook Air. My understanding is that we are looking into an upgrade of ruby in the near future, and so hopefully this will not be an issue for you.

## Step 8: Installing Dependencies
We now have ruby and rails installed, but we still require a couple of extra packages before anything will work. The main two are the Postgres database, and PhantomJS to support JS testing. Both can be installed with Homebrew:

    $ brew update
    $ brew install phantomjs
    $ brew install postgres

## Step 9: Setting up postgres
PhantomJS should be fine to just be left alone after installing, but postgres needs a bit of love to get up and running. The internet recommended that I use [Lunchy](https://github.com/eddiezane/lunchy), to help manage postgres:

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