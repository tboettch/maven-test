A few Maven projects illustrating a bug I've been encountering.

Project Relationships:
* projectA produces two artifacts with different classifiers: projectA and projectA-someclassifier
* projectB depends directly on projectA-someclassifier (and also junit, for illustration purposes).
* projectC depends directly on projectB and projectA (note the lack of classifier) and transitively on projectA-someclassifier

To see the bug, install projectA and projectB to your local repository, then run the following commands on projectC:
* mvn dependency:resolve
* mvn dependency:resolve -DexcludeTransitive=true

In the latter case, projectA-someclassifier should be excluded from the list, but it is actually included. Note that the "ordinary" transitive dependency of junit _is_ excluded. Furthermore, try running those same commands again after removing projectA from projectC's dependencies: with the second command, both projectA and projectA-someclassifier disappear from the list, even though only the direct dependency was changed!
