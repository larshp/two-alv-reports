# The case of two ALV reports and git for deployment

## Scenario?

Assumption: QAS system and PRD system

Assumption: the two ALV reports already exist in QAS and PRD

Assumption: the two ALV reports are in the same git repository(because, clearly we don't want to have one repository per object?)

Two developers, two changes,

CHG1: Add new column COL1 to ZALV1

CHG2: Add new column COL2 to ZALV2

both developers create a branch, and push two commits,

![](commits.drawio.svg)

Development finishes, reviewed, QA'ed and all, say CHG1 before CHG1, say simple classic merge, deployed to QAS for business testing,

![](deployed.drawio.svg)

It happens that both changes contain a bug, business would like to move the column one step to the left, the developers create bugfixes in new branches, the old ones have been merged.
(Assumption: dont rewrite history)

![](bugfixed.drawio.svg)

CHG1 is accepted by business before CHG2, and would like to urgently have the change in production.

But none of the commits in the history have a consistent state for deployment of CHG1, they will all draw some unwanted changes from CHG2!

## Solutions?

Making releases to customers every 3 months(like some large software companies) will of cause solve the issue, but it should not be the benchmark in a world with buzzword trends.

Add commits to rollback CHG2? Possible, but not feasible, think of a system with 10 developers and 100 changes in flight.

With classic CTS the blue and red changes are not intertwined, but must be manually kept track of(tooling does exist, and solutions are known to not get into this kind of trouble).

Use git for development, not deployment. SAP offers great tooling like CTS to deploy to complex landscapes with massive amounts of code.

[Cherry picking](https://medium.com/captain-contrat-engineering/cherry-picking-our-way-to-production-fc36968c7664)? Yeap, well, it rewrites history, which we'd probably not like to do as default for critical business systems.

> It feels like it would end up with the same as STMS, a list of branches for each system that has not been merged to that system/branch yet, with similar pitfalls as CTS.
>
> Each traditional CTS transport can be seen as a “mini” branch for the objects that it contains, which is automatically rebased when imported to the target system. It would also be possible to put every object in its own package to simulate this behavior in git, but it would be a lot of work and not help developers.
>
> Reference https://blogs.sap.com/2020/11/05/cts-is-beautiful/