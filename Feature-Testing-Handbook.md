
For anyone who is interested in doing feature testing on the OFN here are some guidelines you can use to help you plan your approach to testing. Please feel free to add to this.
# 1	Background
Where does testing fit in the overall software development process?
1.	Github ‘issues’ detail the desired improvements we want to make to the software. These are created in two situations a) the global community has decided that there’s a new feature that will be added (e.g. a new report) and b) there’s a bug which has been prioritised to be fixed.
2.	A developer will create a Pull Request. This is the code that makes the changes that solves the issue- either by building a new feature, or by fixing the bug.
3.	This Pull Request code needs to be ‘staged’ and tested. This is the process of putting the code into one of the testing websites (called ‘staging websites) and checking that the code works as intended, before the code gets added to the ‘master’ code.
4.	If the Pull Request is having the desired effect and solving the issue then we can ‘merge’ this Pull Request. If not, it will need to go back to step 2, where the developer improves their pull request before it gets tested again.
5.	All of the merged pull requests are collected together into a ‘release’. This contains many pull requests. Before we launch a release we also test the release itself- to check that all of the PRs are interacting together ok.
6.	The release is eventually adopted by all of the OFN instances.
# 2	Testing
As described above, testing is the process of ensuring that new code is working correctly before it gets added to the master code. If the new code isn't thoroughly tested, it's likely that little or large errors will get into the software and cause malfunctions and problems for users. The testers’ job is to prevent any bugs or errors from getting through.
## 2.1	Get setup to test
Things you’ll need before you can test:
-	A Github account and permission to access the Open Food Foundation repository (http://github.com/openfoodfoundation/openfoodnetwork). 
-	Install the Zendesk extension so you can view the pipe board in Github.
-	Super Admin login details for the Staging Servers that you’ll be testing on.
-	Permission to access the Global Google Drive, specifically the testing notes folder
-	Advanced testers will need access to Semaphore so they can stage their own PRs without the assistance of a developer.
-	You may wish to install the Loom plugin which lets you capture video screenshots. This is helpful in reporting any bugs/issues you see.

**Get familiar with OFN**
For any new tester, a helpful first step to testing is to become familiar with the OFN software. This makes it much easier to spot things which are not quite right when you start testing. A good way to get familiarised is to setup a producer shop and a hub shop, using the instructions in the user guide, and also try using the advanced features. A tester who is familiar with all the functionalities of OFN will find testing much easier.

## 2.2	‘Staging Servers’
When developers finish creating a PR they'll put their new code into a staging server. This is a website that looks just like your local OFN website (what we call the Production Server), but it's full of fake data that you can play around with, without any repercussions. The staging server is where you do testing. To assist in your testing, it's good to have some enterprises setup on the staging server, so you can test the software while doing usual tasks that enterprises commonly do on the site. We call the fake enterprises, products etc that are setup on the Staging server "seeds data". If when testing you feel like there is some basic fake data missing, you can create an issue in Github to ask a developer to add the data you need.

OFN currently has 3 staging servers. You can generally only test one PR at a time on a Staging server, so we have a couple.

## 2.3	Process for staging a PR on a staging server
When a developer asks you to test something, they'll follow the following process:
-	They will stage their new code on a Staging server (which one will depend on which one is available)
-	They’ll move the PR into the Test Ready column in Zenhub
-	They’ll put a comment on the PR saying which staging server the PR is staged on
-	They’ll put a label on the PR showing which server the PR is staged on
-	They’ll send you the link to the PR. 
-	They’ll assign you to the PR (or if the PR was available for anyone to test, when you claim the PR, assign yourself)
## 2.4	What to test

When the developer sends you a PR to test, you should see that the PR contains a description of 'what to test'. In this section developers should have detailed:

* A description of the desired new behaviour (you can also find it in the connected issue which also describes the desired change). This description will usually relate to a certain part of the website, or a certain flow of behaviour.
* A description of what the 'acceptability criteria' are (also in the connected issue)
* A description of any areas of the site that may have been inadvertently impacted by their work, and should therefore also be tested.

It's a tester's job to make sure that anything which should be changed/fixed is, and anything that shouldn't be changed isn't. It's also your task to spot any bugs or glitches that might be there.

## 2.5	Performing the tests 
Once you have this information, you can start experimenting with the software to check that it's behaving as intended and to check that all unrelated features are behaving as normal. Below are some tips to think about as you’re testing

## 2.6	Break your testing down into a checklist of smaller use cases
A good way to be methodical in testing is to break down the overall functionality into a checklist of smaller items to test. Spending the time thinking through all the little test cases gives you more confidence that you’ve checked all the right places and scenarios. There’s 3 ways to break down the testing, by place and by user type and by device.
Note: When it comes to writing up your test notes (covered later in this doc) a checklist of small test items makes it easy to write clear and thorough test notes.

### 2.6.1	Check all the places
New features will often impact on multiple places, such as in the shopfront, admin interface, orders listing, reports and in the order confirmation emails. Think about how the changes to the code could have impacts 'downstream' and make a list of all the places you want to test. The developers should list all the places that could be impacted, but as a tester you might also thing of somewhere else. As you get more familiar with the software it becomes easier to think of the different places that a change could flow on to.

For example:

What to test: The ‘About Us’ text field now includes bold, italics and underline formatting.

Places that might be impacted:
-	Check that in the About Us area of Enterprise Settings you can see the new bold, underline and italics formatting options
-	Check that if I write About Us text using bold/italics/underline that the formatting carries through to a) the profile b) the shop.
-	Check that the formatting options are also available in the /register wizard, for someone who is writing their About Us text there.
-	Check that pre-existing About Us text on profiles and shops looks the same and hasn’t been impacted

