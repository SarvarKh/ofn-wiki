For anyone who is interested in doing feature testing on the OFN here are some guidelines you can use to help you plan your approach to testing. Please feel free to add to this.

## Introduction

Testing is a process of ensuring that new code is working correctly before it gets added to the master code. If the new code isn't thoroughly tested, it's likely that little or large errors will get into the software and cause malfunctions and problems for users.

When developers finish developing a feature (or a fix to a bug) they'll put their new code into a staging server. This is a website that looks just like the OFN, but it's full of fake data that you can play around with, without any repercussions. The staging server is where you do testing. To assist in your testing, it's good to have some enterprises setup on staging, so you can test the software while doing usual tasks that enterprises commonly do on the site.

When a developer asks you to test something, they'll give you a description of what they've done, usually on a github issue. Make sure they give you a detailed description of the following:

* What is the new feature or bug fix?
* How does the developer expect that it will behave?
* Could any elements of the site have been inadvertently impacted by their work, that they know of?

Once you have this information, you can start playing with the software to check that it's behaving as expected and to check that all unrelated features are behaving as normal. If you spot anything that's not working as you would expect, or is broken, record these testing notes on the relevant github issue. The developer will look at your feedback and resolve any important problems, and give it back to you for more testing.

When a piece of code is deemed to be working correctly, it can be 'pushed to production'. This means it will become part of the main software and will be implemented on all instances.

Before you start testing, it's good to become familiar with the OFN software. This makes it much easier to spot things which are not quite right. A good way to get familiarised is to setup a producer shop and a hub shop, using the instructions in the user guide, and also try using the advanced features.

## Testing approach

Testing involved using a new/updated feature as if you are an enterprise, and making sure it's working correctly. So essentially just think of different ways that users might use a feature and check that it's doing what you would expect.

### Think about different user types

Some features only apply to certain users on the OFN. Think about whether the feature you're testing should impact on some or all users. The user types are below:
* Producer - selling None - profile only
* Producer - selling Own
* Producer - selling Any
* Hub - selling None - profile only
* Hub - selling Any
* Customer

### Comparing the new code to the old.

If you're not sure how the existing code functions, and how the new code is different, you can always play with the feature on production to see it's 'pre-developed' state.

### Is it logical?

Is there any way the new feature could be made more logical or easier to use? Ideally features should be self explanatory and intuitive. If you're finding new code confusing, users might as well.

### Check all the places

New features will often impact on multiple places, such as in the shopfront, admin interface, orders listing, reports and in the order confirmation emails.

### Check how new features interact with existing features

Does the new code interact accurately with other features, like private shopfronts, multiple order cycles, inventory, E2Es and customer tags?

## Recording your testing notes

Testing notes are written on the github issue related to the new code. Developers will read your testing notes and do further development if needed, or they'll push the code to production.

Things to record in testing notes:

* Record a brief list of functions that were behaving correctly, like a checklist.
* Record detailed information about anything that was not working correctly
* Record details of any features that don't need urgent attention, but could be improved, or would be 'nice to have'. You can even create a new issue for these thing.

## Format of detailed testing notes

**Background**

What devise are you on?
Which browser?

**Expected behaviour**

Write a description of how you expected the feature/fix to behave. Sometimes the developer will have a description of how the feature should function. Other times this is based on your intuition of how you'd expect the feature to behave, which also represents what an average user might expect.

**Actual behaviour**

Describe what actually occurred. How was the feature's actual behaviour different from what you expected and why is this a problem?
Include as much detail as you can such as what kind of user was involved and what actions led up to the error occurring. Are there some situations where the error occurs, and others where it doesn't? If the errors is hard to replicate include this in the notes.

**Screenshots**

A picture says 1000 words. You can capture what's on your screen with the Prt Scr button on your keyboard. If it's possible to show a bug in action, get a screenshot and illustrate it.

See example below:

[https://community.openfoodnetwork.org/uploads/default/original/1X/a95dd4e0e2851d8a720d564fa582e9d35451f6ab.jpg]

If you have any questions let @sstead know.
