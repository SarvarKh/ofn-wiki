This is detailed information about how the project is organised and how to contribute to development.

## Development process

### Coding Standards
* Please follow the [Ruby on Rails coding standards](http://guides.rubyonrails.org/contributing_to_ruby_on_rails.html#follow-the-coding-conventions).

* Code tags:  Please put a colon (:) after a code tag.  Use <code>TODO:</code> instead of <code>TODO</code>

***
### Issues
Managed on Trello:
- OFN Core Dev - https://trello.com/b/TXnZrrRL/ofn-core-dev
- OFN Big Picture - https://trello.com/b/cDDdFBV2/ofn-big-picture

Staging server: https://staging2.openfood.com.au

***
The issue queue and wiki pages here on github are the place where questions are raised and discussed, and approaches and solutions are considered and solidified. Discussions about potential solutions, particular approaches, etc. are captured here and are available for others to read  -- perhaps as they join the project and come up to speed, or perhaps to recall why as particular feature was or was not implemented a particular way. Once things are worked out and decisions are made (about the approach to take, specific solution to use, etc.), they should be recorded on the Open Food Network website. Thus the OFN website will document how things are done and what solutions are used on the project, and the background technical discussions will still remain available here on github. 

Initially, issues, problems, and ideas should be handled in the issue queue.  
If the discussion blooms into a larger feature, it may be good to create a wiki page as part of this Developer Guide.
***

### Github
You've obviously already found out we're using Github for version control and collaboration.

Some of the Australian the Dev team are working directly from the OpenFoodNetwork repository.
A useful git tool is git-up (https://github.com/aanand/git-up) which will stash, pull, rebase and unstash in one command.

Most of us are working from individual forks of the OpenFoodNetwork repository. 
Git-up is a useful tool in this case also (https://github.com/aanand/git-up), but git-up automatically pulls from origin, and when working from a fork you need to tell it to also fetch upstream. You can use the command: git config git-up.fetch.all true

***

## [Configuration](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Configuration)
### [Data initialization - bootstrapping, seeding, and importing data](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Data-initialization----bootstrapping,-seeding,-and-importing-data)
