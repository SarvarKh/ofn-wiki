Continuous Integration testing (CI) ensures that all automated tests are run each time somebody pushes code to the GitHub repository. We are currently using [Semaphore CI](https://semaphoreci.com/openfoodfoundation/openfoodnetwork-2/), because it seems to be faster and more reliable than its competitors. We used to run tests with [Travis](https://travis-ci.org/openfoodfoundation/openfoodnetwork/) and you can still use it on your own forks.

## Semaphore CI

Semaphore is configured within the project settings on the Semaphore website. Here is how we set it up and you can do the same for your fork.

Ruby 2.1.5 with database pg on the standard platform "Standard v1805.1".

Environment variables:
```sh
TZ=Australia/Melbourne
```

Setup script:
```sh
bundle install --deployment --path vendor/bundle
cp config/application.yml.example config/application.yml
RAILS_ENV=test bundle exec rake db:create db:schema:load
change-phantomjs-version 2.1.1
```

Job 1:
```sh
npm install -g npm@'3.8.8'
npm install
npm install -g karma-cli@0.1.2
RAILS_ENV=test bundle exec rake karma:run
bundle exec rake 'knapsack:rspec[--tag ~performance --format progress -p]'
```

Job 2:
```sh
bundle exec rake 'knapsack:rspec[--tag ~performance --format progress -p]'
```

Job 3:
```sh
bundle exec rake 'knapsack:rspec[--tag ~performance --format progress -p]'
```

Job 4:
```sh
bundle exec rake 'knapsack:rspec[--tag ~performance --format progress -p]'
```

Semaphore sets some environment variables automatically so that Knapsack executes a quarter of all tests in each job.


## Travis CI

Travis offers a free testing infrastructure for free software projects. All forks of the Open Food Network can use Travis CI. The `.travis` configuration file contains all commands to execute on a Travis server to run the tests.

https://travis-ci.org/openfoodfoundation/openfoodnetwork

## Buildkite

Buildkite provides a nice user interface to manage your own CI servers. The Australian team uses it to run tests and deploy to staging and production servers. But you have to pay for an account. See [Deployment with Buildkite](https://github.com/openfoodfoundation/ofn_deployment/wiki/Deployment-with-Buildkite) for more information.