### 2.6.2	Think about different user types
Some features only apply to certain users on the OFN, and others will impact all users. Think about whether the feature you're testing should impact on some or all users and test for all users. The user types are below:
•	Producer, selling None, profile only
•	Producer, selling Own
•	Producer, selling Any
•	Hub, selling None, profile only
•	Hub, selling Any
•	Customer
•	Super Admin users

For example:
What to test: Super Admin users can now ‘toggle’ the subscription feature on and off in Enterprise Settings

Users to test:
-	Check that as a Super Admin user you can see the subscription toggle in Enterprise Settings
-	Check that as a regular user with access to Enterprise Settings you cannot see the subscription toggle. Check this for both an Enterprise who has subscriptions toggled on and off.

### 2.6.3	Device discrepencies
If the PR you are testing involves front end User Interface (what end customer sees on their screen) you should test on a mobile device and desktop to check if it looks ok. The code is supposed to be "responsive" so the visual website is supposed to adapt to the screen size of a mobile in a nice way.

For example:
What to test: Category filters in shops should be condensed into a (+ 5 more) button if the shop has more than 3 categories.
-	Test that the filters are displaying in a condensed format on desktop
-	Test that the filters are displaying in a condensed format on a mobile 

## 2.7	Comparing the new code to the old code
If you’re not totally sure how the system behaved before the PR, you can have a look at any of the production sites. If the PR is intended to fix a bug, you should see that the bug is still present on the production sites, but it should be fixed on Staging. If the PR is intended to add a new feature, you should see that on Production the feature isn’t present. By comparing the staging and production sites you can see what effect the PR is having.

## 2.8	Check how new features interact with existing features
Does the new code interact accurately with other features, like private shopfronts, multiple order cycles, inventory, E2Es and customer tags? Sometimes bugs can hide in unusual layers of features/settings, so try to test some complex shop setups.

## 2.9	Is it logical?
If there's something about the new code that you find unpleasant (visually, the wording of text, navigation) or confusing, make a note. The scope of the PR might mean that the improvement can't be incorporated, but it's good to record it anyway.

## 2.10	Not sure how a feature should work?
Not sure how an existing feature is supposed to operate? There should be a description of how a feature is intended to work in the User Guide. If it's something more detailed, and it's not described in the user guide, the best way to get familiar with how OFN works is to play around. Sometimes no one else will know how a feature behaves in a certain scenario, so the only way to find out is to test different scenarios and see how the system behaves. That's the good thing about the staging servers - you can play around without impacting anything.

## 2.11	Switch up Browsers
Sometimes browsers behave differently when reading the same code. As we don't have an extended dev team we can't test on all browsers type and versions, so as a minimum we recommend to regularly change browsers when you do testing. Switch between Chrome or Firefox for instance.

