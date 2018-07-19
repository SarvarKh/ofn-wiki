## May 9, 2018

### Attendees

Rob (OFN Australia), Maikel (OFN Australia), Hugo (Open Food France), Pau (Katuma)

### Slides

[OFN's Spree upgrade catch up](https://speakerdeck.com/coopdevs/ofns-spree-upgrade-catch-up)

### Notes & Agreement

There's consensus on this approach. Everyone likes it.

The major challenge we face with it is doing active development on two different repos. This might become a barrier for new contributors as well as tedious for currents ones given that some features may require first a PR in the Spree fork and then one in the app. In any case, having the whole product's code base split into two makes it a bit hard to develop.

The only option we have to overcome this is to document as clearly as possible that:
1. The fork is meant for us to add the extension hooks OFN requires. Full stop.
2. OFN's best practices need to be followed in the Spree fork as well; CI, tests and code review. Code Climate falls beyond this.
3. Everyone watches the Spree fork for PRs

We have the README and CONTRIBUTING files plus the Wiki in both repos (OFN and Spree fork) to do that. The status of the Git book's developer guide initiative is unknown. It might be a good place where to document this in the future.

Besides, due to the fact that we're working on a codebase that is at least 5 years old, certain gem versions Spree depends on will have to be restricted. That's the case we had with `rake` in https://github.com/coopdevs/spree/pull/5. Version 11.0 didn't exist back in 2013 and it happens to fail in Spree. You will find other examples in that same PR.

As discussed in [Task Management](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Development-%E2%9B%91#task-management) new actionable issues will be created along the way that devs can pick up.

In the future, once we get to the current version of either Solidus or Spree and decide to stick to it, we should contribute to it by ditching our fork and implementing these extension hooks there. It is very likely that more people will need these too.

Lastly, we agree on giving this approach a try and:

* Reimplement 3 of the failing customizations this way
* Then, meet again and evaluate. Decide when we draw the line and implement all customizations this way
* Current development in `master` keeps using Spree's approach of `class_eval`s and metaprogramming.
