# Feature Toggles 101
## What are Feature Toggles

Feature Toggles are a kind of configuration used to switch specific features on and off at runtime. By on and off we mean that they control whether or not certain code paths are executed (on) or not (off). This means those code paths can be enabled for certain users and/or environments such as staging, production, or development.

It’s a technique in software development that attempts to provide an alternative to maintaining multiple branches in source code (known as feature branches), such that a software feature can be tested even before it is completed and ready for release.

## Why use Feature Toggles

They adhere to the agile school of thought; to ship code early and often. The problem with this paradigm is that shipping fast can cause bugs in production when a change is introduced. Feature Toggles are an attempt to solve this problem by **decoupling deployment of the code of a change from the actual release/activation of that change.**

This means that changes in PRs can be smaller than what makes for users to see. We can afford a smaller granularity level. We can do more but smaller changes to the codebase without showing them to users. For instance, we don’t need to reimplement customer balances in a single pull request but multiple ones.

Feature flags also enable testing in production. By making features accessible only to selected users features can make it to production in a safe way. They can be safely tested, with no risk of them being exposed to regular users. We can then assess performance and other non-functional requirements that are hard to replicate in staging.

By leveraging feature flags, an organization can make its software development life cycle pipeline faster and more efficient. Iterations become shorter, which avoids conflicts and provides feedback to the dev and product teams. The sooner we get this feedback, the cheaper it is to act upon it. The organization can deliver value to the customer sooner and more often.

## Feature toggles should be short-lived
They add complexity to the codebase and so they need to be removed as soon as they served their purpose.

Likewise, there’s a limit on the complexity we can manage and as such, we can only afford to have a limited number of feature-toggles coexisting in the codebase.

## What Feature Toggles are not
They are not a tool to customize features. Their purpose is to accelerate/improve the development of features that will be available to everyone. 

# Feature Toggles in OFN

What follows are the notes of the retro we did on 2021-03-29 about the use of toggles while implementing [Unit Prices](https://github.com/openfoodfoundation/openfoodnetwork/issues/6449).

## Manual testing

We won't manually test pull requests at the early stage of the development of a new feature. This enables having a very quick feedback loop since the pull-request doesn't need to wait for an available tester reducing the time it takes to merge it.

Product and design teams validate the changes early on giving feedback to devs. Then, the team decides when there's been enough quick feedback loop and manual testing is needed and the PR follows the regular process.

If validation from product is needed, devs need to ping them. If something is not clear, ask product and design as quickly as possible so we don't realize too late that we went down the wrong path. Likewise, it's worth having a demo session before involving manual testing so everyone is on the same page. That's also good for them so they don't feel out of the loop.

## When and when not to use Feature Toggles

The goal is to ship things incrementally and enable a quick feedback loop. We can use other mechanisms such as [Dark Launching](https://martinfowler.com/bliki/DarkLaunching.html) and doesn't need to be always a toggle. Sometimes they may add non-negligible complexity. Whatever mechanism we choose needs to be decided up-front.

We separate the discussion from the topic of A/B testing. It's done with toggles but it serves a totally separate purpose.

## Conventions

We realized it's best if there's a shared understanding across the team about which environments have feature-toggles enabled by default. It's easier to reason about.
 
However, having them on by default on staging messes with manual release testing. We would test things users won't see in production yet. Therefore, it's safer to have toggles off by default in staging. 

To avoid leaving them on by mistake when manually testing a toggle, we'll explore how to automate clearing out enabled toggles possibly at deploy-time.                                                                                                                                                                                         
                        
The question about using feature toggles with guest users remains unanswered. Since there's no user record, there's nothing we can check.