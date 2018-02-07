For anyone who is interested in doing feature testing on the OFN here are some guidelines you can use to help you plan your approach to testing. Please feel free to add to this.

## 1- Introduction

Testing is a process of ensuring that new code is working correctly before it gets added to the master code. If the new code isn't thoroughly tested, it's likely that little or large errors will get into the software and cause malfunctions and problems for users.

When developers finish developing a feature (or a fix to a bug) they'll put their new code into a staging server. This is a website that looks just like the OFN, but it's full of fake data that you can play around with, without any repercussions. The staging server is where you do testing. To assist in your testing, it's good to have some enterprises setup on staging, so you can test the software while doing usual tasks that enterprises commonly do on the site. We call those data "seeds data". If when testing you feel like there are some basic fake data missing, you can create an issue in Github to ask to add them.

When a developer asks you to test something, they'll give you a link to the Pull Request (PR) and "assign" yourself to that PR. You can also yourself check the "Testing" column in the [Zenhub delivery pipeline board](https://github.com/openfoodfoundation/openfoodnetwork/#boards?repos=6257856) and assign yourself to the next PR available. You will find in the PR last comments the link to the staging server where the PR have been deployed.

This PR should contain a description of 'what to test'. In this section developers should have detailed:
- A description of the desired new behaviour (you can also find it in the connected issue)
- A description of what the 'acceptability criteria' are (also in the connected issue)
- A description of any areas of the site that may have been inadvertently impacted by their work, and should therefore also be tested.

## 2- Performing the tests
Once you have this information, you can start experimenting with the software to check that it's behaving as intended and to check that all unrelated features are behaving as normal. 

Before you start testing, it's good to become familiar with the OFN software. This makes it much easier to spot things which are not quite right. A good way to get familiarised is to setup a producer shop and a hub shop, using the instructions in the user guide, and also try using the advanced features.

### 2.1- Recording your testing notes

"Testing notes" are recorded in a Google Doc. We recommend you to put it in the [global OFN drive](https://drive.google.com/drive/folders/0B4hQwCDu1jmFR2NZNkF1cFJDbXM?usp=sharing) (it's in edit mode so you should have access) so that we have all testing notes in the same place. Please duplicate the [Testing Notes template](https://drive.google.com/open?id=16UZXJdemEI3EmcpFzJeuLchzkWKaaLoiJThr4cROig0) and rename your new version with the PR number.

#### Use cases
Give a description of what scenario you tested the code in. For each use case, indicate whether it passed (everything was good), or if it failed (did not meet the spec).

Give a description of what didn't work. Include as much detail as you can such as what kind of user was involved and what actions led up to the error occurring. This helps the developer when they try to replicate the problem. Are there some situations where the error occurs, and others where it doesn't? If the errors is hard to replicate include this in the notes.

#### Screenshots
A picture says 1000 words. You can capture what's on your screen with the Prt Scr button on your keyboard. If it's possible to show a bug in action, get a screenshot and illustrate it. Even better if you can capture a GIF or moving image showing the bug.

### 2.2- Testing approach

Testing involve using a new/updated feature as if you are an enterprise, and making sure it's working correctly. So essentially just think of different ways that users might interact with a feature and check that it's performing the way the developer intended.

#### Think about different user types
Some features only apply to certain users on the OFN. Think about whether the feature you're testing should impact on some or all users. The user types are below:
* Producer - selling None - profile only
* Producer - selling Own
* Producer - selling Any
* Hub - selling None - profile only
* Hub - selling Any
* Customer

#### Comparing the new code to the old.
If you're not sure how the existing code functions, and how the new code is different, you can always play with the feature on production to see its 'pre-developed' state.

#### Is it logical?
If there's something about the new code that you find unpleasant (visually, the wording of text, navigation) or confusing, make a note. 

#### Check all the places
New features will often impact on multiple places, such as in the shopfront, admin interface, orders listing, reports and in the order confirmation emails. Think about how the changes to the code could have impacts 'downstream' in other areas and test those as well.

#### Check how new features interact with existing features
Does the new code interact accurately with other features, like private shopfronts, multiple order cycles, inventory, E2Es and customer tags?

## 3- Reporting feedback
Once you have tested all the cases you could think of related to the PR, report your conclusions by commenting on the PR:
(a)- If it's all good, just say something like 'no issue found' and give a high five to the awesome developer who did a good work (+link your google doc url).
(b)- If you spot anything that's not working as you would expect, or is broken, tell the developers by listing the major outstanding issues that should be addressed and link your google doc url. If the developer needs more detail they will go to the google doc, but they may not need to.

## 4- Launching the next action
- If (a) it means the piece of code is deemed to be working correctly, and it can be 'pushed to production'. This means it will become part of the live, publicly used software and will be implemented on all instances. To tell the developers that accredited to merge that your tests are good and they can merge, just move the card to the "Ready to go" column or change the pipeline in the PR to "Ready to go".
- If (b) it means the developer needs to do some more work to fix the remaining issues. Then move the card back to the "In dev" column or change the pipeline in the PR to "In dev". When he/she will have re-worked on it, he will submit modifications that will need again to be reviewed by other developers before they can be "ready to test" again.