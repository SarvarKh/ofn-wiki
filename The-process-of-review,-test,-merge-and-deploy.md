The Open Food Network has many contributors around the world. Everybody is working on different code and still we want to merge it all together into the `master` branch at [Github](https://github.com/openfoodfoundation/openfoodnetwork/).

A core development team currently oversees all contributions to the master branch. A pull request goes through the following stages:

### 1. Change & Submit
Have a look at our [CONTRIBUTING.md](https://github.com/openfoodfoundation/openfoodnetwork/blob/master/CONTRIBUTING.md) for more information about picking up an issue, making changes to the codebase and submitting a PR.

### 2. Review
Pull requests need to be reviewed and approved by at least two members of the core development team before progressing to the test stage. Reviews from other contributors are very welcome. Reviewers may request that changes be made to the pull request. Changes should be added in separate commits, one for each issue pointed out during the review. Some of the added commits may be squashed to remove dev detours from the history (see [[Making a great commit]]). Once a pull request has been approved it can progress to the test stage.

### 3. Test
Most pull requests will require some user testing. Very small or non-functional changes may move to the merge stage directly, at the discretion of the core dev reviewer.

If user testing is required, this should occur in a context that resembles a production environment as closely as possible. Generally this will mean a staging server that is configured similarly to production and that contains production-like data. Once deployed, the changes made by the pull request should be tested by a person who is neither the developer nor the last reviewer. The pull request should contain enough information to determine what to look for and what should be tested.

The person responsible for testing should provide a clear pass/fail with a very brief summary as a comment on the pull request. A link to more detailed testing notes should also be provided. These notes should ideally be created in a public Google Doc or similar shared document in the public domain.

See more detailed testing process [here](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Testing-process).

### 4. Merge & Deploy
After the testing phase, the PR becomes ready to be merged to the master branch by any of the core devs.

Every week we publish a release with the latest additions to the `master` branch.

### 5. Release
Releasing happens every week. The process is documented in its own wiki page. See [[Releasing]].