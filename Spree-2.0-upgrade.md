# Spree 2.0 upgrade

This are the notes about the approach and status followed to upgrade OFN to Spree 2.0.13. The latest published release of the 2.0 branch.

## OFN's Spree fork

We use our own fork of Spree where we brought fixes from later versions of Spree and made some of our own.

Currently, the latest commit from upstream that doesn't diverge from Spree's git history is `31e8ca8cb`. We then cherrypicked some onto it from other Spree versions and merged some pull requests of our own on top of it.

This leaves our current Spree version somewhere close to that commit, which is 651 commits above [2.0.0.beta](https://github.com/spree/spree/commit/c2345855b) and 762 below v2.0.0.

## Getting to version 2.0.0

v2.0.13, the latest 2.0 series release, lays more or less 1656 commits ahead of our Spree fork and 894 ahead of v2.0.0. Given the already big jump to v2.0.0 and without knowing how deep the changes these 894 commits introduce, it's safer to stick to 2.0.0 and get to 2.0.13 on an upcoming iteration.

At the time the 2.0 series releases were made they didn't seem to use neither Github releases consistently nor pull requests. As a result, the version 2.0.13 is present in Rubygems but not on Spree repo's releases section. Apparently, `801f3d423` is the commit that version points to.