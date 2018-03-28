# Delft Students on Software Architecture: DESOSA 2018


**[Arie van Deursen], [Andy Zaidman], [Maurício Aniche], [Liam Clark], [Gijs Weterings] and [Romi Kharisnawan].**<br/>
*Delft University of Technology, The Netherlands, April, 2018*

[arie van deursen]: https://avandeursen.com
[Andy Zaidman]: http://www.st.ewi.tudelft.nl/~zaidman/
[maurício aniche]: http://www.mauricioaniche.com
[Liam Clark]: https://www.linkedin.com/in/liam-clark-b5375baa/
[Gijs Weterings]: https://www.linkedin.com/in/gijs-weterings/
[Romi Kharisnawan]: https://www.linkedin.com/in/romikharisnawan

We are proud to present the fourth edition of
_Delft Students on Software Architecture_, a collection of (TODO) architectural descriptions of open source software systems written by students from Delft University of Technology during a [master-level course][in4315] taking place in the spring of 2018.

[in4315]: http://www.studiegids.tudelft.nl/a101_displayCourse.do?course_id=38330

In this course, teams of approximately 4 students could adopt a project of choice on GitHub.
The projects selected had to be sufficiently complex and actively maintained (one or more pull requests merged per day).

During an 8-week period, the students spent one third of their time on this course, and engaged with these systems in order to understand and describe their software architecture.

Inspired by Brown and Wilsons' [Architecture of Open Source Applications][aosa], we decided to organize each description as a chapter, resulting in the present online book.


## Recurring Themes

The chapters share several common themes, which are based on smaller assignments the students conducted as part of the course.
These themes cover different architectural 'theories' as available on the web or in textbooks.
The course used  Rozanski and Woods' [Software Systems Architecture][rw], and therefore several of their architectural [viewpoints] and [perspectives] recur.

[viewpoints]: http://www.viewpoints-and-perspectives.info/home/viewpoints/
[perspectives]: http://www.viewpoints-and-perspectives.info/home/perspectives/

The first theme is outward looking, focusing on the use of the system.
Thus, many of the chapters contain an explicit [stakeholder analysis], as well as a description of the [context] in which the systems operate.
These were based on available online documentation, as well as on an analysis of open and recently closed issues for these systems.

[context]: http://www.viewpoints-and-perspectives.info/home/viewpoints/context/
[stakeholder analysis]: http://www.mindtools.com/pages/article/newPPM_07.htm

A second theme involves the [development viewpoint][development], covering modules, layers, components, and their inter-dependencies.
Furthermore, it addresses integration and testing processes used for the system under analysis.

[development]: http://www.viewpoints-and-perspectives.info/home/viewpoints/

A third recurring theme is _technical debt_. Large and long existing projects are commonly vulnerable to debt.
The students assessed the current debt in the systems and provided proposals on resolving this debt where possible.

## First-Hand Experience

Last but not least, the students tried to make themselves useful by contributing to the actual projects.
With these contributions the students had the ability to interact with the community; they often discussed with other developers and architects of the systems. This provided them insights in the architectural trade-offs made in these systems.

The students have written a collaborative chapter on some of the contributions made during the course. It can be found in the dedicated [contributions chapter][contrib-chapter].

[contrib-chapter]: contributions/chapter.md

## Feedback

While we worked hard on the chapters to the best of our abilities, there might always be omissions and inaccuracies.
We value your feedback on any of the material in the book. For your feedback, you can:

* Open an issue on our [GitHub repository for this book][dswa.io].
* Offer an improvement to a chapter by posting a pull request on our [GitHub repository][dswa.io].
* Contact @[delftswa][dswa.tw] on Twitter.
* Send an email to Arie.vanDeursen at tudelft.nl.

[dswa.io]: https://github.com/delftswa2018/desosa2018
[dswa.tw]: https://twitter.com/delftswa


## Acknowledgments

We would like to thank:

* Our guest speakers: [Bert Wolters], [Sander Knape], [Allard Buijze], and [Bob Bijvoet].
* All open source developers who helpfully responded to the students' questions and contributions.
* The excellent [gitbook toolset] and [gitbook hosting] service making it easy to publish a collaborative book like this.

[gitbook toolset]: https://github.com/GitbookIO/gitbook-cli
[gitbook hosting]: https://www.gitbook.com/

[Bert Wolters]: https://www.linkedin.com/in/bert-wolters-01652839/
[Sander Knape]: https://www.linkedin.com/in/sander-knape-7bb61537/
[Allard Buijze]: https://www.linkedin.com/in/abuijze/
[Bob Bijvoet]: http://slides.com/bobbijvoet/

## Previous DESOSA editions

1. Arie van Deursen, Maurício Aniche, Andy Zaidman, Valentine Mairet, Sander van der Oever (editors). Delft Students on Software Architecture: [DESOSA 2017], 2017.
1. Arie van Deursen, Maurício Aniche, Joop Aué (editors). Delft Students on Software Architecture: [DESOSA 2016], 2016.
1. Arie van Deursen and Rogier Slag (editors). Delft Students on Software Architecture: DESOSA 2015. [DESOSA 2015], 2015.

[DESOSA 2017]: https://delftswa.gitbooks.io/desosa-2017/content/
[DESOSA 2016]: https://delftswa.gitbooks.io/desosa2016/content/
[DESOSA 2015]: https://delftswa.github.io/

## Further Reading

1. Arie van Deursen, Maurício Aniche, Joop Aué, Rogier Slag, Michael de Jong, Alex Nederlof, Eric Bouwers. [A Collaborative Approach to Teach Software Architecture][sigcse]. 48th ACM Technical Symposium on Computer Science Education (SIGCSE), 2017.
1. Arie van Deursen, Alex Nederlof, and Eric Bouwers. Teaching Software Architecture: with GitHub! [avandeursen.com][teaching-swa], December 2013.
1. Amy Brown and Greg Wilson (editors). [The Architecture of Open Source Applications][aosa]. Volumes 1-2, 2012.
1. Nick Rozanski and Eoin Woods. [Software Systems Architecture: Working with Stakeholders Using Viewpoints and Perspectives][rw]. Addison-Wesley, 2012, 2nd edition.

[sigcse]: https://pure.tudelft.nl/portal/en/publications/a-collaborative-approach-to-teaching-software-architecture(0c7f2aeb-f2d6-4c56-9ab7-5f47f73d133f).html
[teaching-swa]: http://avandeursen.com/2013/12/30/teaching-software-architecture-with-github/
[rw]: http://www.viewpoints-and-perspectives.info/
[aosa]: http://aosabook.org/

## Copyright and License

The copyright of the chapters is with the authors of the chapters. All chapters are licensed under the [Creative Commons Attribution 4.0 International License][cc-by].
Reuse of the material is permitted, provided adequate attribution (such as a link to the corresponding chapter on the [DESOSA book site][desosa]) is included.

Cover image credits:
TU Delft library, TheSpeedX at [Wikimedia](https://commons.wikimedia.org/wiki/File:Library_TUDelft.jpg);
Owl on [Emojipedia Sample Image Collection](http://emojipedia.org/emojipedia/sample-images) at [Emojipedia](http://emojipedia.org/emojipedia/sample-images/owl);
Feathers by [Franco Averta](http://www.flaticon.com/authors/franco-averta) at [Flaticon](http://flaticon.com).


[![Creative Commons](images/cc-by.png)][cc-by]

[cc-by]: http://creativecommons.org/licenses/by/4.0/
[desosa]: https://www.gitbook.com/book/delftswa/desosa2018/details