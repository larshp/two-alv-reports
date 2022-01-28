# The case of two ALV reports and git for deployment

Assumption: QAS system and PRD system

Assumption: the two ALV reports already exist in QAS and PRD

Assumption: the reports are in the same git repository(because, clearly we don't want to have one repository per object?)

Two developers, two changes, both developers create a branch, and push two commits,

CHG1: Add new column COL1 to ZALV1

CHG2: Add new column COL2 to ZALV2

![](commits.drawio.svg)

Development finishes, reviewed, QA'ed and all, say CHG1 before CHG1, say simple classic merge, deployed to QAS for business testing,

![](deployed.drawio.svg)

It happens that both changes contain a bug, business would like to move the column one step to the left, the developers create bugfixes in new branches, the old ones have been merged.
(Assumption: dont rewrite history)

![](bugfixed.drawio.svg)

CHG1 is accepted by business before CHG2, and would like to urgently have the change in production.

But none of the commits in the history have a consistent state for deployment of CHG1, they will all draw some unwanted changes from CHG2!

With classic CTS the blue and red changes are not intertwined, but must be manually kept track of(tooling does exist, and solutions are known to not get into this kind of trouble)

> It feels like it would end up with the same as STMS, a list of branches for each system that has not been merged to that system/branch yet, with similar pitfalls as CTS.
>
> Reference https://blogs.sap.com/2020/11/05/cts-is-beautiful/