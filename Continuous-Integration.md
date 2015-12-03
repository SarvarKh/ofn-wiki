Continuous Integration testing (CI) ensures that all automated tests are run each time somebody pushes code to the GitHub repository. Currently, we support two different CI systems.

* Travis CI
* Buildkite


## Travis CI

Travis offers a free testing infrastructure for free software projects. All forks of the Open Food Network can use Travis CI. The configuration `.travis` configuration files contains all commands to execute on a Travis server to run the tests.

## Buildkite

Buildkite provides a nice user interface to manage your own CI servers. The Australian team uses it to run tests and deploy to staging and production servers. But you have to pay for an account. See [Deployment with Buildkite](https://github.com/openfoodfoundation/ofn_deployment/wiki/Deployment-with-Buildkite) for more information.