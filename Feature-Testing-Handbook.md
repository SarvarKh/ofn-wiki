For anyone who is interested in doing feature testing on the OFN here are some guidelines you can use to help you plan your approach to testing. Please feel free to add to this.

## Background
Where does testing fit in the overall software development process?

1. Github ‘issues’ detail the desired improvements we want to make to the software. These are created in two situations a) the global community has decided that there’s a new feature that will be added (e.g. a new report) and b) there’s a bug which has been prioritised to be fixed.
2. A developer will create a Pull Request. This is the code that makes the changes that solves the issue- either by building a new feature, or by fixing the bug.
3. This Pull Request code needs to be ‘staged’ and tested. This is the process of putting the code into one of the testing websites (called ‘staging websites) and checking that the code works as intended, before the code gets added to the ‘master’ code.
4. If the Pull Request is having the desired effect and solving the issue then we can ‘merge’ this Pull Request. If not, it will need to go back to step 2, where the developer improves their pull request before it gets tested again.
5. All of the merged pull requests are collected together into a ‘release’. This contains many pull requests. Before we launch a release we also test the release itself- to check that all of the PRs are interacting together ok.
6. The release is eventually adopted by all of the OFN instances.

## Testing
Testing is a process of ensuring that new code is working correctly before it gets added to the master code. If the new code isn't thoroughly tested, it's likely that little or large errors will get into the software and cause malfunctions and problems for users. The testers job is to prevent any bugs or errors from getting through.

When developers finish developing a feature (or a fix to a bug) they'll put their new code into a staging server. This is a website that looks just like the OFN, but it's full of fake data that you can play around with, without any repercussions. The staging server is where you do testing. To assist in your testing, it's good to have some enterprises setup on the staging server, so you can test the software while doing usual tasks that enterprises commonly do on the site. We call those data "seeds data". If when testing you feel like there are some basic fake data missing, you can create an issue in Github to ask a developer to add them.

### Get setup to test
You'll need to have login details for the staging server that you'll be testing on. It's good to have both a Super Admin login and regular user logins. 

