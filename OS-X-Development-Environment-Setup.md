## Intro
This is a rough guide to getting a development environment set up on a clean OS X Mavericks install. I make no claim whatsoever that this is the best or only way to go about this process, but I thought that I would document the strategy that worked for me in case it of use to others. While I have made an attempt to make this guide as beginner friendly as possible, users should be very comfortable with the use of command line before attempting to follow these steps, as every step requires use of the command line.

## Step 1. XCode / Command Line Tools
XCode is Apple's suite of OS X and iOS development tools, and provides a whole IDE geared towards that purpose. My understanding is that the only component of XCode that is particularly pertinent to rails development on OS X is the GCC compiler system. It used to be the case that one had to download and install the entire XCode suite in order to get the useful bits, but now Apple provides a condensed 'useful bits' package in the form of Command Line Tools (CLT).

First, check to see if gcc is available:

    $ gcc --version

If not, you'll have to install CLT. You can try to install a package which depends on CLT to prompt CLT to install. A example commonly used on the interwebs is xcode-select:

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
Homebrew is the 'missing package manager' for OS X. Some people don't like it because it encourages a lack of understanding about where and how packages are installed/configured, which can lead to difficulties in debugging if something goes wrong. I tend to like it because it is quick and easy to use, and I generally have more confidence in other people's ability to put together a Homebrew package in an intelligent way, than I do in my own ability to hack together a solution.

At the time of writing, the produce to install Homebrew on your system is listed on the [Homebrew Homepage](http://brew.sh/). If what I have below doesn't work, it may be worth checking back there to see if the procedure has changed. Anyway, this worked for me (note that you will be prompted for your password):

    $ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

If successful you should get it should tell you to run `brew doctor`, so do that:

    $ brew doctor

When that is done you should get: `Your system is ready to brew`, YAY!

## Step 3. Git
Git is the version control system used to manage all the independent code contributions made by awesome people like you! To before installing anything with Homebrew you should update your package lists with:

    $ brew update

Now we can install Git with:

    $ brew install git

And checked that everything is still ok with:

    $ brew doctor

If you get an error (as I did) that says: `Warning: /usr/bin occurs before /usr/local/bin`, it means that you probably have installed the full XCode suite and the default version of Git that ships with XCode is stilling in /usr/bin, meaning our new version of Git that we are installing with Homebrew will not be used because as the error tells us: /usr/bin occurs before /usr/local/bin in the PATH. The following command will fix that by adding a line to ~/.bash_profile.

    $ echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.bash_profile

Refresh your bash profile by entering:

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

To install RVM and the latest version of ruby and rails, which is probably a good idea, use the following command. NOTE. if you REALLY don't want RVM to install anything expect RVM, remove the --rails flag.

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

## Step 7. Installing the version of ruby we require
Our setup will probably require a specific version of Ruby which is unlikely to be the one installed automatically by RVM above, so we will need to install it. At the time of writing, the the ruby version required was stored in a file in the root folder of the Open Food Network project repository called .ruby-version. The version I needed was `1.9.3-p392`, so I installed it into RVM with:

    $ rvm install 1.9.3-p392

Unfortunately I am not 100% across what happened next, but basically my system decided that it needed to compile that version of ruby for itself. I suspect that this was because that version is longer officially supported, and so no precompiled binary of that ruby version was available. Whatever the cause, the compilation process took about 5 hours to complete on my MacBook Air. My understanding is that we are looking into an upgrade of ruby in the near future, and so hopefully this will not be an issue for you.

When that is done, you should be ready to actually get the repository and start using it!

## Step 8: Installing Dependencies
We now have ruby and rails installed, but we still require a couple of extra packages before anything will work. The main two are the Postgres database, and PhantomJS so support JS testing. Both can be installed with Homebrew:

    $ brew update
    $ brew install phantomjs
    $ brew install postgres

## Step 9: Setting up postgres
PhantomJS should be fine to just be left alone after installing, but postgres needs a bit of love to get up and running. The internet recommended that I use [Lunchy](https://github.com/eddiezane/lunchy), to help manage postgres:

    $ brew install lunchy
    $ brew doctor`

When that is done, make sure you have a LaunchAgents folder:

    $ mkdir -p ~/Library/LaunchAgents

And then populate it with symbolic links back to the postgres config file(s) installed by homebrew:

    $ ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents

You should now be able to start and stop the postgres server as you please with:

    $ lunchy start postgres

and

    $ lunchy stop postgres

## Step 10. Adding OFN users (roles) to postgres
Postgres should have been set up with your current user as the default user, meaning that you should be able to access the postgres console using:

    $ psql postgres

If that doesn't work you can try:

    $ sudo -u your_user_name psql postgres

but I don't really see how that would work any better.

If homebrew set up postgres with its own postgres user, something like this might work:

    $ psql -U postgres postgres

Whichever way, once you gain access to the console you can create the user (role) required for the Open Food Network to run properly. These are documented in the config/database.yaml file in the OFN project repository. Note that I am prefixing commands to be entered into the postgres console with '#', you do not need to enter this, I am just using it do distinguish those commands from regular bash commands ($):

    # CREATE USER ofn WITH PASSWORD 'f00d';

Now the databases:

    # CREATE DATABASE open_food_network_dev;
    # CREATE DATABASE open_food_network_test;

Now grant access:

    # GRANT ALL PRIVILEGES ON DATABASE open_food_network_dev TO ofn;
    # GRANT ALL PRIVILEGES ON DATABASE open_food_network_test TO ofn;

Exit the postgres console:

    $ exit

That should be it for database setup!

## Step 11: Cloning the OFN GitHub Repository
First you need to work out where on you hard drive the OFN project folder is going to live. I usually put all of mine in `~/projects`, but it really doesn't matter all much, as long as you know where it is.

If you don't already have a project folder use: 

    $ mkdir ~/projects

to create one and then navigate into it with:

    $ cd ~/projects

Now we are ready to clone the OFN repository:

    $ git clone https://github.com/openfoodfoundation/openfoodnetwork.git

Enter your new cloned repository folder with:

    $ cd openfoodnetwork

You will probably get a message about RVM setting up some new gemsets for the OFN project, that is fine.

## Step 12: Final steps

Install the required gems using:

    $ bundle install

Set up the database(s):

    $ rake db:test:prepare

You will probably want to insert some seed data into the database so that the server has all the things it needs to start up:

    $ rake db:seed

## Step 13. Fire up your server

Fire it up:

    $ rails s

Go to [http://localhost:3000](http://localhost:3000) to play around!