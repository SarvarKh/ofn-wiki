## Intro
Karma is used to run AngularJS specs.

## Getting set up

### On OS X Mavericks
We use an npm package.json to install karma and associated testing packages. Amusingly, one can install npm with homebrew, and vice-versa. I chose to install npm with homebrew:

    $ brew update
    $ brew install npm

Our Karma setup requires PhantomJS. The package.json file specifies a version of PhantomJS, but this will simply create a link to any existing version of PhantomJS in the path that matches the version requirements.

You should now be able to jump straight into installing the npm packages. These are Karma, PhantomJS and Chrome launchers, CoffeeScript preprocessor and the Jasmine testing framework.

    $ npm install

Check that NodeJS is installed. If you install from a package manager your bin may be called nodejs so you just need to symlink it:

    $ ln -s /usr/bin/nodejs /usr/bin/node

And then you should be ready to go (see 'Usage of Karma')....

### On Debian 10

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash
. .bashrc # or open new terminal
nvm install 12
npm install
```

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

    $ bundle exec rake karma:start