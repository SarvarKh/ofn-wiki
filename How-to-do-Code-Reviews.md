Things you should verify as you code review a PR:
* Make sure the [defined dev process](The-process-of-review%2C-test%2C-merge-and-deploy) is being followed
* Make sure the [PR is great](Making-a-great-pull-request) and that [all commits in the PR are great](Making-a-great-commit)
* Make sure [I18n](Internationalisation-%28i18n%29) best practices are followed
* Make sure [the build](Continuous-Integration) for the PR is green
* Make sure the changes are tested at appropriate level (see [rspec tips](Testing-and-Rspec-Tips) and [karma tips](Karma) for help)
* Make sure tech [best practices](Code,-the-way-we-do-things) are being followed
* Focus on the implications, design, readability and complexity of the change rather than only its syntax. The latter is what machines are meant to do and that's why we use [Rubocop](http://batsov.com/rubocop/).
* Make sure the boy scout rule is applied: when changing code, it's important to respect the existing structure, but it's even better when we refactor on the way and improve the code we are changing.