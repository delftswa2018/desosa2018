# Journal
***

## Week 1

This week, we got our first courses on Software Architecture. We assembled a team of 4 people around the project osu! :

- Maiko
- Pavel
- Jonathan
- Vincent

Osu github can be found here : https://github.com/ppy/osu
Our team code repository can be found here : https://github.com/delftswa2018/osu


Then we copied a nicely done hours-tracking sheet form the Mattermost general channel. 

The hours-tracking sheet is here (read-only!) : https://docs.google.com/spreadsheets/d/1w9uZHK7mUF7WfhMJO4EdlnyTnnwtVU-hAPIzG4IlzBg/edit?usp=sharing

Our first meeting on Friday 16th February has for objectives to meet each other and to set up the work organization in the group and try to look a bit about the stakeholders of osu!. 

We used the meeting time to:

- Define our working method (each contribution requires reviews from two other group members before, keeping track of progress of tasks will be done in GitHub issues). The global workflow is described below.
- Set a few initial milestones for the project (apart from course deliverables), and set up tasks to be completed to reach the milestones.
- Attributed group members to those tasks. See hour-keeping chart for details.
- Study previous years chapters, to get insight on what was done by the previous groups and quickly evaluate some chapter design possibilities
- Got our hands on the project's GitHub and, for those who were least familiar with the way of working of GitHub, we familiarized ourselves with it
- Start working on the tasks (browsing open and recently closed issues, analyzing the contributions made to the projects, etc)

##### Group workflow

The workflow was defined as follows:

1. During weekly meeting, review the work of the previous week that wasn't already discussed on GitHub
2. Review deliverables requirement and decide which are appropriate (and possible) to address
2. Make those requirements into clearly defined tasks (with predefined output guidelines)
3. Assign those tasks to group members
4. Create the corresponding issues on GitHub
5. The assigned team member(s) propose a first resolution of the issue (via pull request)
6. Pull request is reviewed by the team, and modified if necessary
7. The pull request is merged

***


## Week 2

This week we have 4 hours of lecture, where we covered technical debt.

We divided the work amoung the four of us and did the following :

