---
layout: default
title: Basics
parent: IDE Productivity
grand_parent: Development Guidelines
nav_order: 1
---

# Basics
{:.no_toc}

The shortcuts in this area are a great starting point when beginning to incorporate shortcuts in to your development workflow. These are some of the most frequently used and highest value in the list and are a combination of refactoring and navigation.

1. TOC
{:toc}

## Show ReSharper Options

The ReSharper Options shortcut activates the actions indicator to show available options. These change greatly based on context.

![](/assets/images/ide-productivity/ShowReSharperOptions.gif)

## Go to Everywhere

Go To Everywhere allows you to search for types, classes, symbols, files, and string literals. It shows recent files you have used when you initiate it, and then adapts to what you’re typing.

![](/assets/images/ide-productivity/GoToEverywhere.gif)

## Go to Recent Locations

Go to Recent Locations displays a list of recently accessed files in Visual Studio. Results are displayed in the order in which the files have been accessed. Pressing Enter immediately after executing the Go to Recent Locations shortcut, will quickly take you back to the file previously being viewed.  Similar to other ReSharper shortcut windows, the Recent Files window allows you to search within the results and supports quick searches by typing the first letter of each word in the file name.

![](/assets/images/ide-productivity/GoToRecentLocations.gif)

## Go to Declaration

When your cursor is on a symbol usage this will take you to its declaration. You can move the other way (find usages of the class/symbol declaration), by selecting its declaration and pressing `Ctrl+B`.

![](/assets/images/ide-productivity/GoToDeclaration.gif)

## Rename

The Rename shortcut will allow you to safely refactor C# entities like namespaces, types, methods, fields, properties, parameters, local variables, events, and delegates. It will update all references to the entities during the rename. Be aware, this does not affect the database or other outside scripts like javascript.

![](/assets/images/ide-productivity/Rename.gif)

## Show In Solution Explorer

This shortcut displays the file you have open currently in the solution explorer or opens its assembly if it is from an external library. This can be useful if you need to find related files in a project with feature folders.

![](/assets/images/ide-productivity/ShowInSolutionExplorer.gif)