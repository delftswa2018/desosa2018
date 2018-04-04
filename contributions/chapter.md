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
