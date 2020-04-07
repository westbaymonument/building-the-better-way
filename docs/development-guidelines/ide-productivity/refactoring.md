---
layout: default
title: Refactoring
parent: IDE Productivity
grand_parent: Development Guidelines
nav_order: 3
---

# Refactoring
{:.no_toc}

Refactoring shortcuts allow you to quickly edit code. Common activities such as commenting code or extracting a method can be completed with just a few keystrokes. Mastering these shortcuts will drastically reduce the number of times you need to reach for a mouse, speeding up your development.

1. TOC
{:toc}

# Extend / Shrink Selection

The extend selection shortcut allows you to select progressively larger blocks of code in a logical succession starting from the location of the cursor.

![](/assets/images/ide-productivity/ExtendShrinkSelection.gif)

# Select Containing Declaration

Similar to the Extend / Shrink Selection shortcut, the Select Containing Declaration shortcut allows you to quickly select successively larger blocks of code. Select Containing Declaration will immediately select the entire member based on the location of your cursor.

![](/assets/images/ide-productivity/SelectContainingDeclaration.gif)

# Comment / Uncomment Line

The comment / uncomment line shortcut allows you to quickly toggle line comments for a single line or a block of code. Quickly comment a block of code by first using the Extend / Shrink Selection shortcut and then Comment Line shortcut.

![](/assets/images/ide-productivity/CommentUncommentLine.gif)

# Refactor This

ReSharper provides numerous refactoring shortcuts such as Extract Method or Inline Variable. While each of these are valuable to incorporate in to your development workflow, a quick way to begin using them is by starting with Refactor This. Refactor This is a quick way to show all ReSharper refactoring options based on the location of your cursor.

![](/assets/images/ide-productivity/RefactorThis.gif)

# Move

The Move shortcut can be used to quickly move objects such as classes, types and methods and is contextual based on your selection. The primary benefit to using the Move shortcut is that ReSharper will automatically update Namespace declarations in the original file as well as all usages when moving Types.

![](/assets/images/ide-productivity/Move.gif)

# Safe Delete

The Safe Delete shortcut allows you to delete types or entire projects / assemblies. When using Safe Delete, ReSharper will examine the solution to ensure there are no existing references. If any do exist, ReSharper will display the list of references which will be broken if the delete operation is continued. Using Safe Delete will help guarantee that the solution continues to compile after the operation is completed. This is an excellent shortcut to use when cleaning up legacy or unused code.

![](/assets/images/ide-productivity/SafeDelete.gif)

# Format Document

Format Document is a Visual Studio shortcut that formats the entire open document. It fixes whitespace and indentation issues.

![](/assets/images/ide-productivity/FormatFoducment.gif)
