## Jul 9, 2018

### Attendees

Maikel (OFN Australia), Matt (OFN UK), Hugo (Open Food France), Luis, Pau (Katuma).

* Discussion about the open issues
* Priorities
* Discuss Maikell's idea ([#2417](https://github.com/openfoodfoundation/openfoodnetwork/pull/2417))
* Doc spikes
* Who's in the team?
* Vacations

### Discussion about the open issues

We have still two themes: data model issues (epics) and fixes that sit across them.

We can use the main epic Zenhub's progress bar as a metric.

* What's the status of [#2225](https://github.com/openfoodfoundation/openfoodnetwork/issues/2225). It's still in dev.
* Decide what we do with [#2286](https://github.com/openfoodfoundation/openfoodnetwork/issues/2286) and [#2206](https://github.com/openfoodfoundation/openfoodnetwork/issues/2206).
* Discuss about [spree/pull/9](https://github.com/openfoodfoundation/spree/pull/9). Either Travis or Semaphore and consider skipping the current failing specs.

What about doing an inception for [#2216](https://github.com/openfoodfoundation/openfoodnetwork/issues/2216) with the insights of [#2218](https://github.com/openfoodfoundation/openfoodnetwork/issues/2218)?

#### Agreements

We continue having these two types of issues described above.

Luis will post the percentage of passing tests of the OFN's test suite once a week in the Slack [#spree-upgrade](https://openfoodnetwork.slack.com/messages/C4NDJT3FY/) channel. This together with the progress bar ZenHub displays in the upgrade's [main epic](https://github.com/openfoodfoundation/openfoodnetwork/issues/2109) should give the visibility the team needs.

In terms of failing tests in both repos we agreed to:

In Spree:

* Use Semaphore with the Travis config that it's already in place.
* We will create an issue for each failing spec we skip. Using RSpec's `xit` method.
* We will work on fixing those while having the test suite passing.

In OFN:

* We add a RSpec tag to all failing specs.
* We run the test suite excluding those with `bundle exec rspec spec --tag ~@failing-due-to-upgrade`.
* All PRs fixing failing specs will have to remove the tag from the test itself.

The benefit: no broken windows. Having the test suite broken we can't know if a particular change broke some more tests or they were just the ones that were already failing. This has leads to dev team not caring about the test suite.

[@mkllnk](https://github.com/mkllnk) agreed on taking https://github.com/openfoodfoundation/spree/pull/8, to move it forward and close it.

Finally, it was agreed to have an inception session for https://github.com/openfoodfoundation/openfoodnetwork/issues/2216 that [@mkllnk](https://github.com/mkllnk) will drive as he's the one that the investigation in https://github.com/openfoodfoundation/openfoodnetwork/issues/2218. This one will be free for every dev to attend.

### Priorities

As we said in the last couple catch-ups, we need a passing test suite and
functional app as soon as possible. This way we can confidently develop
following the same practices we follow on `master`.

We need to put the focus on the upgrade and spend hours on it. That's something
we barely did so far. For this to work we need all core team members to care for
the pipeline management equally so that it doesn't become a bottleneck
preventing us from the former.

This will be documented as "The process" and linked from the core team pledge.

#### Agreements

We'll proceed as follows in terms of priorities:

1. Fix Spree's test suite.
2. Create issues for the failing specs in OFN.
3. Fix those. Go to 2 until done.
4. Document how the team agreed to address each of the new Spree features.
5. Work on the epics documented in the previous step.

Note that although some devs we'll work on 2 and 3 while some others will spend some time on 4 setting the foundations for 5.

### Discuss Maikel's idea

See [[Notes about Maikel's suggested approach]] for some notes about its pros and cons that were used to drive the discussion.

We can plan the ideal approach for decades; we need to move forward and polish
along the way. We still need a 3rd attempt on the extension hooks.

#### Agreements

We keep supporting the old API so that we don't need to change all its callers and this will be done in the `2-0-stable`. There's no need for an adapter because we only keep Spree 2.0 support.

We will do that while taking logic out of the decorators, as the resulting decoupling will make future upgrades easier.

Tomorrow [@luisramos0](https://github.com/luisramos0) and [@sauloperez](https://github.com/sauloperez) will do one hour pair programming session to see if the extension hooks and the approach suggested in [#2417](https://github.com/openfoodfoundation/openfoodnetwork/pull/2417) by [@mkllnk](https://github.com/mkllnk) can live together. If not, [@mkllnk](https://github.com/mkllnk) can continue with that approach of adding logic into the decorator.

### Doc spikes

We need to persist the outcomes of our spikes so that the whole team is aware of
it. Agree on creating wiki pages for each of them and listing them in
https://github.com/openfoodfoundation/openfoodnetwork/wiki/Spree-Upgrade-Themes ?

#### Agreement

The whole team agrees on persisting this insights as new wiki pages in https://github.com/openfoodfoundation/openfoodnetwork/wiki/Spree-Upgrade-Themes.

### Who's in the team?

We need as many resources on the upgrade as we can but there are some features that are still to be finished. So, we need to know how many dev/hours we can spend on the upgrade per week to be able to give any sort of prediction.

#### Agreements

This task and dedication of each dev for the upcoming months:

* Matt, Product Import
* Maikel, last touches on ofn-install (should be ok already), work on the new AUS staging and then Spree upgrade. 3 days a week.
* Hugo, full time on the upgrade. 2 days a week.
* Luis, full time on the upgrade. 2 or 3 days per week.

[@HugsDaniel](https://github.com/HugsDaniel) pointed out that it's hard for him sometimes to decide what to work on next. [@sauloperez](https://github.com/sauloperez) will arrange the ZenHub's Dev Ready column so that devs can pick them up.

### Vacations

Everything needs to be documented and the roadmap be clear enough so that vacations do not affect much its development.

#### Agreements

Pau: on vacations from July 29th until August 29th.
Maikel: 1 week in September.
Hugo: 10 days by the end of September.

[@sauloperez](https://github.com/sauloperez) will focus on dumping all agreements and insights about the upgrade for other devs to work on it until he goes on vacations.
