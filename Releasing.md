# Releasing

## Table of contents

* [When do releases happen](#when-do-releases-happen)
* [How to make a release](#how-to-make-a-release)
  * [Notify translators](#notify-translators)
  * [Pull translations in](#pull-translations-in)
  * [Collect included changes](#collect-included-changes)
  * [Draft the release](#draft-the-release)
* [Testing the release](#testing-the-release)
* [Publish the release](#publish-the-release)
* [Who has the release power](#who-has-the-release-power)

## When do releases happen :steam_locomotive: :train: :train:

Releases follow a set weekly schedule. They get drafted by the release manager in charge every Thursday, then it will be ready for testers to do their magic and give their approval until Monday when the release manager will publish it. Then, we deploy on Tuesday.

See the reasoning behind this process in [Release testing next steps](https://community.openfoodnetwork.org/t/release-testing-next-steps/1741/10) and the conversations raised by [Maikel](https://openfoodnetwork.slack.com/archives/C02TZ6X00/p1571294180029100) and [Luis](https://openfoodnetwork.slack.com/archives/CAVTM01QB/p1571145642017500).

I feel obliged to share the gif [@lin-d-hop](https://github.com/lin-d-hop) and [@RachL](https://github.com/RachL) found. Remember, Tuesday is the day our users we'll receive our :heart:

![](https://media.giphy.com/media/3o7qE52FdzR7awdCo0/giphy.gif)

## How to make a release :spiral_notepad: :white_check_mark: 

### Notify translators

It's useful for translators to know when a release will happen so they can translate the latest entries for the new release. So, please **notify translators in [#translations](https://openfoodnetwork.slack.com/messages/CFU9N7TGB/) that a new release will be prepared**. If you can, do this a couple of days before preparing the release so that when you prepare the release the translations are all ready to go. Below you will find how to set a slack auto reminder to do this automatically for next time.

### Pull translations in
Check for a [Transifex pull request](https://github.com/openfoodfoundation/openfoodnetwork/pulls?utf8=%E2%9C%93&q=is%3Apr+is%3Aopen+head%3Atransifex) and merge it. After merging the pull request, delete the `transifex` branch.

Download all current translations from Transifex with the [Transifex client](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Internationalisation-(i18n)#transifex-client) and add them to the master branch.
  ```sh
  git checkout master
  git pull upstream master
  tx pull --force
  git commit -a -m "Update all locales with the latest Transifex translations"
  git push upstream master
  ```

### Collect included changes

Identify all pull requests that got merged since the last release.
  - Option 1: You can look at the [date of the last release](https://github.com/openfoodfoundation/openfoodnetwork/releases/latest) and use it to [filter pull requests](https://github.com/openfoodfoundation/openfoodnetwork/pulls?utf8=%E2%9C%93&q=is%3Apr+merged%3A%3E2019-08-13T20%3A00%3A00%2B02%3A00+sort%3Aupdated-desc+base%3Amaster) putting the date and time into the filter box like this: `is:pr merged:>2018-07-11T21:37:00+01:00 sort:updated-desc base:master`
  - Option 2: You can list all pull requests with git and open them in your browser:
    ```
    git fetch upstream
    git checkout upstream/master
    latest="$(git tag --sort="v:refname" | tail -1)"
    git log "$latest.." --merges --oneline | grep -oP 'Merge pull request #\K[0-9]+(?= from)' | while read n; do echo "https://github.com/openfoodfoundation/openfoodnetwork/pull/$n"; done | xargs firefox
    ```
**Get the release notes from each of these pull requests**. If no release notes are specified you can just copy the pull request title. We only include PRs that have been merged into `master`.

### Draft the release

Draft a [new release](https://github.com/openfoodfoundation/openfoodnetwork/releases/new) in the Github UI. **Base the release on the commit with the last merged PR you want to include** ('Target->Recent Commits') and not `master`. There's always a chance new PR's are merged between the draft is created and the actual release is published, so this makes the release consistent.

**Make sure the notes can be understood by humans** using the types of changes specified by [Keep a changelog](https://keepachangelog.com) and use only the sections that have at least one PR in it. Keep in mind these notes are the source of truth for everyone: devs, product people and users. Mention any dependencies on [ofn-install](https://github.com/openfoodfoundation/ofn-install) as well.

Unless agreement has been reached in the [#dev](https://openfoodnetwork.slack.com/messages/C2GQ45KNU) Slack channel that a major release is appropriate, releases only bump up the minor version (eg: from 1.16.0 to 1.17.0).

## Testing the release

Releases require some testing to ensure critical paths of the application keep working. Talk to one of the testers available to coordinate this. Once the release's draft is finished and she is aware, stage `master` to a server with PayPal and Stripe integrations set up. These need to be tested.

Keep in mind that any merges done to `master` while the release is in draft won't get tested unless we stage again. We don't have a specific release branch; we just have `master` and tag release version in it.

## Publish the release

**Wait for testers to give the ok** to publish the release; that will create a git tag for you. Check the [Testing the release](#testing-the-release) section above for details.

**Announce** in [#global-community](https://openfoodnetwork.slack.com/archives/C59ADD8F2) you just published a new release. Use the template below :point_down: :

    ```
    Hi all, just letting you know that we just released [version number] :tada:.
    You can read more about it here: https://github.com/openfoodfoundation/openfoodnetwork/releases/tag/[version number]
    ```
**Nudge the next person to release**. You can set reminders in Slack for the dev and the translators:

```
/remind @maikel “Probably time for a new release this week, it’s your turn :wink: Translators have just been notified to get the translations ready” in two weeks
```

```
/remind #translations “A new release will be prepared soon, it's a good time to review transifex ;-)” in two weeks
```

## Who has the release power :zap: :muscle: 

All Ha-Ri certified/core devs will now do [releases](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Pipeline-development-process#release) on a rotating basis.

## Deployment

We usually deploy a release for all instances straight away. Check the current [Deployment status](https://github.com/openfoodfoundation/ofn-install/wiki/Current-deployment-status) page for details. After you deployed, let the instance managers know so that they can run some basic tests. Include a link to the release notes page so that they can easily see what has changed.

> @channel The new release has been deployed to all instances. It's time for your after-deploy checks. You can see what was in the release here: < add the link to the release notes >
