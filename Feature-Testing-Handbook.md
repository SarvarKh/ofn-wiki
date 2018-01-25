For anyone who is interested in doing feature testing on the OFN here are some guidelines you can use to help you plan your approach to testing. Please feel free to add to this.

## Introduction

Testing is a process of ensuring that new code is working correctly before it gets added to the master code. If the new code isn't thoroughly tested, it's likely that little or large errors will get into the software and cause malfunctions and problems for users.

When developers finish developing a feature (or a fix to a bug) they'll put their new code into a staging server. This is a website that looks just like the OFN, but it's full of fake data that you can play around with, without any repercussions. The staging server is where you do testing. To assist in your testing, it's good to have some enterprises setup on staging, so you can test the software while doing usual tasks that enterprises commonly do on the site.

When a developer asks you to test something, they'll give you a link to the Pull Request (PR). This PR should contain a description of 'what to test'. In this section developers should have detailed:

- A description of the desired new behaviour
- A description of what the 'acceptability criteria' are
- A description of any areas of the site that may have been inadvertently impacted by their work, and should therefore also be tested.

Once you have this information, you can start experimenting with the software to check that it's behaving as intended and to check that all unrelated features are behaving as normal. If you spot anything that's not working as you would expect, or is broken, record these testing notes on the relevant github issue. The developer will look at your feedback and resolve any important problems, and possibly give it back to you for more testing.

When a piece of code is deemed to be working correctly, it can be 'pushed to production'. This means it will become part of the live, publicly used software and will be implemented on all instances.

Before you start testing, it's good to become familiar with the OFN software. This makes it much easier to spot things which are not quite right. A good way to get familiarised is to setup a producer shop and a hub shop, using the instructions in the user guide, and also try using the advanced features.

## Testing approach

Testing involved using a new/updated feature as if you are an enterprise, and making sure it's working correctly. So essentially just think of different ways that users might interact with a feature and check that it's performing the way the developer intended.

### Think about different user types

Some features only apply to certain users on the OFN. Think about whether the feature you're testing should impact on some or all users. The user types are below:
* Producer - selling None - profile only
* Producer - selling Own
* Producer - selling Any
* Hub - selling None - profile only
* Hub - selling Any
* Customer

### Comparing the new code to the old.

If you're not sure how the existing code functions, and how the new code is different, you can always play with the feature on production to see its 'pre-developed' state.

### Is it logical?

If there's something about the new code that you find unpleasant (visually, the wording of text, navigation) or confusing, make a note. 

### Check all the places

New features will often impact on multiple places, such as in the shopfront, admin interface, orders listing, reports and in the order confirmation emails. Think about how the changes to the code could have impacts 'downstream' in other areas and test those as well.

### Check how new features interact with existing features

Does the new code interact accurately with other features, like private shopfronts, multiple order cycles, inventory, E2Es and customer tags?

## Recording your testing notes

Testing notes are recorded in google documents. Use this as a template - https://docs.google.com/document/d/16UZXJdemEI3EmcpFzJeuLchzkWKaaLoiJThr4cROig0/edit?usp=sharing

Your notes should include:
Tester: Put your name and github handle.
Environment: e.g Staging1
Browser: e.g Chrome, PC
Devise: e.g. PC

Use cases
Give a description of what scenario you tested the code in. For each use case, indicate whether it passed (everything was good), or if it failed (did not meet the spec).

Give a description of what didn't work. Include as much detail as you can such as what kind of user was involved and what actions led up to the error occurring. This helps the developer when they try to replicate the problem. Are there some situations where the error occurs, and others where it doesn't? If the errors is hard to replicate include this in the notes.

Screenshots- A picture says 1000 words. You can capture what's on your screen with the Prt Scr button on your keyboard. If it's possible to show a bug in action, get a screenshot and illustrate it.