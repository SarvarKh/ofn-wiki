## Intro
Karma is used to run AngularJS specs.

## Getting set up

### On OS X Mavericks
Annoyingly, we need to install the Node.js Package Manage (npm) to install karma. Amusingly, one can install npm with homebrew, and vice-versa. I chose to install npm with homebrew:

    $ brew update
    $ brew install npm

Once that is complete, you should be able to jump straight to installing karma. NOTE: At present I would recommend using the global flag (-g) for installing karma packages with npm, as we have not yet set up a npm package.json file to manage versioning and dependencies within the ofn project.

    $ brew update
    $ npm install -g karma

Next we need a bunch of other packages (as they are now split up into separately rather than being contained in one all-inclusive package): the jasmine BDD testing framework, the coffee preprocessor and a chrome-launcher:

    $ npm install karma-jasmine
    $ npm install karma-coffee-preprocessor
    $ npm install karma-chrome-launcher

You will likely need to install the karma command line packages as well:

    $ sudo npm install -g karma-cli

And then you should be ready to go (see next section)....

## Usage of Karma
Running AngularJS specs with karma has been made pretty simple through use of a rake task which pulls together the karma configuration file (`config/ng-test.conf.js`), and a manifest file (`spec/javascripts/application_spec.js`) and then passes them on to karma to run:

    $ rake karma:start

or if you are using zeus:

    $ zeus rake karma:start