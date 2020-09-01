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

Releases follow a set weekly schedule. The release manager in charge will **draft the release on Thursday**, then it will be ready for testers to do their magic and give their approval until the release manager will **publish it on Monday**. Then, we **deploy on Tuesday**.

## How to make a release :spiral_notepad: :white_check_mark: 


* Create a [New Release Issue](https://github.com/openfoodfoundation/openfoodnetwork/issues/new?template=release.md).

### Pull translations in

Check for a [Transifex pull request](https://github.com/openfoodfoundation/openfoodnetwork/pulls?utf8=%E2%9C%93&q=is%3Apr+is%3Aopen+head%3Atransifex) and merge it.

Download all current translations from Transifex with the [Transifex client](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Internationalisation-(i18n)#transifex-client) and add them to the master branch.

:warning: Make sure you don't have local uncommitted changes! You can run `git checkout .` to discard changes.
  ```sh
  git checkout master
  git pull upstream master
  tx pull --force
  git commit -a -m "Update all locales with the latest Transifex translations"
  git push upstream master
  ```

### Collect included changes

Identify all pull requests that got merged since the last release.
  - Option 1: You can look at the [last release](https://github.com/openfoodfoundation/openfoodnetwork/releases/latest) and **use the date of the pinned commit** to [filter pull requests](https://github.com/openfoodfoundation/openfoodnetwork/pulls?utf8=%E2%9C%93&q=is%3Apr+merged%3A%3E2020-01-16T20%3A00%3A00%2B02%3A00+sort%3Aupdated-desc+base%3Amaster) putting the date and time into the filter box like this: `is:pr merged:>2020-01-16T21:37:00+01:00 sort:updated-desc base:master`
  - Option 2: You can list all pull requests with git and open them in your browser:
    ```
    git fetch upstream
    git checkout upstream/master
    latest="$(git tag --sort="v:refname" | tail -1)"
    git log "$latest.." --merges --oneline | grep -oP 'Merge pull request #\K[0-9]+(?= from)' | while read n; do echo "https://github.com/openfoodfoundation/openfoodnetwork/pull/$n"; done | xargs firefox
    ```
**Get the release notes from each of these pull requests**. If no release notes are specified you can just copy the pull request title. We only include PRs that have been merged into `master`.

### Draft the release

Draft a [new release](https://github.com/openfoodfoundation/openfoodnetwork/releases/new) in the Github UI. **Base the release on the commit with the last merged PR you want to include** ('Target->Recent Commits') and not `master`. There's always a chance new PR's are merged between the draft is created and the actual release is published, so this makes the release consistent. If you take a look at the master branch in [Semaphore](https://semaphoreci.com/openfoodfoundation/openfoodnetwork-2/branches/master), you should see the Transifex commit you just made in the steps above (named "Update all locales with the latest Transifex translations").

**Make sure the notes can be understood by humans.** List all user-facing changes first under their own headline. Then list the rest of the changes. Mention any dependencies on [ofn-install](https://github.com/openfoodfoundation/ofn-install) as well. Copy the user-facing changes to the #instance-managers Slack channel so that they can let people know.

A lot of releases contain only minor bug fixes and tech updates. We then increase the patch version number (1.0.x). If there are significant user-facing changes, we bump the minor version number (1.x.0). The major number is only updated for very big changes like the Spree upgrade from v1 to v2.

### Update the Github issue

Add a link to the **Semaphore build** for the **target commit** of the new release, and a link to the **release draft** itself into the release's Github issue so the release tester has a nice time :heart:.

## Testing the release

Releases require some testing to ensure critical paths of the application keep working. Talk to one of the testers available to coordinate this (message in the #testing channel). Once the release's draft is finished and she is aware, you can **stage the target commit via the Semaphore build page** to a server with PayPal and Stripe integrations set up. These need to be tested.

Keep in mind that any merges done to `master` while the release is in draft won't get tested (unless the release is redrafted with a new target commit and the new build is staged).

## Publish the release

**Wait for testers to give the ok** to publish the release; that will create a git tag for you. Check the [Testing the release](#testing-the-release) section above for details.

**Announce** in [#global-community](https://openfoodnetwork.slack.com/archives/C59ADD8F2) you just published a new release. Use the template below :point_down: :

```
Hi all, just letting you know that we just released [version number] :tada:.
You can read more about it here: https://github.com/openfoodfoundation/openfoodnetwork/releases/tag/[version number]
```

**Nudge the next person to release**.

## Who has the release power :zap: :muscle: 

All Ha-Ri certified/core devs will now do [releases](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Pipeline-development-process#release) on a rotating basis.

## Deployment

We usually deploy a release for all instances straight away. Check the current [Deployment status](https://github.com/openfoodfoundation/ofn-install/wiki/Current-deployment-status) page for details. 

You can deploy to all production servers with:

```
ansible-playbook playbooks/deploy.yml --limit all-prod -e "git_version=[release tag]"
```

After you deployed, let the instance managers know so that they can run some basic tests. Include a link to the release notes page so that they can easily see what has changed. Post on #instance-managers slack channel:

```
@channel The new release has been deployed to all instances managed by the global team.
It's time for your after-deploy checks. You can see what was in the release here:
https://github.com/openfoodfoundation/openfoodnetwork/releases/latest

 
```