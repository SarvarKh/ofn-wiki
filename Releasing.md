# Releasing

## When do releases happen :steam_locomotive: :train: :train:

There's no set schedule for releasing. We just create them on demand when we
decide so in the [#dev](https://openfoodnetwork.slack.com/messages/C2GQ45KNU) Slack channel, but once every 2 weeks is a good guideline.

## How to make a release :spiral_notepad: :white_check_mark: 

* Check for a [Transifex pull request](https://github.com/openfoodfoundation/openfoodnetwork/pulls?utf8=%E2%9C%93&q=is%3Apr+is%3Aopen+head%3Atransifex) and merge it. After merging the pull request, delete the `transifex` branch.
* Download all current translations from Transifex with the [Transifex client](https://github.com/transifex/transifex-client) and add them to the master branch.
  ```sh
  git checkout master
  git pull upstream master
  tx pull --force
  git commit -a -m "Update all locales with the latest Transifex translations"
  git push upstream master
  ```
* Identify all pull requests that got merged since the last release. You can look at the [date of the last release](https://github.com/openfoodfoundation/openfoodnetwork/releases/latest) and use it to [filter pull requests](https://github.com/openfoodfoundation/openfoodnetwork/pulls?utf8=%E2%9C%93&q=is%3Apr+merged%3A%3E2018-05-23T20%3A20%3A00%2B02%3A00+sort%3Aupdated-desc+base%3Amaster) putting the date and time into the filter box like this: `is:pr merged:>2018-07-11T21:37:00+01:00 sort:updated-desc`
* Get the release notes from each of these pull requests. If no release notes are specified you can just copy the pull request title. We only include PRs that have been merged into `master`, so we are not currently adding release notes for Spree Upgrade PRs that are merged into the `2-0-stable` branch.
* Draft a [new release](https://github.com/openfoodfoundation/openfoodnetwork/releases/new) in the Github UI. Make sure the notes can be understood by humans using the types of changes specified by [Keep a changelog](https://keepachangelog.com) and use only the sections that have at least a PR in it. Keep in mind these notes are the source of truth for everyone: devs, product people and users. Mention any dependencies on [ofn-install](https://github.com/openfoodfoundation/ofn-install) as well.
* Unless agreement has been reached in the [#dev](https://openfoodnetwork.slack.com/messages/C2GQ45KNU) Slack channel that a major release is appropriate, releases only bump up the minor version (eg: from 1.16.0 to 1.17.0).
* Publish the release when all is OK; that will create a git tag for you.
* Announce in [#global-community](https://openfoodnetwork.slack.com/archives/C59ADD8F2) you just published a new release. Use the template below :point_down: :

    ```
    Hi all, just letting you know that we just released [version number] :tada:.
    You can read more about it here: https://github.com/openfoodfoundation/openfoodnetwork/releases/tag/[version number]
    ```
* Nudge the next person to release. You can set a reminder in Slack: `/remind @matt-yorkley release in 2 weeks`

## Who has the release power :zap: :muscle: 

All Ha-Ri certified/core devs will now do [releases](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Pipeline-development-process#release) on a rotating basis.
