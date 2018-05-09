We use our own fork of Spree where we brought fixes from later versions of Spree and made some of our own.

Currently, the latest commit from upstream that doesn't diverge from Spree's git history is [31e8ca8cb](https://github.com/spree/spree/commit/31e8ca8cb). We then cherrypicked some onto it from other Spree versions and merged some pull requests of our own on top of it.

This leaves our current Spree version somewhere close to that commit, which is 651 commits above [2.0.0.beta](https://github.com/spree/spree/commit/c2345855b) and 762 below [v2.0.0](https://github.com/spree/spree/commit/deed1b65f995c36ea7d565da0257a920a8a1a62b).