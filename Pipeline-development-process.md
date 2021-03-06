What follows is a description of the practices implemented to develop OFN's software. It details processes to be taken care of by the dev team to fuel what we call the Pipeline. This process is deeply connected to the [general contributors onboarding process](https://ofn-user-guide.gitbook.io/ofn-contributor-guide/working-on-the-ofn-governance/team-organization) described in the contributor guide, but more specific to developers.

## Occasional Contributors

Developers that collaborate sporadically without a set schedule and haven't passed the certification process are considered "occasional contributors". They are not expected to care for the Pipeline or the future of the software. We don’t expect them to contribute in tech strategic discussions on the community forum or Slack. We of course highly invite them to “own” the issues/PR they have taken in charge until they are released. This is a fundamental criteria to enter the certification process.

Eventually, once their contributions become regular they are eligible to join the Core Dev team by passing the certification process.

## The Core Dev Team

The Core team is made of the individual developers that have passed the certification process and accepted the Core Dev Pledge.

While occasional contributors just pick Github issues and contribute sporadically, Core team members are committed to develop the software in a regular basis and care for its success.

This team is currently composed by:

* Matt (Ri)
* Maikel (Ri)
* Pau (Ri)

In its turn, the Core team has three levels depending on the experience of the developer.

### Shu

These are junior developers that have limited experience in software projects and need to get used to all its intricacies.

### Ha

These are developers that either have a somewhat limited software development experience or if they do have enough they are rather new to the OFN’s tech stack.

### Ri

These are commonly known as Senior developers everywhere else. They are the ones that help others advance on their career, take full ownership of their responsibilities and are able to lead any kind of initiative.

## Processes

It's the duty of the Core Dev team to ensure issues and pull requests are moved along the pipeline seamlessly so that we keep a steady development pace. That also means watching out for bottlenecks making sure ZenHub columns have enough items in them.

These are the various processes involved.

### Code Review

Pull requests are meant to be reviewed by all members of the Core team on a regular basis.

Firstly, that helps the whole team by making pull requests ready to be tested so that they can eventually be included into a release.

Secondly, it also raises awareness of the changes made to the codebase and the features and bugs being worked on.

Finally, but not less important is that it helps all developers get familiar with the codebase and learn from others. That is specially important for Shu developers who can greatly benefit from it.

**A pull request needs to be approved by at least 2 Ha or Ri Core Dev team members to be moved to "Test Ready"**. Only on specific occasions when the change is minimal and not risky we allow for a single approval. Examples of this are dependabot's upgrades, PRs improving a test, etc. It's the responsibility of the last approver to move the PR to said column unless there are any questions left unanswered or some action pending. At the same time, **Shu Core Dev team members are also expected to review regularly.** It is a condition for them to pass to the "Ha" level.

See the following for more information:

* [Code Review Process](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Code-review-process)
* [How to do Code Reviews](https://github.com/openfoodfoundation/openfoodnetwork/wiki/How-to-do-Code-Reviews)

### Stage

Testers deploy PRs to staging servers themselves from Semaphore CI deployment button.

In any case, **once a PR is successfully staged, the proper `pr-staged-*` label has to be added to it**. The URL of the server is present in the label's description. 

### Merge

**Ri Core Dev team devs are also responsible for merging a PR as soon a tester has shared her testing notes and approved the change.**

### Release

Likewise, is also a **duty of Ha and Ri Core Dev team members to prepare releases**. The responsibility rotates among them so that workload gets distributed and we don't create knowledge silos. The person in charge of the release is called the Release manager.

**The release manager is responsible for all the steps described in [Releasing](releasing), helping with the deploys to the OFN instances, if needed, as well as to deal with any incidents related to it.**

**It's also the release manager's duty to hand over the responsibility to the next ****dev**** in charge.**

Releases have a set schedule and release management is a rotating role. See [Releasing](releasing).

* Maikel
* Matt
* Pau

So, Luis will make sure Maikel takes over as soon as the release is published. Note that not all instances will deploy the release at the same time or some might even wait for the next one. As long as someone has a problem with the release you published, you’re the go-to person.

In the same manner, the release is sanity tested on a scheduled basis by the following testers:

* Lynne
* Filipe
* Rachel

### Meetings

It is a responsibility of all Core team members, including developers, to attend to the Delivery Train catchup, which is held every 4 weeks. This is the way we have to review the process, get the status of the current roadmap priorities, receive updates on how major things are going and everything else related to the pipe.

