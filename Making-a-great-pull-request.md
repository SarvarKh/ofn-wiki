Open Source projects like the Open Food Network depend on contributions from a community of developers. We encourage everyone and everyone to contribute to the ongoing development of the project. One great way to do this is through submission of a Pull Request (PR) that makes improvements to the codebase.

This guide describes how to make sure your PR is GREAT, if you just starting out and are looking for instructions to help you make your first PR, have a read of our [CONTRIBUTING.md](https://github.com/openfoodfoundation/openfoodnetwork/blob/master/CONTRIBUTING.md).

## Making a great PR 

### Use your own fork

Create a fork when starting your work, even if you have write permissions to the central repository. Keep all your development branches there. When your code is ready, create a pull request against the central repository.

Most pull requests are solving one issue. It's useful to include the issue number in the pull request's name, for example `3727-first-credit-card-default`.

### Provide a explanation of the problem you are solving
When you create a PR, you have the opportunity to explain the changes you have made to the codebase. We provide a template in the PR description box to prompt you to include all of the most important information, but the more detail you are able to provide here the better. Mention the other issues in the repo that your PR is related to, and list any pertinent threads in our [community forum (AKA Discourse)](https://community.openfoodnetwork.org). Someone needs to review your contribution before is merged into the code base. Having lots of information about what they are looking at makes the job of a reviewer much easier, and means that feedback is likely to be faster. That means we can get your change merged as quickly as possible. :)

### Keep your commit history clean, tidy and up-to-date
Your commit history should contain only commits that are relevant to the change you are making. If you find that the list of commits for your PR contains commits that you did not make, then it is possible that you have accidentally merged something into you branch that you shouldn't have. Tools like `git cherry-pick` and `git rebase -i` are very handy for cleaning up your commit history if you find yourself in a jam.

We have a guide to [making a great commit](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Making-a-great-commit) which describes the characteristics of a good individual commit. We love to see PRs constructed entirely out of great commits!


### Prefer `git rebase` to `git merge` for keeping up to date
Please avoid unnecessary merge commits wherever possible. If you are the only one working on your PR, and you need to update your PR with upstream changes from the `master` branch, please use the `git rebase` tool, rather than `git merge`. This helps keep your commit history clean and readable, and therefore easier to understand. For example, on your local machine:

````bash
git fetch origin # updates origin/master
git checkout your-pr-branch
git rebase origin/master # rebases your changes on top of origin/master
git push your-fork your-pr-branch --force-with-lease # updates your PR, overwriting your previous changes
````

### Unit test everything
If your code makes functional changes to the way the codebase runs please make sure that these are covered by appropriate specs. In general, we prefer if unit specs appear in the same commit as the functional changes they cover, and this make it easier to understand the intent of the change when looking at the commit at some later point in time. If your change makes only aesthetic changes, then is it often the case that a line or two to test for such changes can be added to an existing feature spec.

### Review and respond to automated tests
We use Travis-CI to run the automated tests for the codebase against each PR that we receive. You don't need to do anything to make this happen, it's auto-magic! It is important that you check back 10 minutes or so after you have created or updated your PR to make sure that the automated tests have passed. Reviewers will tend to assume that a PR with failing automated tests is a work in progress, and will probably pay it less attention as a result.

We also run automated checks of code quality using CodeClimate, we currently use a Rubocop engine to make sure any new changes adhere to the [ruby style guide](https://github.com/bbatsov/ruby-style-guide), and we will probably add other engines in the future. These checks run automatically too, and need to be resolved before a PR will be looked at in detail.

### Be prepared to accept constructive criticism
It is possible that your PR will not be accepted on the first attempt. People think in different ways and a reviewer will often pick up on something that you might have missed. Don't take this the wrong way, any suggestions made or questions asked are done so in the interests of maintaining the quality and usability of the codebase. We understand that making changes (especially repeatedly) can be frustrating at times, and you should feel comfortable defending any decisions you have made, but please keep any discussions civil and respectful.

