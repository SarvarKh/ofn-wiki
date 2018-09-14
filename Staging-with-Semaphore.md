> **This is an experimental feature with the Australian staging server. Please report to Maikel on Slack if you encounter any problems.**

You can stage pull requests to https://staging.openfoodnetwork.org.au/ yourself. A developer has to give you access to Semaphore CI. Then you can navigate to the staging process from any pull request. But before you stage, follow the checklist to prevent conflicts:

- Check that nobody else is using the staging server.
- Check that the pull request is approved, in the *Test Ready* column and has no test failures or merge conflicts. It needs Three big green ticks at the bottom of the pull request (nothing red).
- Let others know that you are staging it by commenting on the pull request and putting a label on.

![stage-with-Semaphore.gif](animation of staging with Semaphore)

What happens technically:

- The pull request and master are merged so that the result is staged and tested.
- The merge commit is pushed to the staging server.
- The server resets the database to its baseline data.
- A deploy script is run.
