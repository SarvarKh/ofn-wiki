The Open Food Network has many contributors around the world. Everybody is working on different code and still we want to merge it all together into the master branch at [Github](https://github.com/openfoodfoundation/openfoodnetwork/).

The Australian team started the project and is currently supervising all contributions to the master branch. But we want to enable more people to share this responsibility. A pull request goes through the following stages:

### Create

Anyone can create a pull request. It is helpful if the right people are notified to have a look at it.

### Review

Most pull requests are reviewed by peer developers first. Changes are added in separate commits, one for each issue pointed out during the review. Once the reviewed commits are all good, some of the added commits may be squashed to remove dev detours from the history (see [[Making a great commit]]). That will make it easier for following reviews.
The last review is performed by the person who merges the code, currently Rohan, Rob or Maikel from the Australian team. Let's call them merger.

### Test

The merger may test the code locally. That can be part of the review process. Once the merger is happy, they deploy it on a staging server. We use [Buildkite](https://community.openfoodnetwork.org/t/build-pipelines/540) at the moment. It should then be tested by a person who is not the developer and not the last reviewer. The pull request should contain enough information what to look for and what should be tested.

### Merge

If everybody is happy with the pull request, it can be merged into the master branch. We use our Buildkite pipeline for it. It ensures that all tests pass and it deploys to the Australian production server as well.

### Deploy

The Australian team deploys to the production server straight away. It makes sure that the master branch is production ready. If other teams merge in the future, they should deploy to their production server. If they don't feel confident doing that, the code should not be merged.

### Release

Once or twice a month, we create a release from the current master branch. It lists the important changes since the last release and is a trigger for other groups to consider updating their local instance.