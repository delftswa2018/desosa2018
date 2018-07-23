# Contributions

This chapter outlines some of the contributions made by several teams to their open source project.

## Kubernetes

## Mattermost

## Docker

### Pull Requests

Here the effort made to get our Pull Requests to `docker/swarmkit` merged is described.

#### Possible data race in test ([docker/swarmkit#2537](https://github.com/docker/swarmkit/pull/2537))

This was found in the project file `manager/state/store/memory_test.go` at lines 2036-2054.

The issue was a loop variable being used in a `func literal` (anonymous function in Go). Due to the nature of Go, this would lead to unexpected results. [Example from Go reference](https://nanxiao.gitbooks.io/golang-101-hacks/content/posts/functional-literals.html):

```
package main

import (
    "fmt"
    "time"
)

func main() {
    for i := 1; i <= 2; i++ {
        go func() {fmt.Println(i)}()
    }
    time.Sleep(time.Second)
}
```

Expected output here would be:

```
1
2
```

However the output would be:

```
3
3
```

["The cause is the func goroutines don't get the opportunity to run until the main goroutine sleeps, and at that time, the variable i has been changed to 3"](https://nanxiao.gitbooks.io/golang-101-hacks/content/posts/functional-literals.html)

Something similar was happening in the SwarmKit test.
So according to the `Contributions.md` in the SwarmKit repository first an issue was opened ([#2536](https://github.com/docker/swarmkit/issues/2536)) describing the problem.
After the issue was opened, a new branch was created in our fork, with the naming convention mentioned in the SwarmKit `Contributions.md` (#issue-*, in our case `delftswa2018:2536-data-race`).

When the issue was fixed, a Pull Request was opened ([#2537](https://github.com/docker/swarmkit/pull/2537)) which was accepted several hours later by [@aaronlehmann](https://github.com/aaronlehmann).

#### Add company and date to license ([docker/swarmkit#2589](https://github.com/docker/swarmkit/pull/2589))

The LICENSE of the project missed the name of the company and the date. Without these fields the LICENSE is not valid and does not apply to the project.

We added these fields and submitted a PR. The PR has been approved and merged, with one small change of squashing to commits into one.

#### Upgrade containerd executor ([docker/swarmkit#2595](https://github.com/docker/swarmkit/pull/2595))

In PR #2568 the "containerd" executor was removed, because it targeted an older version of containerd which was not being maintained anymore.

This PR aims to reintroduce the containerd executor (v1.0.2). This version is being actively maintained.

Currently the PR is under review by the team. There is one comment on the PR stating that instead of using a static version of containerd, just use the master of containerd such that the vendor will always be up to date.

### Issues

### Make script fails on go version go1.10 darwin/amd64 ([docker/swarmkit#2563](https://github.com/docker/swarmkit/pull/2563))

There was an issue with the `certificate-transparency` in go1.10 darwin/amd64. When running the `make` command the following error appeared.

```text
# github.com/docker/swarmkit/vendor/github.com/google/certificate-transparency/go/x509
vendor/github.com/google/certificate-transparency/go/x509/root_darwin.go:73: cannot use nil as type _Ctype_CFDataRef in assignment
make: *** [bin/swarmd] Error 2
```

The fix for the error is updating the `certificate-transparency` vendor to `certificate-transparency-go` and getting the latest version. As the issue with the nil assignment has been fixed in [google/certificate-transparency-go#155](https://github.com/google/certificate-transparency-go/pull/155).

## OSU

## React

## LoopBack

## Issue #925
[Issue #925](https://github.com/strongloop/loopback-next/issues/925) is marked as "Good first issue" by the maintainers of loopback-next. 
In the description of the issue, one of the developers proposes a fix for moving the start error handling from the main function to the main index file. 
The fix proposed by the developer has been implemented and successfully tested using the build in tests.

We submitted a pull request [PR #1151](https://github.com/strongloop/loopback-next/pull/1151) to loopback-next according to the LoopBack guidelines.
After dealing with the code linter and some small required changed proposed by the developers it was approved.
Three of the developers (of which one code owner) approved it and it was merged 3 days after submitting it.

## Issue #1218
While analysing the code structure for technical debt we proposed a change of the package structure in [issue #1218](https://github.com/strongloop/loopback-next/issues/1218).
Coincidently one of the code owners also thought of the same change just before we submitted the issue.
The developer looking at our submitted issue asked if we wanted to submit a pull request for this issue.

Since it seemed to be a very simple change we started working on the pull request.
Unfortunately it was a more complex issue requiring changing of several configuration files, some documentation changes and a small change in the code.
Eventually we were able to fix all problems and submitted [pull request #1231](https://github.com/strongloop/loopback-next/pull/1231) which is waiting for approval.

## Electron

## Akka

## ElasticSearch

## Spark

## Jenkins

## AngularJS

## TypeScript

## Eden

## Lighthouse

## Phaser

## Xmage

### Contributions

First an overview of the contributions. A more detailed description follows.

| #PR | PR | Merged | Released | Category | Files changed | Additions | Deletions |
| - | - | - | - | - | - | - | - |
| [#4573](https://github.com/magefree/mage/pull/4573) | Implemented Uphill Battle 									| 03/08/2018 | 03/10/2018 	| Feature 			| 3 	| 171 	| 1 	|
| [#4584](https://github.com/magefree/mage/pull/4584) | Typo in classname 											| 03/08/2018 | 03/10/2018 	| Technical Debt 	| 2 	| 3 	| 3 	|
| [#4617](https://github.com/magefree/mage/pull/4617) | Fire PLAY_LAND event only after replace check 				| 03/13/2018 | 03/18/2018 	| Hotfix			| 1 	| 1 	| 1 	|
| [#4648](https://github.com/magefree/mage/pull/4648) | Blocker and Critical level bugfixes throughout the project 	| 03/22/2018 | - 			| Technical Debt	| 31 	| 285 	| 273 	|
| [#4680](https://github.com/magefree/mage/pull/4680) | Improved XMage startup time 								| 03/29/2018 | - 			| Performance		| 2 	| 50 	| 37 	|
| [#4681](https://github.com/magefree/mage/pull/4681) | Clickable message of the day 								| 03/29/2018 | - 			| Solve issue		| 2 	| 122 	| 2 	|
| [#4682](https://github.com/magefree/mage/pull/4682) | Resolving unaccepted changes 								| 03/29/2018 | - 			| Technical Debt	| 3 	| 5 	| 3 	|
| [#4683](https://github.com/magefree/mage/pull/4683) | Fix readme.md card count 									| 03/29/2018 | - 			| Documentation		| 1 	| 1 	| 1 	|
| [#4707](https://github.com/magefree/mage/pull/4707) | SOLID violation fix in token classes 						| 04/04/2018 | - 			| Technical Debt	| 593 	| 5099 	| 886 	|
| [#4709](https://github.com/magefree/mage/pull/4709) | Fixed test errors caused by Elvish Impersonator 			| 04/03/2018 | - 			| Hotfix			| 2 	| 2 	| 1 	|

#### [#4573 Implemented Uphill Battle ](https://github.com/magefree/mage/pull/4573)
| Progress | When | Note |
| - | - | - |
| Created | 03/01/2018 | Added card [Uphill Battle](http://gatherer.wizards.com/Pages/Card/Discussion.aspx?multiverseid=20786) |
| Fix | 03/02/2018 | Fix checking the wrong condition |
| Fix | 03/06/2018 | Fix special card interactions |
| Requested | 03/06/2018 | Included question, still work in progress |
| Completed work in progress part | 03/08/2018 | |
| Merged | 03/08/2018 | by LevelX2 |
| Released | 03/10/2018 | [1.4.28V0](https://github.com/magefree/mage/releases/tag/xmage_1.4.28V0) | 

This is our first contribution. We added the card [Uphill Battle](http://gatherer.wizards.com/Pages/Card/Discussion.aspx?multiverseid=20786) to _Mage.Sets_. It is a card that was requested for a while. It is a pretty simple card, but has some complex interactions with other cards. Due to this, we thought it would be a good start.
The effect of the card had some conflicts with some other specific cards which made the implementation a bit harder than expected.

#### [#4584 Typo in classname ](https://github.com/magefree/mage/pull/4584)
| Progress | When | Note |
| - | - | - |
| Requested | 03/08/2018 |  |
| Merged | 03/08/2018 | by LevelX2 |
| Released | 03/10/2018 | [1.4.28V0](https://github.com/magefree/mage/releases/tag/xmage_1.4.28V0) | 

Our second contribution during the first period of the course, ran into a typo in a classname while analysing the project.

#### [#4617 Fire PLAY_LAND event only after replace check](https://github.com/magefree/mage/pull/4617)
| Progress | When | Note |
| - | - | - |
| Requested | 03/13/2018 | by LevelX2 |
| Merged | 03/13/2018 |  |
| Released | 03/18/2018 | [1.4.28V1](https://github.com/magefree/mage/releases/tag/xmage_1.4.28V1) | 

Hotfix for first contribution, which could cause a potential issues in extremely rare situations.

#### [#4648 Blocker and Critical level bugfixes throughout the project](https://github.com/magefree/mage/pull/4648)
| Progress | When | Note |
| - | - | - |
| Requested | 03/22/2018 |  |
| Merged | 03/22/2018 | by jeffwadsworth  |
| Released | - | | 

We found some bugs and vulnerabilities through a SonarQube scan, this was done during the identification of technical debt. We fixed the highest severity issues, amounting to a total of about 30 issues fixed


#### [#4680 Improved XMage startup time](https://github.com/magefree/mage/pull/4680)
| Progress | When | Note |
| - | - | - |
| Created | 03/20/2018 | Improved startup time |
| Requested | 03/29/2018 |  |
| Merged | 03/29/2018 | by JayDi85 |
| Released | - |  | 

One of the many annoyances with XMage is the loading times at many stages of usage. We started improving this by looking into the  first thing you do: start the program.

#### [#4681 Clickable message of the day](https://github.com/magefree/mage/pull/4681)
| Progress | When | Note |
| - | - | - |
| Created | 03/27/2018 | Make URLs in MOTD clickable |
| Requested | 03/29/2018 |  |
| Merged | 03/29/2018 | by JayDi85 |
| Released | - |  | 

The message of the day often has URL in it, however these URL's are not clickable nor can you select and copy them. In this pull request we introduced the feature to make the URL's in the message of the day clickable and refer them to the correlating website.


One of the many annoyances with XMage is the loading times at many stages of usage. We started improving this by looking into the  first thing you do: start the program.

#### [#4682 Resolving unaccepted changes](https://github.com/magefree/mage/pull/4682)
| Progress | When | Note |
| - | - | - |
| Created | 03/29/2018 | Fix issues related with [PR](https://github.com/magefree/mage/pull/4648) |
| Requested | 03/29/2018 |  |
| Merged | 03/29/2018 | by JayDi85 |
| Released | - |  | 

Fix issues related with [PR](https://github.com/magefree/mage/pull/4648)

#### [#4683 Fix readme.md card count](https://github.com/magefree/mage/pull/4683)
| Progress | When | Note |
| - | - | - |
| Created | 03/29/2018 | Fix readme.md card count |
| Requested | 03/29/2018 |  |
| Merged | 03/29/2018 | by JayDi85 |
| Released | - |  | 

Fixed a typo in the readme. There was one too many zeros in the card count.

#### [#4707 SOLID violation fix in token classes](https://github.com/magefree/mage/pull/4707)
| Progress | When | Note |
| - | - | - |
| Created | 04/02/2018 | Change Token to abstract TokenImpl, new interface Token |
| Requested | 04/03/2018 |  |
| Merged | 04/04/2018 | by JayDi85 |
| Released | - |  | 

There was a violation of one of the SOLID principles: the [Dependency inversion principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle) in the Token class. The situation currently is as follows (botton right area is most relevant): 
![xmage dependency_before](https://user-images.githubusercontent.com/5969155/38249750-f47989cc-374c-11e8-8d44-c6540006e2d3.png)

Here we see that Card references are depending on the Card interface rather than the actual implemented Cards. For Tokens this was not the case, dependencies were on the Token class, which even could be instantiated. Although generally it was used correctly (ie, extending the class for each implemented Token) there were two occasions of the Token class itself being instantiated.

The following changes have been done, the Token class was made abstract and renamed to TokenImpl, after this the Token interface was extracted. All dependencies on TokenImpl were set to the Token interface. The concrete Token usages (ie, "new Token(..)") were replaced with new Token implementations (same behaviour was verified by testing).

Finally, like each Card, each Token implementation now requires a copy method which in turn relies on a copy constructor, both have been implemented for every Token in the game.


#### [#4709 Fixed test errors caused by Elvish Impersonator](https://github.com/magefree/mage/pull/4709)
| Progress | When | Note |
| - | - | - |
| Created | 04/03/2018 | Fixed creature type of Elvish Impersonator |
| Requested | 04/03/2018 |  |
| Merged | 04/03/2018 | by JayDi85 |
| Released | - |  | 

A wrong subtype in the Elvish Impersonator caused the build system to fail on verifying the card properties. This was done in a direct commit to master, so no reviewing was done. To fix this, the new substype was added to the engine, and the card was updated to use this new type.


### Handling pull requests

After making a number of pull requests, we made a few observations as to the merging process used.

Most pull requests are responed to within a day.
All of the pull requests we did were responded to within a week.
We noticed two major issues with they way pull requests are merged.

#### Self-merged pull requests

Some developers merge their own pull requests.
The most notable user is _spjspj_, who often merges their own pull requests before the CI has checked the code.

#### Reverse merge strategy

The "strategy" looks like the following:
1. Thank for contributing
1. Merge pull request
1. Mention potential issues with pull request
1. Mention issues found after testing the PR

Besides causing issues with wrong code ending up in the master branch, it also makes it harder to fix. This is due to updating the pull request no longer being possible after it is merged. A new pull request has to be created if a merged pull request still requires changes.




## Mbedos

## Vue.js

## Godot

### Fix for Creation of Folders with Trailing Dots in Names for Godot on Windows

Windows does not support folders whose name ends with a dot. Not only is access not possible, but also is the deletion of them, without other tricks and hacks. While the creation of such folders is not possible via Windows Explorer, it was possible via the file manager within the Godot editor.


In theory, the issue can be easily fixed by checking the name given by the user, and denying it if it ends with a dot, if the current OS is Windows. However, the first and biggest challenge is to find out where to make such modification. For such a large-scale C++ project, navigation is absolutely a nightmare, especially when the comments are nowhere to be seen. This was eventually solve with the help of the powerful debugging tools of the Visual Studio.


After the pull request [#17243](https://github.com/godotengine/godot/pull/17243) was submitted, a question was raised by other contributors, that if such folders cause problems for Windows users, then they should not be allowed to created by users of other platforms either, to preserve the compatibility of the system. This advice was taken and the check for OS was dropped.

###    Enum Identifier Lookup Fix for Godot Script Editor

A user reported in issue [#17396](https://github.com/godotengine/godot/issues/17396) that enum lookup doesn't work properly for the script editor. After examination of the codes, it was discovered that the multiple use cases mentioned by the user were caused by numerous different reasons. Work was then done to fix the reason behind the biggest group of use cases.


Again, with the help of Visual Studio debugging tools, we obtained a basic understanding of how the identifier lookup works. When the user clicks a word in the script editor, inquiries will be made to other relevant classes to determine the nature of the chosen identifier. After the relevant documentation page is opened, it will automatically scrolls down to the part relevant to the chosen identifier, by searching through the page. The problem is that, the classes responsible for determining the nature of the word does not treat enum values and constant values differently, while the classes responsible to find that word within the documentation page do. Therefore, for an enum word, it will be treated as a constant word by the search function, which means the search function would never find the word within the constant section of the page, therefore leads to the issue that the page doesn't scroll down.


The first iteration of the solution was to have the search function check for the target word within both the constant and enum section of the page, whenever the nature of the word is determined to be a constant. It worked. However, it was scrapped a few days later because we feared that it is a workaround by the very definition, and does not help to reduce the technical debts of the system. In fact it adds more.


To at least avoid adding more technical debt, we had no choice but looked into the classes responsible for determining the nature of the word, and make it so that they separate enum and constant words. The biggest problem that searching through all the enum values is different than searching through all the constants. While the latter, in structure, is just a one-dimensional array, the former one can be seen as a matrix. For each enum type there are multiple variations of values. Another hindrance is that the Godot project works with its own customized container templates instead of std templates, and they are not documented or explained anywhere. This making writing traverse function for the enum types extremely difficult. However, after browsing through relevant class headers, we found legacy codes that do just that, but were hidden because they were not utilized, since in the end the decision back then was to not distinguish enums from constants. By re-utilizing these legacy codes we fixed the problem, for most cases.


This, however, exposed another issue, that the lookup classes cannot search through the global scope for identifiers. It was decided that for global scope words, the determination of their nature will be handled by documentation classes once the page is loaded. Therefore it is still a workaround, but we believe it is justifiable, since it would otherwise require the total rework of how the lookup functions are written, and extends the scope of the pull request uncontrollably. 


The final solution fixes the lookup for identifiers that belong to a class, and use the workaround routine for global scope words. After the PR was merged, we created new issues to alert other devs of the similar use cases that remain unsolved.

## Three.js
