# XMage: Magic Another Game Engine
By [Christiaan van Orle](https://github.com/chrvanorle), [Siyu Chen](https://github.com/sylviachsy), [Danny Plenge](https://github.com/Dahny) and [Marc Zwart](https://github.com/Arcademode)

![Team XMage](https://github.com/delftswa2018/team-xmage/wiki/images/TeamCards.png "Team XMage")  


# Abstract

XMage is an open source Magic the Gathering client and game engine that allows you to play Magic the Gathering games online against human or computer players with full rules enforcement. The core developers get help from many contributors to keep the engine up to date with new card set releases. XMage's architecture is analyzed from multiple perspectives and viewpoints. An overview of the architecture is provided, as well as an analysis of the technical debt present in the system.

# Table of Contents
- [Introduction](#introduction)
- [Stakeholders](#stakeholders)
  * [Power vs Interest Grid](#power-vs-interest-grid)
- [Context View](#context-view)
  * [External Entities](#external-entities)
- [Development View](#development-view)
  * [Module Organization](#module-organization)
  * [Codeline Organization](#codeline-organization)
- [Technical Debt Analysis](#technical-debt-analysis)
  * [Internal Dependencies](#internal-dependencies)
  * [Code Metric Analysis](#code-metric-analysis)
  * [Code Quality](#code-quality)
  * [Testing Debt](#testing-debt)
  * [Discussions about Technical Debt](#discussions-about-technical-debt)
  * [Evolution of Technical Debt](#evolution-of-technical-debt)
- [Deployment View](#deployment-view)
  * [Third-party Software Requirements](#third-party-software-requirements)
  * [System Requirements](#system-requirements)
  * [Server Networking Requirements](#server-networking-requirements)
  * [Runtime](#runtime)
- [Regulation Perspective](#regulation-perspective)
  * [Open Source License](#open-source-license)
- [Usability Perspective](#usability-perspective)
  * [Users and Capabilities](#users-and-capabilities)
  * [Server Administrator Interaction](#server-administrator-interaction)
  * [Player Interaction](#player-interaction)
- [Conclusion](#conclusion)
- [Bibliography](#bibliography)

# Introduction
XMage [[5]](#bibliography) is a project which started development in early 2010. Its goal is to make a free, open-source, online playable computer game version of the original Magic the Gathering card game with actual rule enforcement, something which competitors do not have.
The project is written in Java and was initially developed by [_BetaSteward_](https://github.com/BetaSteward), who still moderates the online forums of the XMage project. Nowadays, it is in the hands of [_LevelX2_](https://github.com/LevelX2).

The chapter starts with performing a stakeholder analysis and a context view of XMage. After that, the module organization and the codeline organization are presented in the development view section. The technical debt is analyzed from different points of view, followed by the deployment view, where the requirements to run the system are demonstrated. Additionally, the project is discussed separately from regulation perspective and usability perspective. Finally, a conclusion ends the whole chapter.

# Stakeholders
This section presents an overview of the types of stakeholders found in the XMage project. Besides the Stakeholders as defined by Rozanski and Woods [[1]](#bibliography) we have also Identified [Competitors](#competitors) and [Non-sourcecode Contributors](#non-sourcecode-contributors). We could not identify other stakeholder types than the mentioned ones.
This diagram is a general overview of all the important stakeholder categories in the project. Per stakeholder category, including the additional stakeholder categories we found ourselves, it shows all the relevant stakeholders. Figure 1 below is a more detailed description of all the stakeholder categories.
Next, the [Power/Interest grid](#powerinterest-grid) and the [Context view](#context-view) based on the stakeholder analysis will be presented.

![Stakeholders](https://github.com/delftswa2018/team-xmage/wiki/images/Stakeholders.png "Stakeholder Overview")

**Figure 1:** _Stakeholder Overview_

## Power vs Interest Grid
This section introduces a power/interest grid based on the stakeholder types we have found and analyzed, as shown in Figure 2, each stakeholder type is represented by a MTG card [[2]](#bibliography). The power and interest of each type of stakeholder is based on the pull request analysis, issue analysis and the analysis of the forum, Gitter and Reddit.

![Power vs Interest grid](https://github.com/delftswa2018/team-xmage/wiki/images/power_interest.jpg "Power vs Interest grid")

**Figure 2:** _Power vs. Interest Grid_

# Context View
The Context View contains the relationships, dependencies and interactions of the system with the environment, as shown in Figure 3. 
It shows the communications channels used in order to communicate, the most important stakeholders, the dependencies of the system and the operating systems the system supports. 

![Context View Diagram](https://github.com/delftswa2018/team-xmage/wiki/images/Context%20View.png "Context View Diagram")

**Figure 3:** _Context View Diagram_

## External Entities
This section touches the external entities of the XMage project, we will elaborate on the different dependencies and the communication platform the community uses.
As the project is written in Java, all Operating Systems can run the XMage game.

### Community
The XMage project largely lives around its community, the users, developers, communicators etc. communicate with each other via several different platforms.
Users among each other tend to use the Reddit pages, users may also post help requests on either the Reddit or Forum to find communicators that can help them resolve their issue.
Developers communicate with each other via the Gitters and Github. The Discord channel is used for finding other users and is even used for tournament communication.

### Dependencies
The XMage project has several dependencies of different types. The entire project is a Maven project written in Java which consists out of different Java projects.
The project can be build using any Java supporting IDE, however at the moment there is only support for the Netbeans and IntelliJ IDEs. The continuous integration is done by Travis which uses JUnit for testing. 
The client server communication is handled by JBoss Remoting 2. In order to provide quick access to card data a H2 database is used for the client, server and test project.
The project also has some utility scripts written in Perl which can help the developer generate code or build certain projects.

# Development View
In this section we will start by looking at the XMage project [[3]](#bibliography) at module level, identifying module organization and design patterns. Some light will also be shed on the codeline organization, further discussing source code organization and the build approach.

## Module Organization
In this section we will look at the organization of the XMage project from a Module perspective. The XMage project is separated into two major parts, the client and the server. Besides this a validation step is included in the build process which forms a relevant part of the project.

### Client-Server Architecture
Here we will discuss the client-server architecture used in the system, as well as what modules are used in which component.

![ClientServer](https://github.com/delftswa2018/team-xmage/wiki/images/ClientServer.png "ClientServer")

**Figure 4:** _A High Level Overview Architecture in the XMage Project_

#### Structure
In XMage, both client and server are running as separate instances of the game. Each instance communicates with the other via sockets, this communication layer for both client and server is implemented in the _Mage.Common_ module. The server manages the game's state and updates the clients, where clients send their respective actions to the server. Both the client and the server implement the core game logic by depending on the Mage module. A high-level overview of the main (runtime) components of the software is shown in Figure 4.

The overview in Figure 5 shows the modules present in the system. Some modules live in both client and server programs, such as the _Mage_ module. The validity assessment program is not present on either server or client, it is not used on production systems at all. The _Mage.Server_ and _Mage.Client_ modules are responsible for running the client and server programs. They load the required resources, such as the cards and game logic (plugins).

![Module organization](https://github.com/delftswa2018/team-xmage/wiki/images/ModuleOrganization.jpg "Module Organization")

**Figure 5:** _A Detailed Overview of the Module Organization in the XMage Project_

#### Server Plugins
The server supports plugins and plenty of them are available by default from the XMage project. The Mage module can load different types of plugins, each of whose interfaces are exposed from this module. The supported types of plugins are listed as shown below in Table 6.

| Plugin Type | Description |
| ------------- | ------------- |
| Player | Allow implementation of player types, examples are the Human player (controller by keyboard/mouse) and the AI (controlled by java code)
| Deck | Allow definition of different deck types, examples are a predefined deck (used in some tests) and a limited deck (ensures a minimum amount of cards) |
| Tournament | Allow the definition of tournament types, comprising of various tournament steps and rules |
| Game mode | Allow the implementation of game types, from simple two player duel's to more complex game modes like Commander free for all |

**Table 6:** _The Supported Types of Plugins in the Server_

The Mage module dynamically loads all available plugins, however this only happens on the server, as the client gets updated by the server it does not need to load the plugins itself. It may be clear that this, lower level, architecture implemented is a plugin-architecture.

The module organization of plugins is shown in Figure 5.

### Validity Assessment
A third interesting part of the system is the validity assessment which happens during the build, verifies all implemented cards and runs all the tests. Cards are extensively validated to ensure that no duplicate or invalid card-names/-numbers/-types etc. exist. This allows the developers to assess that the thousands of cards are in a valid state before releasing a new version of the game.  

### Pattern Usage
Several different design patterns that have been used throughout the XMage project to aid in making the system extendable and maintainable. First and most easy to spot are the huge amount of Singletons [[6]](#bibliography) used for object for which only a single instance is required. For several types a serialization-safe implementation for these has been used as they all are enums instead of classes to prevent singleton duplication after deserialization. A lot of Ability classes throughout the project are singletons too.    
We also see a variety of Factories [[7]](#bibliography) implemented, loading or creating various objects. The most notable ones are the factories that create the respective objects for each type of plugin. For example there is a PlayerFactory which loads the Player type plugins and exposes methods for constructing Player objects based on these Player types. A second example is the GameFactory for loading Game types and exposing methods for constructing Match objects based on these Game types.

## Codeline Organization
In this section, we will look into the code level organization of the XMage project. First, we will present the structure of source code, and then the build will be discussed.

### Source Code Structure
Both files and folders are found in the repository [[3]](#bibliography). In Table 7, the files under the root directory are listed with descriptions.

| File | Description |
| ------ | ------ |
| read.md | A documentation including general information of the project, the installation and developer's instructions |
| pom.xml | Configuration file for the project |
| clean_dbs.sh | A script to clean cards.h2* from Server, Client and Test modules |
| .travis.yml | Configuration file for setting up Travis CI  |
| .gitignore | Configuration file for git describing which files should not be sent to the remote repository |

**Table 7:** _All the Files under the Root Directory_

The repository of XMage is organized based on the modules, as shown in Table 8. Except for the last folder 'Utils', each folder represents a component in the system. This codeline model was chosen based on the fact that the system contains multiple components. It ensures that the project will be developed in a clean and well-organized way. It also provides clear clues for the developers where to check problems or develop a new feature. Some of the source code folders in the modules also contains testing code besides the main code. All the folders are described in the following table.

| Folder | Description |
| ------ | ------ |
| Mage | The logic of the game |
| Mage.Client | The client side with Swing UI, which displays game states and sends player events to the server |
| Mage.Common | The communication of the elements shared between the client and the server |
| Mage.Plugins | A counter plugin to display the number of how many games were played |
| Mage.Server | RMI server which maintains tables and games, sends updates to clients and receives client events |
| Mage.Server.Console | The console of the server |
| Mage.Server.Plugins | All the plugins of the server |
| Mage.Sets | All the card sets and cards |
| Mage.Stats | The stats of the server |
| Mage.Tests | Automatic tests for XMage |
| Mage.Updater | Updating the system based on metadata from remote server |
| Mage.Verify | Asserting correctness of card definitions in _Mage.Sets_ |
| Utils | Perl scripts for developing and updating cards and card sets |

**Table 8:** _All the Folders under the Root Directory_

### The Build Approach
In general, XMage is built on five main modules, including the game logic, the cards and the card sets, the server, the client and the communication between the server and the client [[8]](#bibliography). The system was extended by applying more plugins for the server and adding more modules with different purposes. The build standard for contributing by adding new cards and new sets is described in details on Developer HOWTO Guides [[4]](#bibliography) page. Travis CI, a continuous integration platform, is used in building the whole project. Every time when there is a new commit or a pull request, Travis CI will build it and then deploy to Heroku or update the PR. Travis CI ensures the quality of the source code, and also helps implement the system quickly and detect errors easily.

# Technical Debt Analysis
This section discusses some technical debt analysis from different points of view. We will start high level by looking at the responsibility separation between modules, then go more in depth by analyzing the code metrics we have found from a SonarQube scan. Finally, we will discuss some of the code we have seen in the project during manual analysis.

## Internal Dependencies
In the mage project we found a rather clean separation of responsibilities of modules. Especially the separation of state (Mage.Server), logic (_Mage.Framework_) and view logic (_Mage.Client_) is a nice design for the game. Separating the Client/Server communication into a separate module (_Mage.Common_) and defining the Card sets and Cards into a separate module while depending on the Framework containing the abilities/spells etc. also contributes to a solid design.  
A poorer design choice we have found in the _Mage.Server_ are the dependencies on each plugin. Because of this, the Server cannot compile without these dependencies, analyzing the actual implementation this does not seem like a necessary dependency. Each plugin can dynamically get loaded by the server through a jar file (which can be referenced from a config file) therefore making the dependency redundant.

![Module Diagram](https://github.com/delftswa2018/team-xmage/wiki/images/module-diagram.png)

**Figure 9:** _A Dependency Graph of all Mage Modules_

## Code Metric Analysis
From our SonarQube scan we have analyzed some of the metrics of the project. While the project consists of over a million lines of code as of the latest release (1.4.28) we see that 80% of that belongs to the _Mage.Sets_ module, second being the _Mage.Framework_ making up about 12% of the project in lines of code. The _Mage.Sets_ module is by far the biggest module in the project and this reflects in other size metrics as well like Lines of Code (LOC), amount of Statements [[13]](#bibliography), amount of Functions or Classes. 

| Metric | Total | Mage.Sets | Mage.Client | Mage.Server | Mage.Framework |
| ------ | ----- | --------- | ----------- | ----------- | -------------- |
| LOC | 1,000,000 | 800,000 | 50,000 | 10,000 | 125,000 |
| Statements | 400,000 | 275,000 | 25,000 | 5,000 | 50,000 |
| Functions | 100,000 | 80,000 | 3,000 | 800 | 15,000 |
| Classes | 26,000 | 23,000 | 400 | 80 | 2,500 |
| Code Smells | 14,000 | 7,000 | 2,500 | 300 | 2.500 |
| Cyclomatic Complexity | 150.000 | 100,000 | 10,000 | 2,000 | 25.000 |
| Bugs | 300 | 20 | 180 | 25 | 35 |
| Vulnerabilities | 400 | 30 | 175 | 5 | 50 |
| Duplication | 2.4% | 2.2% | 3.5% | 1.1% | 2.8% | 

**Table 10:** _Various Metrics for the Main Modules of XMage (Rounded for Clarity)_

Although the Mage.Sets module is, as seen in Table 10, the largest module, surprisingly enough it does not contain most bugs and vulnerabilities found by SonarQube. We see high amount of Bugs and Vulnerabilities found in the Mage.Client project, indicating poorer code quality than in the rest of the project. The very low amount of Bugs and Vulnerabilities in other modules indicate a good code quality. Seeing a total of 700 Bugs and Vulnerabilities together on a million lines of code project seems rather good. The project contains low amounts duplication, again given its size this is a good indication. From this analysis we conclude that the overall quality of the code is good, the amount of bugs is reasonable given the size of the project. Regarding improvements we think the Mage.Client module needs the most work, ridding it of the bugs that were found and decreasing the complexity of the overall code. This last point also considers the Mage.Server and Mage.Framework modules, as their complexity with respect to their size in LOC is roughly the same. 

## Code Quality
The static code analysis indicated good overall code quality, but we still found some bad practices in the implementation of XMage. Throughout the code we see large numbers of nested control flow statements, some with depths that make the code near impossible to understand. We consider improving these blocks for better understandability of the code. 

Furthermore, we have noticed a violation of a SOLID principle, namely the Dependency inversion principle. We have depicted a part of the XMage project's class hierarchy in Figure 11. Here we see a good example of the Dependency inversion principle, namely the CardImpl abstract class with children implementing this abstract class. Throughout the code CardImpl is used to depend on any implementation of CardImpl, as the Dependency inversion principle dictates. Now we also see the Token class, which is a concrete class and is used throughout the code for both itself and its children. This results in a dependency on a concrete type which we would rather see implemented like the Cards. The impact of this is that extending the Token class may cause confusion for developers, causing misusage of the Token class or not using references to the correct token class.

Our suggestion to fix this is to make Token an abstract class and rename it to TokenImpl in order match the naming conventions in the XMage project. Then replacing any concrete usages of Token by a suitable implementation of TokenImpl, while leaving all dependencies on TokenImpl. This would result in the same situation that is present for the CardImpl. Besides resulting in fixing the SOLID violation this will also yield higher consistency in the project.

![Mage Object Hierarchy](https://github.com/delftswa2018/team-xmage/wiki/images/mage-object-hierarchy.png)

**Figure 11:** _A Part of the Mage Class Hierarchy_

## Testing Debt
This section discusses the testing debt of the XMage project. This includes the test structure, the test coverage, how manually testing works and the test procedures of the XMage project.

### The Test Structure
The XMage project exists out of many modules. Some of these modules are only made for testing the project like the _Mage.Tests_ and the _Mage.Verify_ modules. The _Mage.Tests_ module consists of tests which cover multiple parts of the project like the server, client and AI but most of all the Mage Framework. These tests are there to make sure that the current functionality of the program does not change by any code changes. The Mage Verify module is more focused on testing the implementation of new cards. It checks all kinds of conditions like whether there are no duplicate cards or whether the card class names are correctly written.

### Testing Procedures
In the XMage project it is not required to add tests for each new implemented card. However, if you make any changes in the _Mage.Framework_ module it is required to properly test your code. Especially if you make any additions it is required to add the necessary tests to make sure your implementation doesn't break anything. This is why the Mage Framework is tested mostly by unit tests, where all the cards are mostly tested manually. When you do add a test, it is important that you add your test in the _Mage.Tests_ module, that it is a JUnit test and that it properly builds in the Maven project.

## Discussions about Technical Debt

In this section, we dive into the XMage Github to search for the discussion between the developers on the technical debt. First, the open issues are analyzed in terms of technical debt, and then we discuss the search results of TODO's [[16]](#bibliography) and FIXME's [[17]](#bibliography).

The developers of the XMage project did discuss some of their technical debt in Github issues [[14]](#bibliography). Most of the issues related to the technical debt were raised 2 to 3 years ago, which are about the programming logic between some objects, the game logic, the cards management and the source code organization. However, some of developers who discovered the issues or joined the discussions are no longer active in the project, and the others decided to keep the current solutions. Recently, they have discussed about not implementing the illegal cards to prevent technical debt, since the illegal cards ask for the abilities that the system does not support [[15]](#bibliography). 

Based on the fact that there are numerous outdated TODO's and FIXME's, it is highly suggested that an issue should be raised to discuss them. For example, for the ones which are still valid a schedule should be arranged and related developers should be assigned to solve them, the ones which do not affect the system anymore should be removed. Additionally, another issue should be created to schedule all the recently-added TODO's and FIXME's.

## Evolution of Technical Debt

This section discusses the evolution of the technical debt in the system. First, the code base will be discussed, then the use of TODOs and finally the impact of the technical debt will be shortly described.

### Code Base
The system has evolved a lot over the past 8 years. At the start of the project, there were around 30000 lines of code. At the end of 2017, there were over a million lines of code in the system. It appears the code base has increased very consistently over the years. The SonarQube analysis shows that in the past year the number of lines of code, files and classes coincide with the increase in technical debt.

### TODO Analysis

The number of TODOs increase steadily over time. As can be seen in Figure 12, the growth in the number of TODO's coincides with the growth in lines of code.

![TODO vs LOC](https://github.com/delftswa2018/team-xmage/wiki/images/todo-vs-loc.png)

**Figure 12:** _A Graph Showing TODOs versus Lines of Code over Time_

It may seem like the number of TODOs increase as more code is written, and the TODO/lines-of-code has been decreasing over time. Further analysis shows however, that most of the time, TODOs are actually ignored. As can be seen in Figure 13, there are TODOs that have existed for over 7 years.

![TODO snippet](https://github.com/delftswa2018/team-xmage/wiki/images/todo-age.png)

**Figure 13:** _A TODO in a piece of code, which was created 7 years ago._

Looking at the TODO message, it appears no one ever looked back at this piece of code. A large number of very old (5 years or older) TODOs are found in the classes related to Artificial Intelligence (AI). It appears the difficulty of making the AI better was too high, and efforts to improve it were abandoned.

### Impact of Technical Debt over Time

As can be seen in Figure 14, the technical debt appears to have an impact on the number of bugs in the system.

![Technical debt vs bugs](https://github.com/delftswa2018/team-xmage/wiki/images/Debt-vs-bugs.png)

**Figure 14:** _A Graph Showing the Technical Debt versus the Number of Bugs_

As the technical debt increases, so does the number of bugs find in the system by the SonarQube analysis.

# Deployment View

This section discusses the hardware, networking and third-party requirements. Even though there are not that many requirements to run XMage, there are still some important requirements to meet. The overview of deployment view in the XMage project is shown in Figure15.

![Deployment View Diagram](https://github.com/delftswa2018/team-xmage/wiki/images/Deployment%20View.png "Deployment view")  

**Figure 15:** _The Overview of Deployment View in the XMage Project_

## Third-party Software Requirements

Both the client and server require Java 1.8.x to be installed.

## System Requirements

As both client and server run on Java, XMage can run on many operating systems. XMage has been tested to work on Windows, Linux and Mac.

### Client

The client requires about 200MB of disk space to run. Optionally, card images can be downloaded, requiring 10GB of disk space. 512MB of memory is required to run the client.

### Server

The popularity of the server determines the memory requirements. For the most popular server from a few years ago [[9]](#bibliography), this was about 3GB of memory for the server, resulting in about 1GB per 100 users, excluding operating system memory usage. The server will require about 150MB of disk space to run. The CPU usage is not very high, so any CPU should be enough to serve a large number of players.

## Server Networking Requirements

Networking may be difficult. There are a few things that have to be considered.

### Network Setup

The server has to be accessible by the intended users. For public servers, this means allowing access to ports 17171 and 17172. If the server is behind a NAT router, these ports will have to be forwarded to the server.

### Network Capacity
The average upload speed required is about 10 Mbit/s for 250 users, while the download speed is about 10% of the required upload speed, as can be seen in Figure 16.

![Server Traffic Diagram](https://github.com/delftswa2018/team-xmage/wiki/images/ServerTraffic.jpg "Server traffic")  

**Figure 16:** _The Bandwidth of Network in the XMage Project_

## Runtime
Java has to be downloaded to run the launcher, but the launcher will then download a specific version of Java to ensure all users run the game using the same version.

# Regulation Perspective 
The XMage project is mainly affected by the copyright surrounding the Magic The Gathering game. In order to circumvent the legal issues coming with adding the images and packing them with the game, they were left out of the XMage project. Instead a download feature was implemented from which the images could be downloaded from an external source, leaving XMage out of the way of any potential copyright infringement.
Solutions less pressing on the players of the game were suggested and discussed but like all discussions [[10]](#bibliography) they came down to the copyright infringement problem.

## Open Source License
When going through the project a lot of the files contain the: _"Copyright 2011 BetaSteward_at_googlemail.com. All rights reserved."_ license in the comments, where also a lot of the files have no license reference. _BetaSteward_ has not worked on the project for years, where the current active team of the XMage project does not deal with any licenses. In our perspective a license, even for open-source projects, is an important part of a project. A license should be added to the project in order to grant new collaborates the permission to use and change and improve the code any way they want, without anyone being able to claim ownership nor make any author liable for any damage his work may have done. Setting up an license is free and should not event take that much time.

# Usability Perspective
In this section explains how the users interact with the system and which user interacts with which part of the system. Then we go into the depth of the different kind of user capabilities.

## Users and Capabilities
We identify two main types of users in the XMage program, the players and the server administrators. The players use the game through the game's Graphical User Interface (GUI), its interaction we will describe in a followup section. From this we can already see that there are no strong technical capabilities required of the players, since their entire interaction with the game happens through a GUI. It is however expected that they know the MTG game, as there is no explanation regarding the rules of the card game embedded in the computer game.
The server administrator have to do some more low level interactions [[12]](#bibliography) like manually editing xml config files, setting up cron jobs [[11]](#bibliography) and creating script files. They can manage which games are going on on the server and which players are present via a special console. It may be clear that there are more significant technical capabilities required of the server administrators.

![Mage UI](https://github.com/delftswa2018/team-xmage/wiki/images/XMage_UI_1.jpg "Mage UI")  

**Figure 17** _The main menu of XMage, with the image download menu opened._

## Server Administrator Interaction
The server administrator first has to set up the server, this is done by following the procedure described in [[12]](#bibliography). In this procedure the server will be configured by various options and the available plugins can also be defined. The logger is configured and cron jobs are defined to regularly restart the server. So far all interaction happens via terminals and file editors. Once the server is running there is of course the option to shut it down/restart it via the terminal, but besides that there is only one way to interact with the running server instance, via the Mage Server Console. Via this console an Admin can see which players are connected to the server, which games are currently being played and by whom. Players can be send messages to, they can also be disconnected or banned via this window.

## Player Interaction
In order to make use of the XMage project the user has to start up the XMage client to interact with the user interface. If the user wants to participate in magic the gathering games he has to join a XMage server. At the moment XMage provides several servers which any user can join after creating an account. When the user has logged into the server he/she is able to start a MTG game or tournament and the user will also be able to configure any relevant properties. From this screen the user is also able to chat with the other users on the server.  
The user interface also provides several other options like the preference menu where the user can change all kind of properties of the interface of the XMage client. The user can also interact with the system by creating or modifying his/her MTG decks by clicking on the deck editor tab and in order to view the actual cards themselves the user can click on the Viewer tab. Because of [regulation issues](#regulation-perspective) the client does not provide the image and symbol resources from the start, however the user can download these resources themselves by using the symbols and images tabs in the user interface.  
These are the most common ways the default user interacts with the system in order to make use of the XMage project.

# Conclusion
XMage is an open-source project which has been worked on for the past 8 years. During these 8 years it has grown from a project with 30000 lines of code to a project with over a million lines of code. Because the project has had many contributors over the years, the code base has grown into a complex structure with many modules and dependencies. 
In this chapter, we have analyzed the architecture of the XMage project by presenting the _Server-Client_ structure and explain about the many plugins it uses. We analyzed the test structure of the code including the test module and the validity assessment. We analyzed the many stakeholders the project consists of and discussed the technical debt of the project including the testing debt, code quality and code metrics.
XMage has proven to be a strong competitor in the Magic the gathering scene. It has become a very interesting free alternative for online magic the gathering and has lots of potential to keep on growing. We do recommend adding an open-source license to the project and improve their merging strategy to make XMage more suitable for the future. 

# Bibliography
[1] Rozanski, N. Woods, E. Software Systems Architecture: Working with Stakeholders Using Viewpoints and Perspectives. Addison-Wesley, 2012. [https://www.viewpoints-and-perspectives.info](https://www.viewpoints-and-perspectives.info).

[2] MTG Card Maker. [https://www.mtgcardmaker.com/](https://www.mtgcardmaker.com/).

[3] The XMage project. Magic Another Game Engine. [https://github.com/magefree/mage](https://github.com/magefree/mage).

[4] The XMage project. Developer HOWTO Guides. [https://github.com/magefree/mage/wiki/Developer-HOWTO-Guides](https://github.com/magefree/mage/wiki/Developer-HOWTO-Guides)

[5] XMage. [http://xmage.de](http://xmage.de)

[6] McDonough, J. E. (2017). Singleton Design Pattern. In Object-Oriented Design with ABAP (pp. 137-145). Apress, Berkeley, CA. 

[7] Hannemann, J., & Kiczales, G. (2002, November). Design pattern implementation in Java and AspectJ. In ACM Sigplan Notices (Vol. 37, No. 11, pp. 161-173). ACM. 

[8] The XMage project. Developer Notes. [https://github.com/magefree/mage/wiki/Developer-Notes](https://github.com/magefree/mage/wiki/Developer-Notes).

[9] The XMage Project. Server load / user disconnects - what does cause the problems #662. [https://github.com/magefree/mage/issues/662#issuecomment-68996043](https://github.com/magefree/mage/issues/662#issuecomment-68996043)

[10] The XMage Project. Card images not bundled? #2698. [https://github.com/magefree/mage/issues/2698](https://github.com/magefree/mage/issues/2698)

[11] The Open Group Base Specifications Issue 7, 2018 edition. [http://pubs.opengroup.org/onlinepubs/9699919799/utilities/crontab.html](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/crontab.html)

[12] Collectible Card Games Headquarters. How to set up a (public) XMage server. [https://www.slightlymagic.net/forum/viewtopic.php?f=70&t=15898](https://www.slightlymagic.net/forum/viewtopic.php?f=70&t=15898) 

[13] SonarQube. Metrics - Statements. [https://docs.sonarqube.org/display/SONAR/Metrics+-+Statements](https://docs.sonarqube.org/display/SONAR/Metrics+-+Statements)

[14] The XMage Project. Issues. [https://github.com/magefree/mage/issues](https://github.com/magefree/mage/issues)

[15] The XMage Project. Issues. UST - Card Implementation Tracker (Tracking issue for Unstable) #4233 [https://github.com/magefree/mage/issues/4233](https://github.com/magefree/mage/issues/4233)

[16] The XMage Project. Search: todo. [https://github.com/magefree/mage/search?p=14&q=todo&type=Code&utf8=%E2%9C%93](https://github.com/magefree/mage/search?p=14&q=todo&type=Code&utf8=%E2%9C%93)

[17] The XMage Project. Search: fixme. [https://github.com/magefree/mage/search?utf8=%E2%9C%93&q=fixme&type=Code](https://github.com/magefree/mage/search?utf8=%E2%9C%93&q=fixme&type=Code)