## 2.12	Languages discrepancies
Depending on the staging server the PR were deployed on, the language you test in may vary. If it's not English, and translations are involved in the PR, you might see "translations missing" while testing. This is normal and you should ignore it as we can't translate new strings before the PR is merged. We recommend as much as possible to test in an English environment.

## 2.13	Super Admin settings
Sometimes you'll need to test some Instance settings, which are controlled by Super Admin logins. When you login as a Super Admin you can see all the settings for the instance- things that users can't see. There's a guide which describes the Super Admin Configuration settings here - https://ofn-user-guide.gitbook.io/ofn-super-admin-guide/

## 2.14	I’ve found a bug while testing
Before you report that you’ve found a bug in the PR, ask first whether the bug occurring on production? If it’s not occurring on production, the bug was likely caused by the PR. In this case you should just describe the bug in your testing notes. If the bug is occurring on production it wasn’t caused by this PR. In this case you should check whether the bug is already captured by an issue in github. If it’s not, create a new issue. When you go to create a new issue you’ll see there’s a template to fill in which is quite straightforward.

## 2.15	Need more email addresses?
When testing you'll often find that you need more user accounts than you have email addresses. In this case you can user this little trick. OFN will see these two email addresses as different users, but all emails will go to the same inbox. sally@openfoodnetwork.org.au and sally+testing@openfoodnetwork.org.au . By adding +testing or +demo or +sunshine etc after your normal email you can create more accounts (there's no limit). This lets you create additional user accounts in OFN without needing lots of inboxes.

# 3	Reporting Feedback
Reporting your feedback is a 2 step process:

## 3.1	Step 1) Recording your testing notes in a Google Doc
Testers should record their detailed "Testing notes" in a Google doc in the global OFN drive (it's in edit mode so you should have access) so that we have all testing notes in the same place. Please duplicate the Testing Notes template and rename your new version with the PR number and description.

The testing template is designed to be broken down into all the user cases which were tested, and the tester should describe what they tested and what the result was. The more detail the better.

**If the test passed colour the box green.**
Describe what worked, and perhaps put a screenshot if you think it's a good record.

**If it failed colour the box red.**
Give a description of what didn't work. Include as much detail as you can such as what kind of user was involved and what actions led up to the error occurring. This helps the developer when they try to replicate the problem. Are there some situations where the error occurs, and others where it doesn't? If the errors is hard to replicate include this in the notes.

**If you’re unsure, colour the box orange.**
Describe your uncertainty, and flag this uncertainty in your comment on the github issue (see 3.2 below). An example of an uncertainty is 'I don't know what the desired functionality is', 'did the developer intend for this?'.

Screenshots
A picture says 1000 words, so use screenshots to show features that are working, and more importantly not working. You can capture what's on your screen with the Prt Scr button on your keyboard. If it's possible to show a bug in action, get a screenshot and illustrate it. Even better if you can capture a GIF or moving image showing the bug –Loom is a good plugin for this.

## 3.2	Step 2) Comment on the PR
Once you have tested all the cases you could think of related to the PR and you've created a testing notes doc, report your conclusions by commenting on the PR in Github.

(a) If it's all good, just say something like 'no issue found' and give a high five to the awesome developer who did a good work. You can also list the things that were working in quick dot poin

(b) If you spot anything that's not working as you would expect, or is broken, tell the developers by listing the major outstanding issues that should be addressed in shortform. If the developer needs more detail they will go to the google doc, but they may not need to.

Lastly, include the link to your google doc in the comment, so the developer can see the record of your detailed test notes.

# 4	Launching the next action
•	If (a) it means the piece of code is deemed to be working correctly, and it can be 'pushed to production'. This means it will become part of the live, publicly used software and will be implemented on all instances. To tell the developers that accredited to merge that your tests are good and they can merge, just move the card to the "Ready to go" column or change the pipeline in the PR to "Ready to go".
•	If (b) it means the developer needs to do some more work to fix the remaining issues. Then move the card back to the "In dev" column or change the pipeline in the PR to "In dev". When he/she will have re-worked on it, he will submit modifications that will need again to be reviewed by other developers before they can be "ready to test" again.
•	If (c) you're not sure, ping the developer and ask them to take a look at the testing notes to determine whether it's ready to go or not.