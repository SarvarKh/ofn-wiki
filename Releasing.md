# Releasing

## When do releases happen :steam_locomotive: :train: :train:

There's no set schedule for releasing. We just create them on demand when we
decide so in the [#dev](https://openfoodnetwork.slack.com/messages/C2GQ45KNU) Slack channel.

## How to make a release :spiral_notepad: :white_check_mark: 

* Identify all pull requests that got merged since the last release.
* Get the release notes from each of these pull requests. If no release notes are specified you can just copy the pull request title.
* Draft a [new release](https://github.com/openfoodfoundation/openfoodnetwork/releases/new) in the Github UI. Make sure the notes can be understood by humans.
* Unless agreement has been reached in the [#dev](https://openfoodnetwork.slack.com/messages/C2GQ45KNU) Slack channel that a major release is appropriate, releases only bump up the minor version.
* Publish the release when all is OK; that will create a git tag for you.
* Announce in [#deployment](https://openfoodnetwork.slack.com/messages/C0DNLAZC7) you just published a new release. Use the template below :point_down: :

    ```
    Hi all, just letting you know that we just released [version number].
    You can read more about it here: https://github.com/openfoodfoundation/openfoodnetwork/releases/tag/[version number]
    ```

## Who has the release power :zap: :muscle: 

[@oeoeaio](https://github.com/oeoeaio) `(Australia/Melbourne)`, [@mkllnk](https://github.com/mkllnk) `(Australia/Melbourne)`, [@enricostano](https://github.com/enricostano) `(Europe/Madrid)` and [@sauloperez](https://github.com/sauloperez) `(Europe/Madrid)`
