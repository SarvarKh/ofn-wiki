## Branching model
### OFN's 2-0-stable branch

The further we go with the upgrade the more issues we find that can't be solved by being compatible with the `master` branch. We need to continue the development in a different branch so that we can work on both, OFN's development and the Spree upgrade, independently.

We already created said branch as [2-0-stable](https://github.com/openfoodfoundation/openfoodnetwork/tree/2-0-stable) and so all PRs related to the upgrade must be merged into it. Someday, when we're finally done will merge `2-0-stable` into `master`.

This requires frequent merges of `master` into `2-0-stable` so that the latter keeps in sync with the fixes and improvements that are shipped to production. Otherwise, we risk diverging too much from `master`. This would lead us to a troublesome final merge where we would have to deal with all the conflicts and adapt the fixes to Spree 2.0 in one single big step, rather than dealing with these in smaller and iterative steps.

### Spree fork's 2-0-4-stable branch

As briefly mentioned in the *newer patches* section we'll keep using a fork of Spree and apply changes to it in the branch [2-0-4-stable](https://github.com/openfoodfoundation/coopdevs/tree/2-0-4-stable) under the Coopdevs Github organization. Ideally, it should be under OFN's organization instead.

## Task management

Tasks are managed by means of Github issues as we normally do. These are grouped under various ZenHub epics all depending on a main epic called [[Placeholder] Spree Upgrade to version 2.0.0](https://github.com/openfoodfoundation/openfoodnetwork/issues/2109).

Its child epics address each of the topics Spree changes in 2.0.0 and affect the customization OFN made on top. We're currently focusing our efforts on [being able to boot the app](https://github.com/openfoodfoundation/openfoodnetwork/issues/2217) already using Spree v2.0.0. This will then unblock other epics and hopefully the work could be parallelized. Take a look at the attached ZenHub's epics on [[Placeholder] Spree Upgrade to version 2.0.0](https://github.com/openfoodfoundation/openfoodnetwork/issues/2109) to see the full list.

![Spree upgrade epics](https://github.com/coopdevs/openfoodnetwork/blob/1b235a8cf6f619b458c0112dac6156f539b88ff9/doc/img/spree_upgrade_epics.jpg)

In turn, the actual action items will be issues attached to these epics. They will be small and discrete tasks that together will solve the epic at hand.

## Pull requests

The approach we follow to work on these issues mentioned above mimics the branching model we follow for the regular development of OFN.

Once the developer is assigned to the issue from the Github UI a new branch from [2-0-stable](https://github.com/openfoodfoundation/openfoodnetwork/tree/2-0-stable) needs to be created. Then, when opening the pull request notice the base branch has to be `2-0-stable` instead of `master`, as we're used to.

Once approved, the changes will be merged into `2-0-stable`.

![Spree upgrade branches](https://raw.githubusercontent.com/coopdevs/openfoodnetwork/1b235a8cf6f619b458c0112dac6156f539b88ff9/doc/img/spree_upgrade_branches.jpg)
