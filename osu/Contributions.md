# Contributions

In this file we will describe our efforts to provide a contribution for osu! project.

## Getting started

Any new contributor must make himself familiar with the [Contribution section of Readme.md](https://github.com/ppy/osu/blob/master/README.md) and the [Development and testing section of wiki](https://github.com/ppy/osu-framework/wiki/Development-and-Testing) before working on the project. Having a standardized working process for every developer is a good thing: it ensures better communication and mutual understanding.

## Contributions

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
