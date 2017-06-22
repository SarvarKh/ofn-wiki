# Making a great commit
All contributions to the Open Food Network codebase are very gratefully received. You can help us pull your work into the codebase as quickly and easily as possible by making sure that your PRs are full of high quality commits. Below is a guide to what we consider a great commit, and a few examples of commits that we love. :)

## Commit content
#### Each commit should make an incremental and *coherent* change to the codebase
In other words, each commit should ideally be achieving one thing, and one thing only. This allows us to do things like `git bisect` at a later date, and understand exactly where a particular bug was introduced much more easily. It also allows for the use of `git revert` to roll back problematic commits, and the use of `git cherry-pick` to pull changes across to another branch. In general, the fewer the changes per commit, the better. A good metric is: if you cannot describe all of the changes you have made in < 50 characters, that is a good sign that your code should be split across multiple commits.

#### Commits should not make changes that are effectively reverted in a subsequent commit
If you try something and it doesn't work, that is totally fine. Great in fact! Experimenting is how we test ideas and come up with new and interesting solutions. That said, your commit history does not need to reflect your experimentation. Once you have settled on a solution, you should make sure that any moot changes are squashed or edited out of your commit history (using `git rebase -i` or just rewriting your commits). The main reason that this is important is that it is extremely tedious to rebase on top of a series of commits that make changes in one direction, and then make the changes back again. It is important to think about how others will have to interact with each one of your commits, not just the end result.

#### Each commit should contain changes to the functional code AND any corresponding spec changes
Functional changes and specs should not be split across multiple commits. The core OFN team employ TDD, and would love to see the same principles applied by other contributors. In addition for the obvious need for test coverage to prevent breaking changes, specs also provide an additional layer of documentation for your code. Another programmer looking at a functional change you make will appreciate the ability to find the corresponding spec (where you describe the change in plain language) in the same commit.

#### Each commit should pass the test suite (ideally)
The ability to run the test suite at any point is very useful when bisecting and rebasing, but this is an aspirational goal rather than a requirement. Of course we all make commits on the run without running the entire test suite every time, and of course tests break in unforeseen ways. Sometimes it takes more time and energy to fix rectify broken specs in the original context than it is worth, but it is absolutely worth attempting where possible. `git commit --amend` and `git rebase -i` are extremely useful tools, and can be used to fix up any commits you have already made that may have broken the test suite.

## Commit messages should...
* be as short as possible, while providing sufficient detail to understand what is happening
* begin with a capital letter, and should not contain a full stop
* be split across multiple messages where more detailed information is required. eg. `git commit -m 'msg' -m 'details'`
* ideally begin with an imperative verb "Add...", "Update...", "Fix...", "Remove..."
* under no circumstances contain detail only intelligible to the original programmer (eg. `Emergency save`, or `Debug enterprise controller`, or `Tidy up`)

## Examples of great commits
TODO