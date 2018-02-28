# Releasing

## When do releases happen :steam_locomotive: :train: :train:

There's no set schedule for releasing. We just create them on demand when we
decide so in the [#dev](https://openfoodnetwork.slack.com/messages/C2GQ45KNU) Slack channel.

## How to make a release :spiral_notepad: :white_check_mark: 

* Check for a [Transifex pull request](https://github.com/openfoodfoundation/openfoodnetwork/pulls?utf8=%E2%9C%93&q=is%3Apr+is%3Aopen+head%3Atransifex). If there is no pull request, but a branch named `transifex` with new translations, then open a pull request and merge it. After merging the pull request, delete the `transifex` branch.
* Identify all pull requests that got merged since the last release. You can look at the [date of the last release](https://github.com/openfoodfoundation/openfoodnetwork/releases) and use it to [filter pull requests](https://github.com/openfoodfoundation/openfoodnetwork/pulls?utf8=%E2%9C%93&q=is%3Apr+merged%3A%3E2018-02-08) putting the date into the filter box like this: `is:pr merged:>=2018-02-08`
* Get the release notes from each of these pull requests. If no release notes are specified you can just copy the pull request title.
* Draft a [new release](https://github.com/openfoodfoundation/openfoodnetwork/releases/new) in the Github UI. Make sure the notes can be understood by humans.
* Unless agreement has been reached in the [#dev](https://openfoodnetwork.slack.com/messages/C2GQ45KNU) Slack channel that a major release is appropriate, releases only bump up the minor version.
* Publish the release when all is OK; that will create a git tag for you.
* Announce in [#deployment](https://openfoodnetwork.slack.com/messages/C0DNLAZC7) you just published a new release. Use the template below :point_down: :

    ```
    Hi all, just letting you know that we just released [version number].
    You can read more about it here: https://github.com/openfoodfoundation/openfoodnetwork/releases/tag/[version number]
    ```
* Create the `transifex` branch again to allow new translations to be published there: `git fetch origin master && git push origin FETCH_HEAD:refs/heads/transifex`

## Who has the release power :zap: :muscle: 

[@oeoeaio](https://github.com/oeoeaio) `(Australia/Melbourne)`, [@mkllnk](https://github.com/mkllnk) `(Australia/Melbourne)`, [@enricostano](https://github.com/enricostano) `(Europe/Madrid)` and [@sauloperez](https://github.com/sauloperez) `(Europe/Madrid)`
