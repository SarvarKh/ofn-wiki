# Releasing

## When do release happen :steam_locomotive: :train: :train:

There's no set schedule for releasing. We just create them on demand when we
decide so in the #dev Slack channel.

## How to make a release :spiral_notepad: :white_check_mark: 

* Identify the pull requests that got merged since the last release.
* Get the release notes from each of these pull requests. If no release notes
    are specified you can just copy the pull request title.
* Draft a new release in the Github UI. Make sure the notes can be understood by
    humans.
* Unless agreement has been reached in the #dev Slack channel that a major
    release is appropriate, releases only bump up the minor version.
* Publish the release when all is OK; that will create a git tag for you.
* Announce in #deployment you just published a new release. Use the template
    below :point_down: :

    ```
    Hi all, just letting you know that we just released [version number]. You can read more about it here: [link to GitHub release]
    ```

## Who has the release power :zap: :muscle: 

[@oeoeaio](https://github.com/oeoeaio), [@mkllnk](https://github.com/mkllnk), [@enricostano](https://github.com/enricostano) and [@sauloperez](https://github.com/sauloperez)
