## Prioritization of Pull Requests

We manage the priority of PRs in the "Code Review" pipeline in Zenhub.

### Choosing a PR to review

A reviewer should choose the next PR from the top which they haven't reviewed yet in the PR's current round of reviews.

### Adding a PR to the pipeline

When adding a PR to the pipeline, do not simply add it to the top (LIFO) or the bottom (FIFO) of the pipeline.

From the following list, identify the first category that the PR meets, and add the PR to the bottom (FIFO) of the section of the pipeline for this category. The categories would follow the same order as below.

1. General blockers - For example, if the PR fixes a bug that prevents deployment or fixes failing tests in `master`.
2. S1 and S2 bugs
3. PRs no longer in 1st round of reviews
4. PRs with minimal review needed
5. Other bugs and features

Let's say that, in the pipeline, there is a PR for an S2 bug and a feature PR that is already in its 2nd round of reviews. See the following examples:

Category | PR in Pipeline | &nbsp;
--- | --- | ---
S1 and S2 bugs | PR for S2 bug | 
S1 and S2 bugs | &nbsp; | &lt;- Insert PR for S1 bug
S1 and S2 bugs | &nbsp; | &lt;- Insert another PR for S2 bug
PRs no longer in 1st round of reviews | PR in 2nd round of reviews | 
PRs no longer in 1st round of reviews | &nbsp; | &lt;- Insert another PR in 2rd round of reviews
Other bugs and features | &nbsp; | &lt;- Insert new PR for feature