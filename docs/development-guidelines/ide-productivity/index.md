---
layout: default
title: IDE Productivity
nav_order: 3
parent: Development Guidelines
has_children: true
---

# IDE Productivity
{: .no_toc}

The more efficiently you can apply the mechanics for creating code, the more time you  have to do something else. "Something else" could be other project activities, like spending more time exploring the design options for a challenging feature. "Something else" could also just simply be finishing a project more quickly, enabling us to give more value to a client.

Every time you use a keyboard shortcut, you can potentially save around 2 seconds just by not having to take your hands off the keyboard, place it on the mouse to click on one or more UI widgets, and then move your hands back to the keyboard.

2 seconds may not sound like much, but even a small improvement leads to big gains on frequent actions. Several training and learning companies have done calculations or experiments showing how using shortcuts can save 40 to 60 hours a year!

[How keyboard shortcuts could revive America's economy](https://www.brainscape.com/blog/2011/08/keyboard-shortcuts-economy/)
[How You Can Save 40+ Hours From Your Life With Excel Keyboard Shortcuts](https://www.thekeycuts.com/save-hours-life-excel-keyboard-shortcuts/)

Who wouldn't want to free up over a week of boring mechanical work to apply more thinking to a design problem, learn something new, help a coworker, or just simply be done faster?

In addition to saving time, mastering the use of shortcuts during development allows you to concentrate on the software you're creating rather than the mechanics of writing code. Every move from the keyboard to the mouse is a context switch that takes your attention off of the problem at hand.

1. TOC
{: toc}

## Visual Studio vs. ReSharper

Around the same time Visual Studio was heavily modified to support .NET development in the early 2000's, JetBrains created a Visual Studio extension with many of the features from their Java IDE called IntelliJ IDEA. These features greatly increased the efficiency of developing within Visual Studio.

There are also a number of shortcuts for features that are purely within Visual Studio (e.g. code folding) that are worth knowing and using. This guide includes shortcuts for features in both Visual Studio and Resharper.

Every subsequent release of Visual Studio has added native implementations of Resharper features that closes the out-of-box gap between the two. However, the refined experience and power of ReSharper still surpasses that of Visual Studio alone, therefore we require all developers at Headspring to master the use of ReSharper shortcuts.

## Shortcut Schemes

### **CONSIDER** using the IntelliJ IDEA binding over the Visual Studio binding
{: .text-yellow-300 }
The initial versions of ReSharper shipped with a set of default keyboard bindings that paralleled their Java IDE. These have come to be known as the "IDEA" layout.

As Visual Studio has grown in functionality, a newer alternative binding set called "Visual Studio" layout was introduced to minimize conflicting shortcuts and gently do an in-place replacement of the competing Visual Studio functionality.

Now we've practically come full circle, with Visual Studio 2017 offering a Resharper keyboard mapping, for "those who used ReSharper in the past".

[Improving your productivity in the Visual Studio Editor](https://devblogs.microsoft.com/visualstudio/improving-your-productivity-in-the-visual-studio-editor/)

You are free to use whichever keybindings you like, but if you don't have a preference you should at least consider the IDEA layout. It is what is taught in our Career Start program, and also what many of our current developers were first exposed to and have grown in experience with. The IDEA layout has the additional benefit of being consistent across JetBrains products such as Rider or IntelliJ. If you find yourself using one of these tools, you won’t have to re-learn shortcuts.

### **DO** think in actions, not keystrokes
{: .text-green-100 }
Regardless of which keybindings you adopt, try hard to mentally think "goto type", not "Ctrl-T" or "Ctrl-N" in your daily use after you've initially learned them. This will help in conversations while pairing or assisting others.

## Approach

The features that ReSharper adds to the Visual Studio development experience are quite extensive and reach beyond just shortcuts. Attempting to learn many shortcuts at once can be daunting.

### **DO** learn shortcuts in small chunks
{: .text-green-100 }
We’ve found that breaking shortcuts up into logical chunks and focusing on incorporating 3 - 5 new shortcuts in to your development workflow each week is the best way to learn. In this document, we’ve captured some of the most useful shortcuts and demonstrate their use with an example.

The guide below breaks down some of the most useful shortcuts in to three groups. Start with the Basics, then move to Navigation and end with Refactoring. Spend about two weeks focusing on each section. A good benchmark to gauge your progress is to evaluate whether or not you can effectively code without picking up your mouse after learning the Basics and Navigation shortcuts.

### **CONSIDER** making a concentrated effort to apply shortcuts to your work by using refactoring techniques
{: .text-yellow-300 }
One of the best opportunities for practicing shortcuts is refactoring code. If you haven’t read them, the following are recommended books on the subject.

![Refactoring](/assets/images/ide-productivity/RefactoringBookCover.jpg)
![Working Effectively With Legacy Code](/assets/images/ide-productivity/WorkingEffectivelyWithLegacyCodeBookCover.jpg )

## Learning Guide

**Basics**
The shortcuts in this area are a great starting point when beginning to incorporate shortcuts in to your development workflow. These are some of the most frequently used and highest value in the list and are a combination of refactoring and navigation.

**Navigation**
These shortcuts will allow you traverse through your project faster.

**Refactoring**
Refactoring shortcuts allow you to quickly edit code. Common activities such as commenting code or extracting a method can be completed with just a few keystrokes. Mastering these shortcuts will drastically reduce the number of times you need to reach for a mouse, speeding up your development.

|Title|IDEA|Visual Studio|Group|Learning Order|
|---|---|---|---|---|
|Show ReSharper Options|`Alt+Enter`|`Alt+Enter`|Basics|01|
|Go to Everywhere|`Ctrl+N`|`Ctrl+T`|Basics|02|
|Go to Recent Locations|`Ctrl+E`|`Ctrl+,`|Basics|03|
|Go to Declaration|`Ctrl+B`|`F12`|Basics|04|
|Rename|`F2`|`Ctrl+R,R`|Basics|05|
|Show In Solution Explorer|`Shift+Alt+L`|`Shift+Alt+L`|Basics|06|
|Go to Usages|`Ctrl+Alt+F7`|`Shift+Alt+F12`|Navigation|07|
|Run All Tests In Solution|`Ctrl+T,L`|`Ctrl+U,L`|Navigation|08|
|Show Navigation Options|`Ctrl+Shift+G`|`` Alt+` ``|Navigation|09|
|Go to File Member|`Ctrl+F12`|`` Alt+\ ``|Navigation|10|
|Go to Derived Symbols|`Ctrl+Alt+B`|`Alt+End`|Navigation|11|
|Go to Previous Cursor Location|`Ctrl+-`|`Ctrl+-`|Navigation|12|
|Extend/Shrink Selection|`Ctrl+W` `Ctrl+Shift+W`|`Ctrl+Alt+Right` `Ctrl+Alt+Left`|Refactoring|13|
|Select Containing Declaration|`Ctrl+Shift+[`|`Ctrl+Shift+[`|Refactoring|14|
|Comment/Uncomment Line|`Ctrl+/`|`Ctrl+K,C`|Refactoring|15|
|Refactor This|`Ctrl+Shift+R`|`Ctrl+Shift+R`|Refactoring|16|
|Move|`F6`|`Ctrl+R,O`|Refactoring|17|
|Safe Delete|`Alt+Del`|`Ctrl+R,D`|Refactoring|18|
|Format Document|`Ctrl+K,D`|`Ctrl+K,D`|Refactoring|19|

## Cheat Sheets

The following cheat sheets, published by JetBrains, are a good resource to print out and keep visible at your desk as you learn shortcuts and begin to incorporate them in to your development.

[IntelliJ IDEA Scheme](https://www.jetbrains.com/resharper/docs/ReSharper_DefaultKeymap_IDEAscheme.pdf)

[Visual Studio Scheme](https://www.jetbrains.com/resharper/docs/ReSharper_DefaultKeymap_VSscheme.pdf)

[Visual Studio Default Shortcuts](http://visualstudioshortcuts.com/2017/)