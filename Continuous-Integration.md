Continuous Integration testing (CI) ensures that all automated tests are run each time somebody pushes code to the GitHub repository. We are currently using GitHub Actions to run our test suite. The tests should run automatically on your fork and on any PR that you submit.

## Buildkite

Buildkite provides a nice user interface to manage your own CI servers. The Australian team uses it a bit differently. We use the user interface to trigger deployments to staging and production servers. It uses the Github API to check if other CI runs were successful. It is a paid service. See [Deployment with Buildkite](https://github.com/openfoodfoundation/ofn_deployment/wiki/Deployment-with-Buildkite) for more information.