### Stage a PR on a staging server
When a developer asks you to test something, they'll give you a link to the Pull Request (PR) and "assign" yourself to that PR. You can also yourself check the "Testing" column in the [Zenhub delivery pipeline board](https://github.com/openfoodfoundation/openfoodnetwork/#boards?repos=6257856) and assign yourself to the next PR available. You will find in the PR's last comments the link to the staging server where the PR have been deployed (for example http://staging.openfoodnetwork.org.au)

### What to test
This PR should contain a description of 'what to test'. In this section developers should have detailed:
- A description of the desired new behaviour (you can also find it in the connected issue)
- A description of what the 'acceptability criteria' are (also in the connected issue)
- A description of any areas of the site that may have been inadvertently impacted by their work, and should therefore also be tested.

It's a tester's job to make sure that anything which should be changed/fixes is, and anything that shouldn't be changed isn't. It's also your task to spot any bugs or glitches that might be there.

### Performing the tests
Once you have this information, you can start experimenting with the software to check that it's behaving as intended and to check that all unrelated features are behaving as normal. 

Before you start testing, it's good to become familiar with the OFN software. This makes it much easier to spot things which are not quite right. A good way to get familiarised is to setup a producer shop and a hub shop, using the instructions in the user guide, and also try using the advanced features.

### Test cases
It can be helpful to test the feature in cases - each case is a scenario where a certain type of user, with a certain type of account is trying to achieve something. You can test each scenario and comment on whether everything functioned correctly for that use case.

### Think about different user types
Some features only apply to certain users on the OFN. Think about whether the feature you're testing should impact on some or all users. The user types are below:
* Producer - selling None - profile only
* Producer - selling Own
* Producer - selling Any
* Hub - selling None - profile only
* Hub - selling Any
* Customer

### Comparing the new code to the old.
If you’re not totally sure how the system behaved before the PR, you can have a look at any of the production sites. If the PR fixes a bug, the bug will still be occuring on production, but not on staging. By comparing the staging and production sites you can see what effect the PR is having.

### Not sure how a feature should work?
Not sure how an existing feature is supposed to operate? There should be a description of how a feature is intended to work in the [User Guide](https://guide.openfoodnetwork.org/). If it's something more detailed, and it's not described in the user guide, the best way to get familiar with how OFN works is to play around. Sometimes no one else will know how a feature behaves in a certain scenario, so the only way to find out is to test different scenarios and see how the system behaves. That's the good thing about the staging servers - you can play around without impacting anything.

### Is it logical?
If there's something about the new code that you find unpleasant (visually, the wording of text, navigation) or confusing, make a note. The scope of the PR might mean that the improvement can't be incorporated, but it's good to record it anyway.

### Check all the places
New features will often impact on multiple places, such as in the shopfront, admin interface, orders listing, reports and in the order confirmation emails. Think about how the changes to the code could have impacts 'downstream' in other areas and test those as well. The developers should list all the places that could be impacted, but as a tester you might also thing of somewhere else.

### Check how new features interact with existing features
Does the new code interact accurately with other features, like private shopfronts, multiple order cycles, inventory, E2Es and customer tags? Sometimes bugs can hide in unusual layers of features/settings, so try to test some complex shop setups.

### Think about browsers and devices discrepancies
- Sometimes browsers behave differently when reading the same code. As we don't have an extended dev team we can't test on all browsers type and versions, so as a minimum we recommend to regularly change browsers when you do testing. Switch between Chrome or Firefox for instance. 
- Also if the PR you are testing involves front end User Interface (what end customer sees on their screen) you should test on a mobile device to check if it looks ok. The code is supposed to be "responsive" so the visual website is supposed to adapt to the screen size of a mobile in a nice way.

### Languages discrepancies
Depending on the staging server the PR were deployed on, the language you test in may vary. If it's not English, and translations are involved in the PR, you might see "translations missing" while testing. This is normal and you should ignore it as we can't translate new strings before the PR is merged. We recommend as much as possible to test in an English environment.

#### Super Admin settings
Sometimes you'll need to test some Instance settings, which are controlled by Super Admin logins. When you login as a Super Admin you can see all the settings for the instance- things that users can't see. There's a guide which describes the Super Admin Configuration settings here - https://ofn-user-guide.gitbook.io/ofn-super-admin-guide/

#### I’ve found a bug while testing
Is this bug occurring on production? If it’s not occurring on production, the bug was likely caused by the PR. In this case you should just describe the bug in your testing notes. If the bug is occurring on production it wasn’t caused by this PR. In this case you should check whether the bug is already captured by an issue in github. If it’s not, create a new issue. When you go to create a new issue you’ll see there’s a template to fill in- pretty straightfoward.

#### Need more email addresses?
When testing you'll often find that you need more user accounts than you have email addresses. In this case you can user this little trick. OFN will see these two email addresses as different users, but all emails will go to the same inbox. sally@openfoodnetwork.org.au and sally+testing@openfoodnetwork.org.au . By adding +testing or +demo or +sunshine etc after your normal email you can create more accounts (there's no limit). This lets you create additional user accounts in OFN without needing lots of inboxes.

## Reporting feedback
Reporting your feedback is a 2 step process:

### 1) Recording your testing notes

"Testing notes" are recorded in a Google Doc. We recommend you to put it in the [global OFN drive](https://drive.google.com/drive/folders/0B4hQwCDu1jmFR2NZNkF1cFJDbXM?usp=sharing) (it's in edit mode so you should have access) so that we have all testing notes in the same place. Please duplicate the [Testing Notes template](https://drive.google.com/open?id=16UZXJdemEI3EmcpFzJeuLchzkWKaaLoiJThr4cROig0) and rename your new version with the PR number.

#### Use cases
Give a description of what scenario you tested the code in. For each use case, indicate whether it passed (everything was good), or if it failed (did not meet the spec).

Give a description of what didn't work. Include as much detail as you can such as what kind of user was involved and what actions led up to the error occurring. This helps the developer when they try to replicate the problem. Are there some situations where the error occurs, and others where it doesn't? If the errors is hard to replicate include this in the notes.

#### Screenshots
A picture says 1000 words. You can capture what's on your screen with the Prt Scr button on your keyboard. If it's possible to show a bug in action, get a screenshot and illustrate it. Even better if you can capture a GIF or moving image showing the bug.

### 1) Comment on the PR

Once you have tested all the cases you could think of related to the PR and you've created a testing notes doc, report your conclusions by commenting on the PR:
- (a) If it's all good, just say something like 'no issue found' and give a high five to the awesome developer who did a good work. You can also list the things that were working in quick dot points
- (b) If you spot anything that's not working as you would expect, or is broken, tell the developers by listing the major outstanding issues that should be addressed. If the developer needs more detail they will go to the google doc, but they may not need to.

Then include the link to you google doc in the comment, so the developer can see the record of your detailed test notes.

## 4- Launching the next action
- If (a) it means the piece of code is deemed to be working correctly, and it can be 'pushed to production'. This means it will become part of the live, publicly used software and will be implemented on all instances. To tell the developers that accredited to merge that your tests are good and they can merge, just move the card to the "Ready to go" column or change the pipeline in the PR to "Ready to go".
- If (b) it means the developer needs to do some more work to fix the remaining issues. Then move the card back to the "In dev" column or change the pipeline in the PR to "In dev". When he/she will have re-worked on it, he will submit modifications that will need again to be reviewed by other developers before they can be "ready to test" again.
- If (c) you're not sure, ping the developer and ask them to take a look at the testing notes to determine whether it's ready to go or not.