- Jonathan analysed 5 opened issues (in [issue.md (branch issues-analysis)](https://github.com/delftswa2018/team-osu/blob/issues-analysis/issues.md)) and 5 closed pull requests (in [pullrequests.md (branch pull_requests-analysis)](https://github.com/delftswa2018/team-osu/blob/pull_requests-analysis/pullrequests.md) ), trying to pick some with discussions. He also added a Gantt diagram in the stakeholder to show the developpers contribution over time, and competitors to the Stakeholders file.

- Pavel worked on the [Stakeholders.md (branch Stakeholders-analysis)](https://github.com/delftswa2018/team-osu/blob/Stakeholder-analysis/Stakeholders.md) analysis, with help of the others. Most of the facts around stakeholders are aggregated, except for the administrators category : we are having a hard time figuring out who should we add in the category. The main content is mostly done, and there's still work on visualization and the Power-Influence grid to be done. (Maintainers : may count users that maintain their own beatmaps) (He Will probably deal with visualization on Saturday)

- Maiko started context view, writing the introduction and the system scope and responsibility. Soon, he will come with external entities and interface graph, and explanations that come with it. 

- Vincent started the analysis of closed issues, doing 4 and the last one will come this week-end.He also worked on C#. Very soon he will complete the analysis of 5 pull requests to finish these two analysis parts as soon as possible.

We need to refactor, or not use the tasks tracker in the hour tracker. It's neither useful, nor easy to keep track of things inside, considering everything (else) is going through GitHub. We will make a more intensive use of GitHub's Milestones instead.

We will now merge our changes in a branch called Deliverable 1, to make things clearer and use a tree-like structure (master being roots, where Deliverables are primary branches)


At the end of the week, the Stakeholders gained visualization with a PI grid made by Jonathan, Maiko finished the context view and got reviews from everyone. The issues analysis and pull requests analysis have been completed, reviewed by everyone, small changes or typos were fixed by their pull requests, and they were successfully merged to the Deliverable D1 branch.

***


## Week 3

This week we had 4 hours of lecture: 2 were dedicated to testing and refactoring of legacy code, and 2 were a guest lecture from Adyen, telling us how their system was architected.

During the start of the week, all team members contributed actively to the finishing of D1 by providing feedback and applying it.

After the completion of D1, we started working on D2. First, we put in place the same kind of branch and issues structure as for D1. We also created issue tags and milestones for D2.

Pavel initiated the work on D2 by browsing the open issues and making a selection of issues that seemed manageable at our level of knowledge of the project, and listed them in GitHub issues.
We had our weekly meeting on March 2nd, to work on D2. We started reviewing some of them, and looking at what we could do.
However, during that meeting, we noticed hat we had problems with the project repository: after recent modifications, the different versions we had were not functional anymore. This has later been resolved by cleaning the repository and merging it to upstream so that we were sure we had the proper version.

During the meeting, we decided on the following task repartition:

- Pavel and Maiko would work on fixing [issue #2022](https://github.com/ppy/osu/issues/2022)
- Vincent and Jonathan would work on the development view

The individual contributions of each team members are described below:

- Pavel contacted _peppy_, the lead developer of osu!, to introduce our team and ask pointers on potential contributions (as well as to avoid to waste efforts by working on something, only to discover during the pull request that something had been done as part of a bigger update). He also browsed the open issues once again and made a selection of issues that seemed manageable at our level of knowledge of the project, and listed them in GitHub.
Later, he spent a lot of time fixing [issue #2022](https://github.com/ppy/osu/issues/2022) together with Maiko. 

- Maiko reviewed the issues selected by Pavel in order to choose one to try and fix. He later spent a lot of time fixing [issue #2022](https://github.com/ppy/osu/issues/2022) together with Pavel. He became familiar with osu and its framework, and by consequence, he wrote for the development view some remarkable aspects and patterns he observed.

- Jonathan gathered information on what was important to have in the development view, as well as guidelines and pointers on it from Rozanski & Woods, and summarized everything on a file available for everyone in the DevelopmentView branch. 
He then spent a lot of time on researching and redacting the content of the development view with Vincent, and extract dependency graphs from Visual Studio.
It is important to note that he spent a significant amount of time fixing our repository issues discussed above, once to get hand on up-to-date code from the upstream repository, then a second time to get a clean repository to work on.

- Vincent took part in the review of issues available for fixing. He also kept studying C# to get up-to-speed with the codebase, despite a slight delay. His version of the code had particular problems for building, and they were fixed during the meeting.

He later spent a lot of time on researching and redacting the content of the development view with Jonathan, and organized the analysis in a "chapter-like" format.

Pavel later spent time on issues [#2022](https://github.com/ppy/osu/issues/2022), framework issue [#1444](https://github.com/ppy/osu-framework/issues/1444) and issue [#2180](https://github.com/ppy/osu/issues/2180). The first issue was discovered to be more complicated then it seems, and for the second issue, a pull request [#2173](https://github.com/ppy/osu/pull/2173) was created. However, although issue problem was identified correctly, the pull request was rejected due to inaccuracies in commit history. Our git skills and usage should be improved, so we do not trash project repositories with unnecessary commits. For the third issue, a pull request was created which was approved and merged to _osu!_ master branch. During this he also made himself familiar with osu! and osu-framework architetures. Details of attempts to make contribution are described in Contributions.md file.

***

## Week 4

This week, we have to produce the deliverable 2. It was first announced for Tuesday night, then was postponed to Friday night.

Everyone continued working on what they have been working on previously, and the gitbook structure was built on Tuesday evening. 
At the same time, we got our feedback for Deliverable 1, and since last Tuesday we did our best to take the feedback for way-of-working into account, to show a better organization and a cleaner repository.

Pavel finished the contribution.md, had the 3rd attempt for pull request validated, and he updated parts of the repository according to the feedback. He modified the README and created a accurate description in the wiki section.

Maiko finished working on the issue fixes. Then, he wrote a section about the different patterns he found while reading the code. He later reviewed the other parts of the deliverable, in particular the development view and the contribution.

Vincent finished the development view, and updated the pictures in it.

Jonathan built the gitbook structure (and learned a bit more about it). He also rebased the project repository, for good this time, learning a bit more about git too. He reviewed along with Maiko the development view and the design patterns section.


_Since the deadlines are now moved to Fridays, in order to describe best the workflow we are doing, we will now update the weekly journal on Friday._

During meeting we worked on Deliverable 2 and revisited our workflow. Most noticable points are:

- reducing parts of work. During working on the first deliverable we had relatively big work units. This kind of work distribution strategy was complicating reviewing process of each others' work and causing delays in processing pull requests. We assume that keeping work units smaller should speed up working process. For that purpose we need to increase utilisation of github features (issues, projects and so on)
- meetings should be described in more details. For that we should create a separate project for each meeting. We will assign issues to this project as an agenda and will keep more preciese time track.
- we worked on deliverable 2. After discussion, we concluded that our deliverable was ready to be handled. Just need to wrap up our hour tracker, but in general it looks good.

Time spent and more detailed results are described in closed project [Week 4 meeting agenda](https://github.com/delftswa2018/team-osu/projects/3?)


***

## Week 5

Jonathan started the week-end by writing a to-do list for the deliverable. Later on Tuesday, he started analysing the Visual Studio metrics, but he quickly dropped it to work on how to run SonarQube, where Pavel struggled. After some hours, he figured it out, and he wrote about SonarQube results.

During the meeting we worked on Deliverable 3, consisting mostly on reviewing and minor fixes. Jonathan was absent during the meeting for a personal trip. Still, he was available on Mattermost is case he was needed.

Pavel tried first to run SonarQube, before letting it to Jonathan. He then took the ReSharper module to run various analyses, and wrote a section on it. He also spent a significant amount of time reviewing various works during the whole week.

Vincent first wrote the analysis on CodeFactor. Then, he took back the Visual Studio metrics that Jonathan abandoned at a very early stage, and mostly did the whole of it. On Friday, he spent a lot of time in the afternoon reviewing and writing a section about social debt.

Maiko started late with his contributions due to another deadline, he managed to put some effort in the course but needs to gather up with the rest in the next weeks. He plans to start working this Sunday on implementing the first feedback. In the contributions he mostly reviewed the others' works and made the overview of the tool analysis and the test analysis.

On Friday night, everyone gathered to evict all typos, polish all the files, merge them in coherent branches, and build the gitbook PDF, in a several hours-long finalization phase.
