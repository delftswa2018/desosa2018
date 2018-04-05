# Contributions

This chapter outlines some of the contributions made by several teams to their open source project. 

## Kubernetes

## Mattermost

## Docker

## OSU

## React

## Loopback

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

## Three.js
