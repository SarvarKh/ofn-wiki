## Make current code future proof 

Refactor the current code so that it is compatible with the upcoming spree
version. This then can be merged into master without having to depend on the
particular version of our Spree fork.

## Do not tackle a major breaking change in a single PR

When a breaking change in Spree affects few different OFN features or modules,
such as the removal of a database column, tackle them in small isolated PRs that
deal only with one of these modules/features.

## Go with a major breaking change at a time

Do not pick the next major breaking change until all the PRs related to previous
one are merged.

This helps sharing the knowledge across the whole team as work can be split up
and reduces the chances of having conflicts and dependencies between breaking
changes.

It is also easier to focus on a single change than trying to remember all the
bits and pieces of few of them at the same time.

## Provide context and comparison across versions

Always provide the context of the change and explain how things work now and how
they will in the upcoming version, so that people understand why the change is
needed.

## Get insights from more experienced devs

Ask other more experiencied developers when digging into specific and cumbersome
OFN features/modules that are affected by the upgrade. Make sure you understand
what's the purpose of said feature/model including all it's hidden #dragons.

## Document all the customizations

Take the time to time to document all the customizations built on top of Spree
that you encounter.

## Reconsider why things were implemented this way in the first place

Give a chance to reducing the amount of customization made on top of Spree. Step
back when you see code affected by a change. Why was that customization added in
the first place? Was it really needed? Can it be done in a simpler
way? Can you change a method or variable name that
makes its intent a bit more clear?

Always aim for the smallest maintenance cost and greatest changability.
