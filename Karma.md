## Intro
Karma is used to run AngularJS specs.

## Getting set up

### On OS X Mavericks
Annoyingly, we need to install the Node.js Package Manage (npm) to install karma. Amusingly, one can install npm with homebrew, and vice-versa. I chose to install npm with homebrew:

    $ brew update
    $ brew install npm

Once that is complete, you should be able to jump straight to installing karma. NOTE: At present I would recommend using the global flag (-g) for installing karma packages with npm, as we have not yet set up a npm package.json file to manage versioning and dependencies within the ofn project.

    $ brew update
    $ npm install -g karma@0.12.31

Next we need a bunch of other packages (as they are now split up into separately rather than being contained in one all-inclusive package): the jasmine BDD testing framework, the coffee preprocessor and a chrome-launcher:

    $ # karma-jasmine from 0.2.x onward uses jasmine 2.x, which has breaking changes
    $ npm install karma-jasmine@0.1.5
    $ # karma-coffee-preprocessor will default to a newer version with breaking changes.
    $ npm install karma-coffee-preprocessor@0.1.0
    $ npm install karma-phantomjs-launcher@0.1.4
    $ npm install karma-chrome-launcher

You will likely need to install the karma command line packages as well:

    $ sudo npm install -g karma-cli@0.0.4

Check that NodeJS is installed. If you install from a package manager you bin may be called nodejs so you just need to symlink it:

    $ ln -s /usr/bin/nodejs /usr/bin/node

And then you should be ready to go (see next section)....


### On Ubuntu 12.04
The npm shipped with 12.04 is broken, giving errors like:
    Error: failed to fetch from registry: karma
    or
    Error: No compatible version found: karma

However, Chris Lea's PPA works. Here's how to install it:

    $ sudo add-apt-repository ppa:chris-lea/node.js 
    $ sudo apt-get update
    $ sudo apt-get install nodejs

From there, follow the instructions for OS X Mavericks.

## Usage of Karma
Running AngularJS specs with karma has been made pretty simple through use of a rake task which pulls together the karma configuration file (`config/ng-test.conf.js`), and a manifest file (`spec/javascripts/application_spec.js`) and then passes them on to karma to run:

    $ rake karma:start

or if you are using zeus:

    $ zeus rake karma:start