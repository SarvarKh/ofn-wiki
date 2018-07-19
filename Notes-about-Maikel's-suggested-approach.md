# Maikel's approach

## Pros

* Less work
* Allows merging now
* Less troublesome merge conflicts
* Makes every dev aware of changes in the data model
* Fewer divergenses between master and 2-0-stable. New development needs to work
    with Spree 2.0 as well.

## Cons

* Duplicates logic. If we change code in Spree we might need to do as well in OFN. Easy to forget.
* It digs deeper on the metaprogramming approach of Spree. Lack of any sort of
    encapsulation. Getting closer to a Bash script.
* We do not decouple, which won't make the next upgrade step any easier.
* We need to cleanup the code before doing another upgrade.
* Requires testing both Spree versions. Might not be possible now and might only
    be possible by the end of the upgrade. Can testers test the 1.3 branch while
    we test the 2.0 branch, merge and we let them test when it's possible?
* Increases the complexity of the codebase considerably. Less temporal than we
    think...
* The units tests of this overriden methods will be very hard to write do to the
    many their many responsibilities

## To consider

We need to add deprecation warnings to the 1.3 conditional branches.

How does this related/affect the extension hooks approach?

We stopped making the impossible to make things backward compatible in favor of
moving forward. We saw there were way too many movable pieces and steps that
made it much harder.
