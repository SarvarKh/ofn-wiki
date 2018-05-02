Theses are the notes about the approach and status followed to upgrade OFN to Spree 2.0.0. The first stable release of the 2.0 branch.

Expect them to be updated very frequently as we move along on the process.

## OFN's Spree fork🍴 

We use our own fork of Spree where we brought fixes from later versions of Spree and made some of our own.

Currently, the latest commit from upstream that doesn't diverge from Spree's git history is [31e8ca8cb](https://github.com/spree/spree/commit/31e8ca8cb). We then cherrypicked some onto it from other Spree versions and merged some pull requests of our own on top of it.

This leaves our current Spree version somewhere close to that commit, which is 651 commits above [2.0.0.beta](https://github.com/spree/spree/commit/c2345855b) and 762 below [v2.0.0](https://github.com/spree/spree/commit/deed1b65f995c36ea7d565da0257a920a8a1a62b).

### Newer patches

The balance between aiming for a small upgrade and the need for a fix made us keep our fork of Spree. You can read more about it in https://github.com/openfoodfoundation/openfoodnetwork/issues/2236#issuecomment-384332024.

So, we keep the upgrade as small as possible to Spree 2.0.4 (read the reason below) but including a fix made in 2.0.8 without which the app does not boot. These patches will be carefully documented in [our fork's README](https://github.com/coopdevs/spree/blob/2-0-4-stable/README.md).

## Getting to version 2.0.4 ⛰ 

v2.0.13, the latest 2.0 series release, lays more or less 1656 commits ahead of our Spree fork and 894 ahead of v2.0.0. Given the already big jump to v2.0.0 and without knowing how deep the changes these 894 commits introduce, it's safer to stick to 2.0.0 and get to 2.0.13 on an upcoming iteration.

v2.0.0 however, as reported in [Spree fail to install](https://github.com/spree/spree/issues/3203), Spree's engines specify conflicting versions of `jquery-rails` making Bundler fail on trying to a find a version that fits both. As a result, the lowest working version we can point to is v2.0.4, where this was [fixed](https://github.com/cloudbring/spree/pull/1). That moves us 66 commits ahead of v2.0.0.

At the time the 2.0 series releases were published they didn't seem to use either Github releases consistently nor pull requests. That's why you will find the version 2.0.13 in Rubygems but not on Spree repo's releases section. Apparently, [801f3d423](https://github.com/spree/spree/commit/801f3d423) is the commit that version points to.

## Methodology ⛑ 

### 2-0-stable branch

The further we go with the upgrade the more issues we find that can't be solved by being compatible with the `master` branch. We need to continue the development in a different branch so that we can work on both, OFN's development and the Spree upgrade, independently.

We already created said branch as [2-0-stable](https://github.com/openfoodfoundation/openfoodnetwork/tree/2-0-stable) and so all PRs related to the upgrade must be merged into it. Someday, when we're finally done will merge `2-0-stable` into `master`.

This requires frequent merges of `master` into `2-0-stable` so that the latter keeps in sync with the fixes and improvements that are shipped to production. Otherwise, we risk diverging too much from `master`. This would lead us to a troublesome final merge where we would have to deal with all the conflicts and adapt the fixes to Spree 2.0 in one single big step, rather than dealing with these in smaller and iterative steps.

### Task management

Tasks are managed by means of Github issues as we normally do. These are grouped under various ZenHub epics all depending on a main epic called [[Placeholder] Spree Upgrade to version 2.0.0](https://github.com/openfoodfoundation/openfoodnetwork/issues/2109).

Its child epics address each of the topics Spree changes in 2.0.0 and affect the customization OFN made on top. We're currently focusing our efforts on [being able to boot the app](https://github.com/openfoodfoundation/openfoodnetwork/issues/2217) already using Spree v2.0.0. This will then unblock other epics and hopefully the work could be parallelized. Take a look at the attached ZenHub's epics on [[Placeholder] Spree Upgrade to version 2.0.0](https://github.com/openfoodfoundation/openfoodnetwork/issues/2109) to see the full list.

![Spree upgrade epics](https://github.com/coopdevs/openfoodnetwork/blob/1b235a8cf6f619b458c0112dac6156f539b88ff9/doc/img/spree_upgrade_epics.jpg)

In turn, the actual action items will be issues attached to these epics. They will be small and discrete tasks that together will solve the epic at hand.

### Pull requests

The approach we follow to work on these issues mentioned above mimics the branching model we follow for the regular development of OFN.

Once the developer is assigned to the issue from the Github UI a new branch from [2-0-stable](https://github.com/openfoodfoundation/openfoodnetwork/tree/2-0-stable) needs to be created. Then, when opening the pull request notice the base branch has to be `2-0-stable` instead of `master`, as we're used to.

Once approved, the changes will be merged into `2-0-stable`.

![Spree upgrade branches](https://raw.githubusercontent.com/coopdevs/openfoodnetwork/1b235a8cf6f619b458c0112dac6156f539b88ff9/doc/img/spree_upgrade_branches.jpg)

## How to extend this doc

Updates to this documentation are encouraged but make sure everyone on the team agrees to them. Ask the team on the [#spree-upgrade](https://openfoodnetwork.slack.com/messages/C4NDJT3FY/) Slack channel their take on the topic. Then, update it to reflect the shared consensus.

## Questions?

Got questions? do not hesitate to raise a hand on the [#spree-upgrade](https://openfoodnetwork.slack.com/messages/C4NDJT3FY/) Slack channel. While this documentation is kept up to date it may not reflect all the details we gathered so far or any other considerations about the upgrade.