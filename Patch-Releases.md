Occasionally we need to make a patch release where we add a specific bugfix on top of a tagged release.

This involves checking out the tagged release, cherry-picking some commits on top and making a new release.

You can check out and branch a tagged release like this:

```
git fetch upstream --tags
git checkout tags/v3.6.5 -b 3-6-5-plus-hotfix
```
Then cherry-pick the necessary commit(s), push the new branch to upstream, and draft a new release based on the branch.