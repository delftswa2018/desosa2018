# Contributions

This chapter outlines some of the contributions made by several teams to their open source project.


## Akka

- Merged pull request [24655](https://github.com/akka/akka/pull/24655): Add log() method to typed Logging API, fixing issue [24648](https://github.com/akka/akka/issues/24648).

- Merged pull request [24850](https://github.com/akka/akka/pull/24850): Add tests for BoundedBlockingQueue.

- Merged pull request [25025](https://github.com/akka/akka/pull/25025): Fix BoundedBlockingQueueSpec against spurious wakeups fixingfailing test in [24991](https://github.com/akka/akka/issues/24991)


## Angular

### docs: correct grammar mistakes in CONTRIBUTING.md

![Contribution-status](https://img.shields.io/badge/contribution--status-merged-green.png)

 * Authors: [Derk Snijders](https://github.com/i3anaan)
 * [Link to PR](https://github.com/angular/angular/pull/22285)

While looking at the `CONTRIBUTING.md` file on how to make contributions we have found numerous mistakes. We have created this pull request which was merged quite fast.

### build: fall back on default version number if there the .git directory is not found

![Contribution-status](https://img.shields.io/badge/contribution--status-discussion-yellow.png)

 * Authors: [Derk Snijders](https://github.com/i3anaan)
 * [Link to the issue](https://github.com/angular/angular/issues/22551)
 * [Link to PR](https://github.com/angular/angular/pull/22552)

While getting ourselves acquainted with the angular codebase and trying to build it locally we have noticed that it does not build when running angular as a submodule in git.
Therefore, we have created an issue and later a PR which we first merged into our fork of Angular by review of several team members and then created this pull request to upstream.
It seems that the Angular team is moving away form this build script and will use BAZEL in the future.
The pull request is currently open.


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



## Eden

- Merged pull request [1480](https://github.com/sahana/eden/pull/1480): Update Arabic and Korean translations.



## ElasticSearch

### Test coverage generation

While looking through the testing documentation, we noticed that the section on generating test coverage with JaCoCo still used Maven commands to generate test coverage. However, seeing as Elasticsearch switched from Maven to Gradle in October 2015 as described in [this issue](https://github.com/elastic/elasticsearch/issues/13930), we thought a good contribution would be to update this documentation. Having posted [an issue about this](https://github.com/elastic/elasticsearch/issues/28867) on the Elasticsearch GitHub, we got feedback that JaCoCo was not included in the build configuration at the moment, and they asked us if we could add it in there. We tried to get it working, but seeing as we have no prior experience with the Gradle build system and seeing as the tests take twenty minutes to run (and only succeed on a small subset of our machines), we decided to abandon this contribution for our first contribution.

However, we do still think it is important for Elasticsearch to get JaCoCo or Cobertura integration. So for the third deliverable, we picked up where we left off and tried again. After some hacking around, we found out what the cause of the problem is. It [turns out](https://github.com/gradle/gradle/blob/master/subprojects/jacoco/src/main/java/org/gradle/testing/jacoco/plugins/JacocoPlugin.java#L149) that the JaCoCo plugin for Gradle looks for all tasks with type `org.gradle.api.tasks.testing.Test`. However, the plugin that enables randomized testing replaces the regular testing tasks with Elasticsearch's own [`RandomizedTestingTask`](https://github.com/elastic/elasticsearch/blob/master/buildSrc/src/main/groovy/com/carrotsearch/gradle/junit4/RandomizedTestingTask.groovy)s, which do not inherit from the aforementioned `Test` type. JaCoCo is therefore not able to recognize these tasks as testing tasks and add the JaCoCo extension to them. We believe the Cobertura plugin works in the same way.

`RandomizedTestingTask` and `Test` do share a common ancestor, namely Gradle's `DefaultTask`. Therefore, we tried to simply change `RandomizedTestingTask` to extend from `Test` instead of from `DefaultTask`. Initially, this generated some Groovy compilation errors, which were easily resolved. However, upon executing `./gradlew test jacocoTestReport`, we ran into a more severe problem at runtime. In response, we posted another comment on the aforementioned issue, stating the above and asking whether the Elastic developers have any idea how we could fix the issue. Ryan Ernst [@rjernst](https://github.com/rjernst), who was also the author of the original Maven-to-Gradle issue, replied that he originally wanted to make `RandomizedTestingTask` extend `Test`, but that he ran into a number of issues while doing so, after which he suggests a workaround for the issue, namely to call JaCoCo directly from Ant in the `PrecommitTasks`.

For the final deliverable, we aimed to have resolved the JaCoCo issue, as well as to have `RandomizedTestingTask` extend `Test` to reduce the technical debt in their build system. However, after more analysis and weeding through errors, we found that a full refactor of `RandomizedTestingTask` is most likely necessary in order to have it extend `Test`. We simply do not have the time for that, nor do we have enough knowledge of the Elasticsearch build system to properly implement this. As for the offered alternative, this would entail running the tests during the pre-commit tasks, which is specifically reserved for _non-test related_ tasks. And even then, we highly doubt whether we will not run into the same problem with `RandomizedTestingTask`.

Therefore, we decided against solving this issue. However, we did decide to update the outdated documentation with information on the current state of generating test coverage, for which we soon had [a pull request](https://github.com/elastic/elasticsearch/pull/29255). While we had pinged three Elastic members to review the pull request, a different Elastic member, Nik Everett [@nik9000](https://github.com/nik9000), replied 23 hours after submitting this pull request that it looked good and that he merged it.

### Adding a usage example of the JLH score

Instead of the test coverage generation, we decided on updating a documentation issue of Elasticsearch. We found [this issue](https://github.com/elastic/elasticsearch/issues/28513), which states that there is no example of the JLH scoring mechanism in the documentation. In response to this issue, we created a [pull request](https://github.com/elastic/elasticsearch/pull/28905) that adds the example. After a day and a bit we received an approve from Luca Cavanna [@javanna](http://github.com/javanna), one of the integrators we identified in the first deliverable, and the pull request was also merged directly. This means we have successfully contributed to Elasticsearch!

### Resolving some technical debt from SonarQube analysis

For deliverable 3 we performed a SonarQube analysis of Elasticsearch, which measured 280 working days of technical debt in Elasticsearch. In response to this figure, we created [an issue](https://github.com/elastic/elasticsearch/issues/29081) on the Elasticsearch GitHub to ask the developers whether there is interest in fixes of code smells and if there are any specific issues in this category that they would like resolved. Half a day later, an Elasticsearch developer replied, to our surprise:

> ... In general we'd be happy to fix whatever might end up being a problem to users. ...

This response shows us that the Elasticsearch developers are more focused on keeping their users happy, rather than keeping their developers happy by keeping the code maintainable.

In the final week, we decided to solve one of the issues indicated by SonarQube. This issue flagged various `equals` methods that either had no type check, or had a faulty comparison where a field `foo` would be compared against `foo` of the same instance, instead of to `other.foo` as would be expected. 

We created [a pull request](https://github.com/elastic/elasticsearch/pull/29348), addressing this issue. We soon got a positive response from Alan Woodward [@romseygeek](https://github.com/romseygeek), indicating that some of the fixed issues did indeed look like bugs. He asked us if we could also add some tests to prevent future bugs appearing in these methods. We did add tests for most equals methods that we touched, except for two. The reason for this can be found in the conversation of the pull request.

After a minor CheckStyle fix indicated by the continuous integration, both Alan and the continous integration were happy with the changes, and the pull request was merged on Friday the 6th of April.



## Electron

### Communication with the Stakeholders

For the understanding of architecture of Electron and contribute towards it, we made use of all channels that were possible as described below:

- PRs and Issues on Github repos as explained below
- *Slack*: We used Slack channel of Electron to interact with our initial contacts ([Samuel Attard](https://github.com/marshallofsound) and [deepak1556](https://github.com/deepak1556)) final points of contacts ([zeke](https://github.com/zeke) and [ckerr](https://github.com/ckerr)). Zeke has been very helpful towards us by replying to most of our queries and even doing a video call to solve queries about the contribution to be made. 
- *Discuss Forums*: We wanted to know, what the community using Electron thinks of the technical debt, development view and the stakeholder analysis. We got lots of views on the posts created but unfortunately no replies. We believe that the community using electron is interested in solving their development problems rather than to know more about Electron's internals. Links: [Post1](https://discuss.atom.io/t/studying-architecture-of-electron-1-development-view-and-technical-debt/53884), [Post2](https://discuss.atom.io/t/studying-architecture-of-electron-2-stakeholders-involved/53885)
- Shivam also joined the *Electron's HQ Maintainer Group* as he was invited by Zeke. On approval, Shivam can now directly create branches in Electron's repository without forks.

### Accepted Pull Requests

#### 1. [PR #1170 - Updated Contributing.md for electronjs.org](https://github.com/electron/electronjs.org/pull/1170)

**Issue**: There was no existing issue regarding this PR. Shivam directly sent the PR as it was a simple contribution.

**Repository**: https://github.com/electron/electronjs.org/

**Status**: 

Created at|Person| Status|Comment
--|--|----|---------
March 3|Shivam Miglani|Created PR |Describing PR
March 3|[ckerr](https://github.com/ckerr)|Reviewed and approved|Thanks for the improvements! Don't worry about feeling that the PR is not substantial. Everyone starts somewhere and your fixes are much appreciated! 
March 3|[zeke](https://github.com/zeke)|Merged after AccessLint,WIP, and travis-ci/pr checks passed| Great work, @Shivam-Miglani. Thanks!

**Description**: [Shivam-Miglani](https://github.com/Shivam-Miglani) was just going through contributing.md for the issue [#1156](https://github.com/electron/electronjs.org/issues/1156) and in the process fixed some broken links and spelling mistakes. The main developers [zeke](https://github.com/zeke) and [ckerr](https://github.com/ckerr) appreciated the effort. This illustrates that Electron has very active developer community and they abide by their [code of conduct](https://github.com/electron/electronjs.org/blob/master/CODE_OF_CONDUCT.md)(which doesn't discriminate among contributors), making them a very open and welcoming community.
Specifically,
- fixed links (Handlebars, context-builder.js and editing content)
- fixed some spell errors.


#### 2. [PR #295 -  feat: parse tutorial sidebar nav content](https://github.com/electron/electronjs.org/issues/1156)

**Issue**: [#1156 - Add a sidebar nav to tutorial pages- electronjs.org/docs/tutorial](https://github.com/electron/electronjs.org/issues/1156)

**Repository**: https://github.com/electron/electronjs.org/

**Description**:
This PR relates to the first step of [Issue#1156](https://github.com/electron/electronjs.org/issues/1156), in which we have to extract the localized Table of Contents lists from `/content/**/docs/README.md` files and export them as part of i18n module.

What has been done:
- `parse-nav.js`: extracts localized toc's lists and exports them as `i18n.navs`, not to be confused with `i18n.website.locale.nav`.
- **Unit tests** for the same using[Mocha](https://mochajs.org/) and [Chai](http://www.chaijs.com/) frameworks:
  - UT1. tests that keys of i18n.navs are locales and ensures that number of languages is greater than 10
  - UT2. tests that every key has a value and that the value is HTML content by looking for `<ul>` and `<li>` in every key's value. Only `<ul>` won't work as outer one was added manually.
  - UT3. tests that sidebar nav's content is picked up and not any other list from README's by checking the existence of 'Setting up the Development Environment'.
- `readme.md` and `stats.json` were updated after running npm test.

**Discussion on Issue Status**: 

|Created at|Person| Status|Comment|
|--|--|----|---------|
|Feb 28|[zeke](https://github.com/zeke)|Created Issue [#1156 - Add a Sidebar nav to tutorial pages](https://github.com/electron/electronjs.org/issues/1156)  |Describing issue: "This is a followup to electron/electron#11416. @felixrieseberg wrote a bunch of tutorial content and created a nice table of contents in electron/electron#11966. Now that we have that, let's display it in a sidebar on tutorial pages, so people can navigate their way through the tutorial content more easily."|
|Mar 1|Sharad|Showed interest|@zeke , I would like to work on the sidebar nav!|
|March 1|[zeke](https://github.com/zeke)|Agreed and described the first step|@sharadshriram cool! I think a first step would be updating https://github.com/electron/electron-i18n to extract the locaized TOC lists from /content/**/docs/README.md and export them as part of the electron-i18n module. If that sounds daunting, let me know and I can help give more guidance.|
|March 3|Shivam|Takes over Sharad's work|@zeke  I'm helping @sharadshriram in this. We are from the same uni and can collaborate. ~~We get that we need to extract TOC's of all languages from ``/content/**/docs/README.md`` but we don't get the part 'export them as part of ``electron-i18n`` module'.~~ **Update**: This is what I understand until now: Write a async function in  https://github.com/electron/i18n/blob/master/script/collect.js to extract TOC lists from ``/content/**/docs/README.md``. The code would look something like this:*(`Code snippet 1`). @zeke Let me know if I'm thinking in the right direction.|
|March 3|[zeke](https://github.com/zeke)|Feedback on code snippet - provides his own snippet but warns that not entirely correct|That's close, though the collect script is more for pulling actual content from various sources. I would recommend writing a standalone script that is required and used from the build-module script. It should do something like:*(`Code snippet 2`)|
|March 4-8|Shivam|Trying code and posted his queries|@zeke *newbie node.js doubt* How do I run and test this code? I created ``script/get-toc.js``, tried to call it in build-module.js but then it can't find the function. Then, I thought okay let me just try to run this standalone, so I did ``const i18n = require('..')``, to do ``i18n.docs[locale]['docs/README.md']``, then it says ``Reference error: i18n is not defined``. I tried running ``test/index.js``, says ``Error: Cannot find module '..'`` at similar line ``const i18n = require('..')``. Checked environment paths, deleted node_modules and reinstalled them by now.|
|March 9|[zeke](https://github.com/zeke)|Is not available|I'm offline for a bit, but will help you move this forward when I'm back. Thanks for being patient!|
|April 2| [zeke](https://github.com/zeke)| Sets up a **video call** to explain doubts| @Shivam-Miglani do you have any time this week to join a video call? We can work on this together if you like.|
|April 3| Shivam|Video call with Zeke| Zeke was very helpful and cleared all the doubts and troubles I was having in understanding [i18n](https://github.com/electron/i18n/pull/295)|
|April 3| Zeke|Update from Zeke|For the record, @Shivam-Miglani and I had a call today. Shivam is going to work on opening a PR to i18n that adds parsed navs for every language.|

**PR Status**:

|Created at|Person| Status|Comment|
|--|--|----|---------|
|April 4|Shivam|Created PR #295 in [i18n](https://github.com/electron/i18n/pull/295)| Described PR. *The code for parsing localized READMEs for content of navigation and unit tests is described in `Code snippet 3 and 4`]|
|April 4|[zeke](https://github.com/zeke)|Requested Changes|Can you back out the changes to the README and stats.json? Those aren't related to this PR, and are just a byproduct of running the build. Otherwise, this is looking good!|
|April 4|Shivam|Performed requested changes|reverted the commit and added again the unit tests excluding `readme.md` and `stats.json`|
|April 5|[zeke](https://github.com/zeke)|Approved and merged changes|All CI checks (semantic pull request, WIP and travis-ci/pr passed)|


**Code Snippet 1: Shivam's initial guess**
```Javascript
//access the directory
const basePath = path.join(__dirname, '..', 'content')
const contentPath = (lang) => path.join(basePath, lang, 'docs')
langs.forEach((lang) => {
    //call the function getNavbarContent(lang)
}

async function getNavbarContent (lang) {
      //extract data from Readme's
      //somehow add these localized TOCs
      // to i18n object (not sure how yet - as a separate attribute within docs perhaps)
}
//Later export it as i18n object in the electronjs.org website:
const i18n = require('electron-i18n')
```
**Code Snippet 2: Zeke's response with warning that not entirely correct**
```Javascript
const cheerio = require('cheerio')

function getTableOfContents(locale)
  const doc = docs[locale]['docs/README.md']
  const html = doc.sections.map(section => section.html).join('\n')
  const $ = cheerio.load(html)
  const heading = $('a[href="#guides-and-tutorials"]').parent() // h2
  const toc = heading.next('ul').html()
}
```


**[Code Snippet 3: parse-nav.js - code written by Shivam](https://github.com/delftswa2018/i18n/blob/354a84ec58457e3f5c882d38dd1e633c46b0914c/script/parse-nav.js)**: Comments were added later for readability purposes of the course. Original code doesn't contain comments.
```Javascript
//cheerio is jquery like html parser
const cheerio = require('cheerio')

//path and fs are used for writing the file
const path = require('path')
const fs = require('fs')

//i18n is the whole documentation and translations object for electron
const i18n = require('..')
const locales = Object.keys(i18n.locales)

//function to get navigation bar content from localized README
function getNav (locale) {
  const docs = i18n.docs[locale]
  const readme = docs['/docs/README']
  const html = readme.sections.map(section => section.html).join('\n')
  const $ = cheerio.load(html)
  //second heading is what we wanted
  const startHeading = $('h2')[1]
  //until next unordered list is what we wanted
  const listItems = $(startHeading).next('ul').html()
  const nav = `<ul>${listItems}</ul>`
  return nav
}

//make a dictionary object for parsed navs for each locale
const navsByLocale = locales
  .reduce((acc, locale) => {
    acc[locale] = getNav(locale)
    return acc
  }, {})

//set navs property for i18n object
i18n.navs = navsByLocale

//write to index.json file which represents i18n object
fs.writeFileSync(
  path.join(__dirname, '../index.json'),
  JSON.stringify(i18n, null, 2)
)
```
**[Code Snippet 4: index.js - unit tests written by Shivam using Mocha and Chai frameworks](https://github.com/delftswa2018/i18n/blob/354a84ec58457e3f5c882d38dd1e633c46b0914c/test/index.js)**
```Javascript
//unit test 1
describe('i18n.navs', () => {
  it('is an object with locales as keys', () => {
    i18n.navs.should.be.an('object')
    const keys = Object.keys(i18n.navs)
    keys.should.include('en-US')
    keys.should.include('fr-FR')
    keys.length.should.be.above(10)
  })

//unit test 2
  it('has a value and has valid html content as values', () => {
    const values = Object.values(i18n.navs)
    values.every(value => value.should.be.a('string'))
    values.every(value => value.should.contain('<ul>'))
    values.every(value => value.should.contain('<li>'))
    values.should.not.contain('<html>')
    values.should.not.contain('</html>')
    values.should.not.contain('<head>')
    values.should.not.contain('</head>')
    values.should.not.contain('<body>')
    values.should.not.contain('</body>')
  })

//unit test 3
  it('has the required sidebar nav content for tutorials', () => {
    const value = i18n.navs['en-US']
    value.should.contain('<a href="/docs/tutorial/development-environment"')
    value.should.contain('Setting up the Development Environment')
  })
})
```

#### 3. Translation for Hindi Documentation using crowdin
Electron uses [crowdin](https://crowdin.com/project/electron) for incorporating translations of its documentation into [i18n](https://github.com/electron/electron-i18n ). Sharad and Shivam made some contributions for translations of few tutorial pages for the language "Hindi".

![](images/c3.png)

### Work in Progress
- [#1156 - Add a sidebar nav to tutorial pages- electronjs.org/docs/tutorial](https://github.com/electron/electronjs.org/issues/1156): Waiting for further instructions from Zeke for second part of this issue in which we will add the required HTML from i18n object to the website electronjs.org/docs/tutorial. This requires knowledge of express.js

### What we learnt about the architecture from contributions:
By forming the communication channels to a friendly and open community like Electron, we were able to make complex coding contributions even though we didn't have any prior knowledge about the environment. This helped us to know about the various **architectural aspects** such as:
- Electron's core (electron/electron) is the main part of the framework. However many other parts exist, such as `electron/i18n` for language translations and documentations and `electron/electronjs.org` as the website.
- All these frameworks are directly or indirectly influenced by the core of Electron.
- The TD is low in Electron as they have about 500 apps depending relying on their stability. Some, TD has been introduced by introduction of newer version. Zeke validated our findings on Technical debt and encouraged us to learn more about Electron and work towards solving even complex contributions.
- The long PR we made was pretty helpful in understanding why unit tests were so important and why everything is segregated into 46 repositeries that Electron project has. We also came to know how Electron cleverly keeps all of its documentation in a repo called i18n which allows it's to reduce TD regarding translating stuff by continuous integration of translations put into the crowdin site.
- Open source projects like Electron have strict code of conduct which is followed by all developers making it a very nice and friendly community even for newbie contributors like us.



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


## Kubernetes

Before starting to make any contribution to the Kubernetes project, it is necessary to read the main [contributor documentation](https://github.com/kubernetes/community/tree/master/contributors/guide) and agree to its terms. After this, checking for open issues along the [Kubernetes' repositories](https://github.com/kubernetes) is a good way to start; and that's how we actually began. Below you can find the contribution progress made by us, Team-Kubernetes, to the [Kubernetes project](http://kubernetes.io).

1. **Contribution to Issue** [\#1613](https://github.com/delftswa2018/community/commit/47b06d8fdad666653db5fdc02c1c496c41168c95), "SIG-cli CONTRIBUTING.md should point at contributor guide".  This issue is related to Kubernetes documentation and belongs to the [SIG-CLI Community repository](https://github.com/kubernetes/community/tree/master/sig-cli). The [CONTRIBUTING.md](https://github.com/kubernetes/community/blob/master/sig-cli/CONTRIBUTING.md) file needs some updates, duplicated information removal, and consistency check.  All these fixes have been completed in [our forked repository](https://github.com/delftswa2018/community/blob/%231613-nidorano-sig-cli-contribution-ptr/sig-cli/CONTRIBUTING.md). We have also checked there is not a WIP (Work In Progress) regarding this specific issue; hence, we created a PR asking merge these fixes. This PR has been reviewed by the k8s community and approved by [idvoretskyi](https://github.com/idvoretskyi).

2. **Updating outdated URL in a readme file**, we have found out while exploring the contribution guide, that a URL in the [contributor cheatsheet](https://github.com/kubernetes/community/pull/1883/files#diff-44d578a3dd781a55fe797ae8cea800ea) to the bot commands page was outdated and still referred to the [old page](https://git.k8s.io/test-infra/commands.md). This PR proposes a slight change so that the URL points directly to the [new page](https://prow.k8s.io/command-help), thus less clicks and frustration for future readers. This pull request has been reviewed and approved by [cblecker](https://github.com/cblecker) and has been merged into the [Kubernetes Community repository](https://github.com/kubernetes/community).

### Contributions Summary

| k8s Issue | k8s PR                                        | Title | Progress | Status |
| ----------|-----------|--------|--------|-------|
| [kubernetes/ community\#1613](https://github.com/delftswa2018/community/commit/47b06d8fdad666653db5fdc02c1c496c41168c95) | [kubernetes/ community\#1887](https://github.com/kubernetes/community/pull/1887) | SIG-cli CONTRIBUTING.md should point at contributor guide | - Identified [repository](https://github.com/kubernetes/community) to contribute to <p>- Forked [repository](https://github.com/delftswa2018/community) to work on <p>- Addition of pointers to main Kubernetes contribution guide <p>- Removed duplicated information about CLA <p>- Checked for consistency | Approved and Merged |
| None | [kubernetes/ community\#1883](https://github.com/kubernetes/community/pull/1883) | Correcting an outdated URL in contributor cheatsheet | Approved and merged | Approved and Merged |



## Lighthouse

### [Fix broken link](https://github.com/GoogleChrome/lighthouse/commit/bfb9bb513f25a12cf67303c9a193e78dda2030fd)

While only a minor contribution it is a valuable one nonetheless.
It simply fixes a broken link.
This contribution is probably more valuable to us then to the Lighthouse team because for us it was a good oppertunity to familiarize ourselves with contributing to the project without being able to cause too much damage.

### [Update documentation](https://github.com/GoogleChrome/lighthouse/pull/4680)

Because Lighthouse is a large scale open source project it can happen that documentation becomes out of date.
Changes will be made to the project and the corresponding documentation will not be updated because of time constraints or because people simply forgot.
In this case there was a small error in the `README.md` which still referenced an old feature that was removed from Lighthouse.
We filled an appropriate pull request to solve the issue.

### [Export report to CSV](https://github.com/GoogleChrome/lighthouse/pull/4912) 

Our main contribution allows developers to export the Lighthouse results to an CSV file.
Previously it was only possible to export the results to `JSON` or to `HTML`.
With our feature it is possible to also export the results in the `CSV` format.
Each row (line) of the `CSV` file contains the information of 1 audit.
It describes:
- The name of the category the audit belongs to
- The name of the audit
- A description of the audit
- What score type is used to grade the audit
- The score that the website earned for the audit



## LoopBack

### Issue #925
[Issue #925](https://github.com/strongloop/loopback-next/issues/925) is marked as "Good first issue" by the maintainers of loopback-next. 
In the description of the issue, one of the developers proposes a fix for moving the start error handling from the main function to the main index file. 
The fix proposed by the developer has been implemented and successfully tested using the build in tests.

We submitted a pull request [PR #1151](https://github.com/strongloop/loopback-next/pull/1151) to loopback-next according to the LoopBack guidelines.
After dealing with the code linter and some small required changed proposed by the developers it was approved.
Three of the developers (of which one code owner) approved it and it was merged 3 days after submitting it.

### Issue #1218
While analysing the code structure for technical debt we proposed a change of the package structure in [issue #1218](https://github.com/strongloop/loopback-next/issues/1218).
Coincidently one of the code owners also thought of the same change just before we submitted the issue.
The developer looking at our submitted issue asked if we wanted to submit a pull request for this issue.

Since it seemed to be a very simple change we started working on the pull request.
Unfortunately it was a more complex issue requiring changing of several configuration files, some documentation changes and a small change in the code.
Eventually we were able to fix all problems and submitted [pull request #1231](https://github.com/strongloop/loopback-next/pull/1231) which is waiting for approval.



## Mattermost

Team Mattermost, as a very community-friendly team, has been maintaining a Help Wanted List on GitHub as [Issues](https://github.com/mattermost/mattermost-server/issues?utf8=✓&q=is%3Aissue%20is%3Aopen%20%5BHelp%20Wanted%5D).
There are labels like `Up For Grabs`, indicating the team has recognised the issue and wants help from the community, and `Difficulty: Advanced`, `Difficulty: Intermediate` and `Difficulty: Introductory`, indicating the assumed complexity to fix the issue.

### Status Overview:

| NO.     | Issue                                                                | PR                                                               | Statues | Note                     |
| ------- | -------------------------------------------------------------------- | ---------------------------------------------------------------- | ------- | ------------------------ |
| [1](#1) | [#5465](https://github.com/mattermost/mattermost-server/issues/5465) | [#893](https://github.com/mattermost/mattermost-webapp/pull/893) | Merged  | An Help Wanted Issue     |
| [2](#2) | [#8397](https://github.com/mattermost/mattermost-server/issues/8397) | [#895](https://github.com/mattermost/mattermost-webapp/pull/895) | Merged  | An Issue We Found        |
| [3](#3) | [#7660](https://github.com/mattermost/mattermost-server/issues/7660) | [#989](https://github.com/mattermost/mattermost-webapp/pull/989) | Merged  | Repay the Technical Debt |
| [4](#4) | [#8483](https://github.com/mattermost/mattermost-server/issues/8483) | -                                                                | WIP     | An Help Wanted Issue     |

---

#### <a name="1">1. PLT-5270 Bold, italic and striked links should show link preview when available</a>

**Pull Request**: [#893](https://github.com/mattermost/mattermost-webapp/pull/893)  
**Issue**: [#5465](https://github.com/mattermost/mattermost-server/issues/5465)  
**Timetable**:

| Date                                                                                                                    | Status                             | Note                                |
| ----------------------------------------------------------------------------------------------------------------------- | ---------------------------------- | ----------------------------------- |
| [02-03-2018](https://github.com/mattermost/mattermost-server/issues/5465#issuecomment-370056150)                        | Claim the Issue/Ticket             |                                     |
| [02-03-2018](https://github.com/mattermost/mattermost-server/issues/5465#event-1502246180)                              | Ticket Approved                    |                                     |
| [03-03-2018](https://github.com/mattermost/mattermost-webapp/pull/893#issue-172676296)                                  | Submit the Pull Request            |                                     |
| [05-03-2018](https://github.com/mattermost/mattermost-webapp/pull/893#pullrequestreview-101102127)                      | Change Requested                   | Review of the first core developer  |
| [05-03-2018](https://github.com/mattermost/mattermost-webapp/pull/893/commits/f06a8b4bc7f8ecc072ecc096f6093e2da2ce6029) | [Change Made Accordingly](#change) | Get Approved by him after that      |
| [05-03-2018](https://github.com/mattermost/mattermost-webapp/pull/893#pullrequestreview-101214164)                      | Change Requested                   | Review of the second core developer |
| [05-03-2018](https://github.com/mattermost/mattermost-webapp/pull/893#discussion_r172251395)                            | [Defend Our Approach](#defend)     | Get Approved by him after that      |
| [05-03-2018](https://github.com/mattermost/mattermost-webapp/pull/893#pullrequestreview-101283461)                      | Get Approved                       | Review of the product manager       |
| [05-03-2018](https://github.com/mattermost/mattermost-webapp/pull/893#event-1504857885)                                 | Get Merged                         |                                     |

**Description**:

We found this issue by looking through the Help Wanted List, focusing on issues with the labels `Up For Grabs` and `Difficulty: Introductory`.

Mattermost has a feature called link preview, that if there is at least one url in the post, a preview of an abstract and an image of the first url will be shown below the post.
But, currently the feature does not work when the link is Markdown-formatted, i.e. **bold**, _italic_, or ~~crossed-out~~ links do not show link previews.

We identified that the webapp first checked if there was a link in the post message by calling `extractFirstLink()`, inside which the pattern matching happened.
The function ignored everything formatted in Markdown syntax.
Therefore, we added a function, `loopReplacePattern()`, to first replace all Markdown syntax with `''`, to remove all Markdown syntax, and then perform the pattern matching on the plaintext.

**Interaction with Stakeholders**:

<a name="change">*Change According to Review*</a>

One of the core developers, [@jespino](https://github.com/jespino) first reviewed our pull request and [requested a change](https://github.com/mattermost/mattermost-webapp/pull/893#pullrequestreview-101102127) for a minor bug.
We [submitted a commit](https://github.com/mattermost/mattermost-webapp/pull/893/commits/f06a8b4bc7f8ecc072ecc096f6093e2da2ce6029) to fix that accordingly. 
Later, the pull request was approved by him.

<a name="defend">*Defend Our Approach*</a>

The other one of the core developers, [@hmhealey](https://github.com/hmhealey) responded a while later.
At the beginning, he [requested](https://github.com/mattermost/mattermost-webapp/pull/893#pullrequestreview-101214164) us to try to solve the nested structure [with a single regular expression](https://github.com/mattermost/mattermost-webapp/pull/893#discussion_r172232349) instead of a loop function.
We [argued](https://github.com/mattermost/mattermost-webapp/pull/893#discussion_r172251395) that the native JavaScript regex engine supports neither balanced matching nor recursive matching.
Then @hmhealey did some searching himself and agreed with our approach by [approving the pull request](https://github.com/mattermost/mattermost-webapp/pull/893#pullrequestreview-101262723).

---

#### <a name="2">2. Remove a duplicate max-nested-callbacks key in the `.eslintrc.json` file</a>

**Pull Request**: [#895](https://github.com/mattermost/mattermost-webapp/pull/895)  
**Issue**: [#8397](https://github.com/mattermost/mattermost-server/issues/8397)  
**Timetable**:

| Date                                                                                               | Status                                | Note                                         |
| -------------------------------------------------------------------------------------------------- | ------------------------------------- | -------------------------------------------- |
| [02-03-2018](https://github.com/mattermost/mattermost-server/issues/8397#issue-301808266)          | Submit the Issue                      |                                              |
| [05-03-2018](https://github.com/mattermost/mattermost-server/issues/8397#issuecomment-370433380)   | Issue Recognised                      | @hmhealey replied and asked us to fix it     |
| [05-03-2018](https://github.com/mattermost/mattermost-webapp/pull/895#issue-172897653)             | Submit the Pull Request               |                                              |
| [05-03-2018](https://github.com/mattermost/mattermost-webapp/pull/895#pullrequestreview-101253878) | Approved by the first Core Developer  | The team requires that a pull request should |
| [06-03-2018](https://github.com/mattermost/mattermost-webapp/pull/895#pullrequestreview-101589568) | Approved by the second Core Developer | be approved by at least two core developers  |
| [06-03-2018](https://github.com/mattermost/mattermost-webapp/pull/895#event-1506771152)            | Get Merged                            |                                              |
| [06-03-2018](https://github.com/mattermost/mattermost-webapp/pull/895#event-1506771152)            | Issue Closed                          |                                              |

**Description**:  

The second contribution we made was different from the first one, because this time we found the issue ourselves.

Team Mattermost provides an `.eslintrc.json` file for ESLint as the code style standard to contributors.
And while we read through the source code, we noticed that there was a duplicate rule in the file.
We submitted an issue regarding this on GitHub right after we found it, and submitted a pull request later to fix it.

---

#### <a name="3">3. Migrate `delete_post_modal.jsx` to be pure and use Redux</a>

**Pull Request**: [#989](https://github.com/mattermost/mattermost-webapp/pull/989)  
**Issue**: [#7660](https://github.com/mattermost/mattermost-server/issues/7660)  
**Timetable**:

| Date                                                                                               | Status                                | Note                                               |
| -------------------------------------------------------------------------------------------------- | ------------------------------------- | -------------------------------------------------- |
| [16-03-2018](https://github.com/mattermost/mattermost-server/issues/7660#issuecomment-373849899)   | Claim the Issue/Ticket                |                                                    |
| [19-03-2018](https://github.com/mattermost/mattermost-server/issues/7660#issuecomment-374186085)   | Ticket Approved                       |                                                    |
| [22-03-2018](https://github.com/mattermost/mattermost-webapp/pull/989#issue-176750182)             | Submit the Pull Request               |                                                    |
| 22-03-2018                                                                                         | Changes Requested                     | Multiple discussion happened with the Stakeholders |
| 22-03-2018                                                                                         | Changes Made Accordingly              | And multiple changes had been made accordingly     |
| [23-03-2018](https://github.com/mattermost/mattermost-webapp/pull/989#pullrequestreview-106375549) | Approved by the first Core Developer  |                                                    |
| [23-03-2018](https://github.com/mattermost/mattermost-webapp/pull/989#pullrequestreview-106471203) | Approved by the second Core Developer |                                                    |
| [23-03-2018](https://github.com/mattermost/mattermost-webapp/pull/989#event-1537723633)            | Get Merged                            |                                                    |

**Description**:  

Mattermost is currently transitioning from Flux towards Redux to help relieve some **Technical Debt**.
We wanted to have a meaningful contribution in this area.
The Mattermost team keeps a [spreadsheet](https://docs.google.com/spreadsheets/d/1AlFS2F4H74JsONxIS_VNZBxrVJolZxFh7yN46RNCwyg/edit#gid=0) with all the components that still need to be moved over.
This looked like a good opportunity.

Before claiming a component we looked at the complexity of each component.
As we are still new to Flux/Redux, we looked at how complex the change would be and choose one that seemed doable.
We choose to look into the `delete_post_modal`.
It deals with a few cases about deleting a post, i.e. delete a simple post, delete a post with comments, delete a comment, and delete a post by editing it into a null text.

This was, however, still not an easy contribution, requiring multiple files to be changed and corresponding tests to be written.
We followed the [instruction](https://docs.mattermost.com/developer/webapp-to-redux.html) proposed by the team, to achieve this.
All `PropTypes` of the modal were moved and documentations were added.
The modal was migrated from `React.Component` into `React.PureCompoment`.
All related stores were changed into a single one.
And variables holding previous store states were moved into props fed.
Working on this helped us to take a further look into the actual architecture of the project.
Eventually, we added a new component test on this modal from scratch, following a [blog post](https://grundleborg.github.io/posts/react-component-testing-in-mattermost/) from the team, which granted us a better understanding of the Standardization of Testing and the Testing Debt of the team and the project.

**Interaction with Stakeholders**:

After we claimed the ticket, core developers from the Mattermost team had provided a lot of help.
They have published a guideline for [Migrating the Webapp to Redux](https://docs.mattermost.com/developer/webapp-to-redux.html), explaining what to be done for each step.
When requiring changes, they also provided possible solutions in the [pull request conversation](https://github.com/mattermost/mattermost-webapp/pull/989/conversation).



## Mbedos

* [PR #6286](https://github.com/ARMmbed/mbed-os/pull/6286) This pull request improves the documentation of the project by fixing typos/sentences in several README.md files.
  * The pull request is issued on 6-3-2018 and has passed CI checks. This pull request was merged into MBed OS into release 5.7.7 and 5.8.1 on 8-3-2018.
  * The reviewers for this pull request are [@cmonr](https://github.com/cmonr) and [@0xc0170](https://github.com/0xc0170), both have approved the changes.
* [PR #6537](https://github.com/ARMmbed/mbed-os/pull/6537) This pull request is to include coverage from c tests that always run whenever a Travis CI build is triggered.
    * The pull request is issued on 3-4-2018 and has passed CI checks.
    * At this moment, the pull request is marked to need a review.



## OSU

### Osu issue [#2022](https://github.com/ppy/osu/issues/2022): Shift input being handled incorrectly after both shift keys are held down at the same time.

**Anamnesis**: Issue was opened more than 20 days before it was noticed by us. This issue was discovered during gameplay proccess. Apparently, after pressing both Left and Right Shift keys and releasing them both, osu! still counts Shift as pressed. This issue harms gameplay. Interestingly, other modifier keys, such as Ctrl and Alt (if assigned to specific action) do not produce the same effect. It seems that it is noncritical issue: it's difficult to press both shifts at same time and it's hard to imagine that someone will deliberately press both shifts during game. 

**Initial suggestion**: It's probably a mistake during modifier key processing in _osu!_ framework.

**Working on issue**: At first this issue seemed to be relatively easy to fix. We just thought that we would need to find the mistake in processing the Shift Key Down action and fix it. We managed to track all the way from InputManager to the beginning of processing KeyDown event in OpenTKKeyboardHandler class. We discovered that the osu! framework uses [OpenToolkit](https://github.com/opentk/opentk) library for low-level processing. Apparently this library is causing troubles. We performed some tests and [found](https://pastebin.com/GgD5tCbg) that the osu! framework processes exactly what is forwarded to it by OpenTK. Therefore this can be considered to be an OpenTK issue. We suggested to move this issue from gameplay to framework project and to label it with OpenTK label. Potential solution could be to create a patch between OpenTK and osu! framework which will solve this very specific issue, but it doesn't seem to be an appropriate solution.

In an attempt to get information about this issue from different sources, we also [reported](https://github.com/opentk/opentk/issues/739) the same issue to the OpenTK project. No answer has been provided at the time of writing.

**Pull request**: Is not created. For now we would prefer to wait for OpenTK response to understand if some actions on our side are actually required. Should this issue be fixed in next release of OpenTK library, there would be no need of any actions on our part, because the inputs processing of osu! framework works fine. This issue is not critical, so it doesn't require immediate temporary patch until it is fixed by OpenTK.

**Results**: Mechanism of processing input signals of osu! was studied. Problem was identified and reported.

### Osu-framework issue [#1444](https://github.com/ppy/osu-framework/issues/1444): TextBox doesn't handle left/right arrows where it used to.

**Anamnesis**: Recent issue, was opened on the same day it was noticed by us. This issue was discovered during testing. In text box Left and Right keys were not handled. This issue complicates navigating through text boxes. 

**Initial suggestion**: Probably, just a misplacement of overriding statement due to inattention.

**Working on issue**: We managed get to the bottom of this quickly. There is a flag in parent class TextBox which indicates if this kind of navigation should be processed or ignored. In the parent class the flag was set to _true_, meaning that Left and Right keys should be processed as we would expect. However, in the children class FocusedTextBox, which is actually used in osu!, this value was set to _false_, meaning that pressing Left and Right keys would not move the cursor. After setting this value to _true_, the navigation is working as expected. 

**Pull request**: Pull request [#2173](https://github.com/ppy/osu/pull/2173) was created to merge this changes to master. 

**Results**: Although the problem causing the issue was identified and fixed correctly, the pull request was rejected due to unnecessary commits in history. The pull request was closed and [a new one](https://github.com/ppy/osu/pull/2174) with the same fix was created by _peppy_ in order to fix the issue. 
_peppy_ also strongly recommended that we improve our git skills if we want to contribute to this (or, frankly speaking, to any) project in order to avoid garbaging the repository with weird commits and annoying integrators with contributors incompetence.

### Osu issue [#2180](https://github.com/ppy/osu/issues/2180): "Disable mouse wheel" under "visual settings

**Anamnesis**: Recent issue, was opened on the same day it was noticed by us. In the gameplay Visual Settings menu there is a checkbox, responsible for enabling/disabling the handling of the mouse wheel. There is two problems. Firstly, Visual Settings is not the appropriate menu for such an option. Secondly, this option is not something that a user would toggle often, especially during gameplay. Therefore a reasonable suggestion would be that this option should not be present in gameplay settings menu.

**Initial suggestion**: Removing of legacy code is needed. Probably was part of some test or of previous menu concept.

**Working on issue**: Pretty simple issue. We just needed to remove the lines of code responsible for creating the checkbox for this option and to bind it to OsuSettings. _peppy_ said that there is a plan to create subsections in this menu (like for settings in main menu), but 

- this is not in short-term plans and it is not an urgent matter.
- this option will not be present in this menu anyways.

So he confirmed that just removing this option would be an appropriate solution for now.

**Pull request**: Pull request [#2184](https://github.com/ppy/osu/pull/2184) was created to merge this changes to master.

**Results**: Pull request was approved and merged. This time, we applied feedback from _peppy_ which was given in pull request [#2173](https://github.com/ppy/osu/pull/2173) and our pull request was accurate and clean.

### Osu issue [#2234](https://github.com/ppy/osu/issues/2234): "Caps lock warning should only appear when typing password"

**Anamnesis**: Recent issue, was opend on the same day it was noticed by us. Some applications, which requires typing password, usually warns user about CapsLock key being pressed. So it is more difficult to user to enter incorrect data, and it is easier to avoid annoying procedure of repeating typing. In _osu!_ this warning appears for every text box, including those which are not in focus now. So interface is garbaged with unnecessary warnings making

**First suggestion**: Most probably it would be a simple solution. Just need to insert additional conditions, so the warning appears only for focused window.

**Working on issue**: Just as was stated in previous section - this solved a problem.

**Pull request**: Pull request was not created because of two reasons. Firstly, by the time we were available to figure out solution [pull request](https://github.com/ppy/osu/pull/2237) was already created, so there was no point in duplicating PR. Secondly, _peppy_ suggested alternative fix, which is addressing the problem at its source: TextBox was handling text input, but it should leave the input handling process to InputHandler in a first place. To sum it up, a better solution had already been proposed.

**Results**: We should not only consider the behavior of component when fixing bugs. We should also consider purposes of the specific components and make sure that the specific component will serve the purpose it was created for and not any other purposes, as it may cause a chaos in design.

Below will be listed issues for which we were not able to reach some conclusive results, but still we put an effort to them.

* Osu issue [#2315](https://github.com/ppy/osu/issues/2315): Shift-delete no longer works to delete beatmaps at song select. Title here is self-explanatory. Errors in the input processing were suggested initially. However, it appears that the problem was in the wrong focus processing. [Pull request](https://github.com/ppy/osu-framework/pull/1493) for this issue now awaiting review.
* Osu issue [#2286](https://github.com/ppy/osu/issues/2286): When audio is set to mute, specific audio samples are still occasionally played. Initial suggestion was that muting was not performed accurate enough. However, we were not able to think about something more convenient than just creating an additional condition for all sound effects in the game. This solution cannot be considered as a good one. Therefore we didn't create pull request. Firstly, it most likely would have been rejected by _peppy_, as he cares very much about the code quality. Secondly, it's not a good policy, to submit pull requests with solutions, which are not satisfactory for us.


## Phaser

### Issue 3231
As a first contribution, we decided to fix Phaser [issue 3231](https://github.com/photonstorm/phaser/issues/3231).
This seemed like a good starting point as it concerned a rather minor issue which allowed us to get to know the system gradually.

Phaser allows both Canvas and WebGL rendering.
The issue at hand described that the TileSprite gameobject did not rotate in Canvas rendering mode, even though the rotation was specified.
We started tracing down the issue and found out each gameobject has a separate JavaScript file for WebGL and Canvas rendering.

We investigated the "TileSpriteCanvasRenderer.js" file and discovered that the rotation was not implemented at all. Furthermore we noticed that scaling and flipping were not implemented either.

Before we were able to start implementing, we had to read into Canvas rendering and drawing.
Unfortunately the solution was not as simple as just calling the `rotate()` function.
The reason for this is that this rotates the sprite around it's origin, whereas it should rotate around it's center.
Therefor the tilesprite rendering needed to be partially rewritten as some additional Canvas translations were required.
In the end we managed to get the rotation, scaling and flipping working but the fix turned out to be more complicated than initially anticipated.

After making a pull request in our forked repository and reviewing the code, we made a [pull request](https://github.com/photonstorm/phaser/pull/3358) in the Phaser repository.
The Travis CI build succeeded and the pull reqyest got accepted soon after.

### Issue 3268
The second contribution we made was a fix for [issue 3268](https://github.com/photonstorm/phaser/issues/3268). The creator of this issue described that object based atlas texture loading does not work as explained in the official documentation. This was caused by the fact that the path of the texture was not loaded correctly. The cause was found in the AtlasJSONFile.js filetype loader and after the fix was implemented we created a [pull request](https://github.com/photonstorm/phaser/pull/3357). This got accepted.

We also opened an issue notifying Richard about an issue regarding a texture scaling difference between the Canvas and WebGL renderers, [issue 3338](https://github.com/photonstorm/phaser/issues/3338).
This was fixed soon after, with us confirming the fix which Richard requested.

Furthermore Tomas implemented a fix for [issue 3215](https://github.com/photonstorm/phaser/issues/3215), but unfortunately someone else submitted a pull request for the same issue before we did.

### Issue 3385
We made another contribution by fixing [issue 3385](https://github.com/photonstorm/phaser/issues/3385).
A user reported a bug where he described that setting the alpha value for RenderTexture does not work in WebGL mode, whereas it did work in Canvas mode.
It turned out it is possible to set the tint of a texture, but the WebGL renderer always set the tint to `0xffffff`, which is white.
The alpha value was simply ignored.
We fixed this by calculating the tint based on the set alpha value.
The closer this tint is to black, the more transparent the texture is.
We created the [pull request](https://github.com/photonstorm/phaser/pull/3445) and it was merged in a matter of minutes.
It will be part of the next release.

### Technical debt improvements
For improving the technical debt we also made some pull requests to fix bugs or code smells. We made pull requests for:
- [Removing an incorrect line](https://github.com/delftswa2018/phaser/pull/4)
- [Strange self-assignments](https://github.com/delftswa2018/phaser/pull/5)
- [Unused variable assignment](https://github.com/delftswa2018/phaser/pull/6)

### Example fixes
We also made 2 fixes related to the example-repo called phaser3-examples.
We created a [pull request](https://github.com/photonstorm/phaser3-examples/pull/32) on fixing a Phaser 3 example about mouse events.

Also, we made a [pull request](https://github.com/photonstorm/phaser/pull/3509) which fixes a function call in the normal Phaser repository. This also fixed 3 issues/examples on the original phaser3-examples repository. 



## Spark

### Contribution to documentation
Our first contribution to Apache Spark is a contribution to the documentation. We thought that that the context model we created for D1 would be useful in the documentation of Apache Spark, as it provides clear insight into how Apache Spark fits into the world around it. Apache Spark requires a JIRA issue for every Pull Request, so we created both of these according to the [contributing guidelines](http://spark.apache.org/contributing.html). They are available at the following locations:

> **JIRA issue**: [https://issues.apache.org/jira/browse/SPARK-23579](https://issues.apache.org/jira/browse/SPARK-23579)  
> **Pull Request**: [https://github.com/apache/spark/pull/20731](https://github.com/apache/spark/pull/20731)

Since we have never contributed to the Apache Spark project before, the automatic Pull Request builder requires approval from one of the committers before it will test the Pull Request. This requires a manual action which is what we are currently waiting for.

After waiting a while, we noticed that nobody had looked at this Pull Request yet, so we sent a mail to the mailing list about it. Shortly after that, we got some feedback in the Pull Request. Unfortunately, they considered the changs to the documentation to be too detailed for the introduction page of the documentation. Additionally, the diagram we made for the context model contained several proprietary logo's which they can't display, given that Spark is an Apache Software Foundation project. While we did try to contribute only some text to the documentation, we ultimately had to agree with the Spark contributors that this contribution may not be that useful to them. So we closed the Pull Request, as they requested, without it being merged.

### Contribution to reduce technical debt
While we were doing research on technical debt, we found out that the Spark code contains several pieces of code where the static code analysis tool, Scalastyle, is disabled. We identified 2 pieces of code where we could easily enable the code analysis tool again.

1. The first one actually didn't require any changes to the code. The code analysis tool was disabled but the code was entirely valid according to the current rules. However, the code analysis tool was entirely disabled here, so this piece of code would never be analyzed against any of the rules.
2. The second piece of code had the code analysis tool disabled for the rule that check the maximum length of the line. We found that the reason they exceeded this length was that they had a long URL in there which they couldn't split over 2 lines. But we found out that the site that this URL points to also has shortened variants of their URLs. So we could replace the long URL with an official, shortened version of the same URL and the line would no longer exceed the maximum length.

We started our contribution for the first one by creating an issue in JIRA and then a Pull Request on Github, as described in the contribution guidelines for Spark. The issue and Pull Request are available here:

> **JIRA issue**: [https://issues.apache.org/jira/browse/SPARK-23769](https://issues.apache.org/jira/browse/SPARK-23769)  
> **Pull Request**: [https://github.com/apache/spark/pull/20880](https://github.com/apache/spark/pull/20880)

We actually got some feedback quite quickly on both the issue and Pull Request, from the same person. An interesting side-note, he actually submitted a contribution to remove a problem identified by the code analysis tool as well, shortly before us. His contribution was on the API interface in the R programming language though.

Our first bit of feedback was that we didn't need to create the JIRA issue for this contribution, since it's considered a minor change. This wasn't documented in the contribution guidelines so it seems more like an optimization that they've informally introduced to reduce overhead on minor changes.

We also got some feedback on a more appropriate way to test our change, using a Spark-provided script instead of running Maven directly. So we tested our changes this way from then on.

Mainly, we were asked to look around for similar problems. We noticed and documented this behavior in our earlier technical debt research as one of the ways that Spark developers reduce technical debt. Since we already looked at all these pieces of code with Scalastyle disabled, in Spark Core, we could immediately answer that we already did this. We mentioned that we only found one other piece of code that could be easily fixed. Given that we now knew we didn't need a JIRA issue for this, we created only a Pull Request for this. The Pull Request can be found here:

> **Pull Request**: https://github.com/apache/spark/pull/20882

However, the Spark developers preferred having both of these contributions in a single Pull Request. So we ended up closing this second Pull Request and adding the second contribution to the same Pull Request as the first one.

We got approval for Jenkins to start a build with our changes. After this was completed the code change was approved by another Spark developer than the one we'd been communicating so far. This may indicate that they try to adhere to the four-eyes principle for code reviews. It was then merged and the Pull Request was closed. We then deleted the branch we created for this change since it was no longer needed.

An interesting note here is that the Pull Request in Github doesn't actually say that the Pull Request is merged. According to the status of the Pull Request in Github, it was just closed without being merged. This is due to the way that Spark is set up. The Github repository is actually just a mirror of their main repository, hosted by the Apache Software Foundation. So they merge the code in their main repository which automatically triggers the Pull Request to be closed. They usually put a comment in the Pull Request (manually) to confirm that it was merged. But you can always verify this because the commit where the code is merged is linked to the Pull Request since it's the commit that closes the Pull Request. This results in a message like this in the Pull Request:

> [`asfgit`](https://github.com/asfgit) `closed this in` [`6ac4fba`](https://github.com/apache/spark/commit/6ac4fba69290e1c7de2c0a5863f224981dedb919) `19 hours ago`

You can then look at the linked commit to see that the code was really merged.

### Our Experience
Our experience from contributing these changes was quite positive. While we didn't get the documentation change merged, the Spark developers were very helpful, respectful and provided constructive criticism. With the changes to reduce technical debt, we got a response very quickly. This may indicate that they care greatly about reducing technical debt, but it may just have been a case of good timing. Either way, we once again got treated very well and got very useful and helpful feedback.

With the changes to reduce technical debt, the process actually went quite quickly. These are the times of the main events of the Pull Request, relative to when the Pull Request was created:

* First response after ~1 hour
* Approval for the Pull Request to be built after ~3 hours
* Code change approved after ~20 hours
* Pull request merged and closed within 24 hours

The most time elapsed between when the build was started and the code change was approved. The build actually took about 4 hours so there wasn't much to be done until this was finished. But even so, this seems like quite a quick turnaround time for a Pull Request. We were very happy with this.


## TypeScript


- Pull request: [Microsoft/TypeScript#22275](https://github.com/Microsoft/TypeScript/pull/22275)
- Closes issue: [Microsoft/TypeScript#21617](https://github.com/Microsoft/TypeScript/issues/21617)

This issue was found in TypeScript's issue tracker labeled with "help wanted", "good first issue", "Bug" and "Domain: Error Messages". The milestone attached to this issue is "Community", which is an indication that anyone from the community can help fix the issue.

The issue was created because a more elaborate error message should be given when iterating anything that is not an array or a string using a `for-of` loop. In some cases, enabling the compiler option `--downlevelIteration` would resolve the error, but this was not clear from the error message.

The issue was created at 4 February 2018 by [@weswigham](https://github.com/weswigham), and has on that day been discussed with [@DanielRosenwasser](https://github.com/DanielRosenwasser) and a member of the community.

On 12 February 2018, GitHub user [@isc30](https://github.com/isc30) asked what he could do to help, but after that the issue was abandoned.

On 1 March 2018, during our weekly Thursday meeting, we had found this issue and Maarten ([@mpsijm](https://github.com/mpsijm)) decided to investigate it. After a few hours of working through contribution guidelines and getting the development environment to work, we were confident that we would be able to fix this issue. Because Maarten had other duties in the afternoon, we decided that he should write a comment on the issue to notify that he would be fixing it, to avoid anyone else taking up the issue.

That same evening, around 11 PM, the issue was fixed and a pull request was made. We included a tag to @weswigham and @DanielRosenwasser to encourage review from them, since they had initially discussed the issue. Fifteen minutes later, @weswigham had reviewed and approved our pull request and asked @DanielRosenwasser "to double check the copy".

On 8 March 2018, after having received no response in a week, we decided to comment on the pull request, mentioning @weswigham and @DanielRosenwasser.

On 30 March 2018, [@mhegazy](https://github.com/mhegazy) merged the pull request, apologizing for the delay.



## Vue.js

### Contribution 1: "Style: change control flow to early exit"

[Pull Request Page](https://github.com/vuejs/vue/pull/7729)

* Authors:
  + [Lars Stegman](https://github.com/LarsStegman)
* Reviewers:
  + [Eduardo San Martin Morote](https://github.com/posva)
  + [Evan You](https://github.com/yyx990803)

#### Description
This pull request changed the function `src/core/observer/watcher.js#teardown` to exit early instead of having a nested if statement.

For example:

```
if(condition) {
    doSomething()
}
```
was changed to 

```
if(!condition) {
    return
}

doSomething()
```

#### Result

Even though the pull request has not been closed yet, Eduardo has commented that they prefer the first version. For this reason we assume that the contribution will not be merged. Eduardo responded to our pull request within an hour and thinks it's great that we want to get more involved with the codebase.

After a couple of days the pull request was closed by Evan You. He commented that in general they do not accept stylistic pull requests.

#### Note

We had some trouble getting the CI to work on all tests. We didn't think this was due to an error by us and we were right about this. The Weex runtime version did not match the test case versions in the `dev` branch (see discussion in [#7737](https://github.com/vuejs/vue/pull/7737)). The code base also has some flaky tests, which might be a nice bigger contribution to pay off technical debt.

### Contribution 2: "docs: fix grammar mistake"

[Pull Request Page](https://github.com/vuejs/vue/pull/7739)

* Authors:
  + [Lars Stegman](https://github.com/LarsStegman)
* Reviewers:
  + [Evan You](https://github.com/yyx990803)

#### Description

Fixes a grammar issue in a documenting comment.

#### Result

Merged after a couple of days without comment by Evan You.

### Contribution 3: "fix(e2e-todomvc-test): trigger click on .new-todo instead of footer"

[Pull Request Page](https://github.com/vuejs/vue/pull/7938)

* Authors:
  + [Tim Speelman](https://github.com/TimSpeelman)
* Reviewers:
  + [Blake Newman](https://github.com/blake-newman)

#### Description

A test fails at `test\e2e\specs\todomvc.js:124:15`, returning:


```
Testing if element 
<.todo:nth-child(1) label> contains text: "edited!".
 - expected "edited!" but got: "test"
```

The `todomvc.js` spec double clicks the label of an already-completed todo (line 117), then changes its text (lines 120-121) and finally blurs the field by clicking something else (line 122): the footer. But here's the catch: it actually "clicks the in-view centre point" of the footer (according to Nightwatch docs), which is where the 'Active' filter button is rendered (at least when using Chrome, this does not happen in PhantomJS).

As it clicks the 'Active' filter, the just-edited todo is hidden. When the spec then asserts the text of the first visible todo, it does not match its expectations and fails. So the fault lives on line 122: `browser.click('footer')`.

A simple solution is simply to click something else, like the `.new-todo` text input. This solves the issue. 

We made this pull request to prepare for moving the Vue codebase from PhantomJS to Chrome Headless as described in the Technical Debt chapter.

#### Result

The pull request has been approved by Blake Newman, member of the core team, and awaits merging by Evan You.

### Contribution 4: "refactor(compiler): Document and refactor filter parser"

[Pull Request Page](https://github.com/vuejs/vue/pull/7948)

* Authors:
  + [Arthur Guijt](https://github.com/8uurg)
* Reviewers:
  + [HerringtonDarkholme](https://github.com/HerringtonDarkholme)

#### Description

By analyzing method length and cyclomatic complexity we found a few methods that were interesting to look at. The top contender is a function in filter-parser.js, which is effectively a string parsing state machine.

The original version was extremely hard to understand, stepping through the code using a debugger, looking into when certain booleans and numbers were incremented to figure out how it worked. This code hasn't been touched in a long time, and given its complexity may have turned into something like legacy code; something that may at some point may have to be replaced altogether.

The main goal of this parser turned out to be very easy to explain in words: find the | that separate filters and turn it into a callable expression. The former part, the filter separation, was the most problematic.

First of all, comments were added to provide the rationale behind the code. A || isn't a |, just like a | inside of any kind of string and a | inside of a regex expression aren't the | that separate filters. This was added as documentation explaining that these cases should be ignored.

A large chunk of the method's complexity was related to detecting string literals. We wrote a special method for skipping over strings and similar cases, named seek. This method looks for the ending of the corresponding string while ignoring escape sequences. By using seek we could remove the four variables and their corresponding if-statements for detecting the end. This greatly reduced the complexity of the code.

Moreover, magical numbers for character codes were replaced with constants whose names represent the actual character, to make things more readable. Finally some slight cleanup was performed regarding redundant if-statements and variables.

#### Result

A review has been posted by HerringtonDarkholme, praising the contribution.
The PR has not been accepted yet.



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