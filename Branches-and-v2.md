## master

Since `2-0-stable` (the branch where we dealt with the Spree's v2 upgrade) merge into `master` done in [#3742](https://github.com/openfoodfoundation/openfoodnetwork/pull/3742), `master` runs on Spree v2. All new releases stem from it and since that moment new versions belong to the `v2.X.X` series.

However, because of the [gradual roll-out](https://community.openfoodnetwork.org/t/ofn-v2-rollout-plan/1619/17) of v2, only instances that make it through get to deploy these releases.

## 1-31-stable

The moment we performed the merged mentioned above, we started the branch `1-31-stable` from which we got the release [v1.31.0 Bananas](https://github.com/openfoodfoundation/openfoodnetwork/releases/tag/v1.31.0). The last one before moving instances to v2 and the one that fixed some things to have the servers ready for v2.

It is only meant to be used to fix s1 bugs that v1-instance may experience. So only releases patching those may come out of it but there has been no need so far.
