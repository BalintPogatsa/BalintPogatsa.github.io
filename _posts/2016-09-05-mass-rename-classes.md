---
layout: post
title: Mass rename classes in your project
date: 2016-09-05
---

Let's say you find yourself in a project where your predecessors were using an obsolate/wrong naming convention.
For example somebody was prefixing classes with "C" (typical for legacy code):

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/mass_rename_prefixed_class.png "Classes with undesirable prefix")

Or postfixing MVVM views and view models with V/VM instead of View/ViewModel:

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/mass_rename_postfixed_class.png "Classes with undesirable postfix")

Prefixes can be especially annoying as it takes much longer for your eyes to find the source file in a folder that you are looking for.

So what are your options if you have a lot of names that you would like to fix? 
Unfortunately there is no possibility to rename many files in Visual Studio using regular expressions (there is even no option for that in ReSharper), so you need to do it by another tool.
As you have many files, you really don't want to rename every class one by one.

Luckily in [Total Commander](https://www.ghisler.com/) there is a Multi-Rename Tool, that you can open by selecting the files that you want to rename and pressing Ctrl+M.
Besides that you can use [Notepad++](https://notepad-plus-plus.org/) to rename the content of multiple files.
With these two tools you can easily fix all those naming problems.

I will describe 3 different cases:

  * Removing prefix from class names (i.e. cOldPrefixedClass.cs)
  * Change postfix of class names (i.e. FirstVM.cs)
  * Change postfix of WPF controls (i.e. FirstV.xaml and FirstV.xaml.cs)

## Removing prefix from class names
<span class="step">1</span>To rename all occurences in code, open Notepad++, press Ctrl+Shift+F to open Find in Files tool.
In the opened dialog choose your project directory, choose "Regular expression" as search mode, set file filters to "c*.cs" and specify your RegExp.
If you first click "Find All", you can review what changes you will make.

    Find what: ([\W+])[Cc]([A-Z]\w+)([\W+])
    
	// Here you refer to the groups from the RegExp which were specified inside curly brackets.
    Replace with: $1$2$3


![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/step0_mass_rename_prefixed_class.png "Replace class names in source code")
   
<span class="step">2</span>Repeat quite similarly for project files (*.csproj).

    Find what: ([\W+])[Cc]([A-Z]\w+\.cs)([\W+])
    
    Replace with: $1$2$3

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/step1_mass_rename_prefixed_class.png "Replace file references in project files")
   
<span class="step">3</span>To rename files, first in Total Commander use Find Files tool to list all the files that you need to rename and click "Feed to listbox" button:

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/step2_mass_rename_prefixed_class.png "Find files to rename")
   
Select all files and open Multi-Rename Tool (Ctrl+M).
In the dialog I was using a regular expression to match files starting with "c" or "C" and the rest of the filename beginning with a capital letter.

    Search for: [cC]([A-Z].*\.cs)
    
    Replace with: $1

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/step3_mass_rename_prefixed_class.png "Rename files")
   
## Change postfix of class names
The steps are pretty similar to removing prefixes so I will just show the steps you need.
In my case I will search for "VM" postfix and replace it with "ViewModel".

<span class="step">1</span>To rename all occurences in code, go to Notepad++ - Find in Files tool (Ctrl+Shift+F).
Then set file filters to "*VM.cs" and replace occurences.
   
    Find what: ([\W+])([A-Z]\w+)VM([\W+])
    
    Replace with: $1$2ViewModel$3

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/step0_mass_rename_postfixed_class.png "Replace class names in source code")
   
<span class="step">2</span>For project files set file filters to "*.csproj" and replace.

    Find what: ([\W+])([A-Z]\w+)VM(\.cs[\W+])
    
    Replace with: $1$2ViewModel$3

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/step1_mass_rename_postfixed_class.png "Replace file references in project files")
   
<span class="step">3</span>To rename files open Total Commander - Find files tool (Ctrl+F) with "*VM.cs" filter - Feed to listbox.
Then select all files and open Multi-Rename Tool (Ctrl+M).
   
    Search for: (\.*)VM(\.cs)
    
    Replace with: $1ViewModel$2
    
![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/step3_mass_rename_postfixed_class.png "Rename files")
   
## Change postfix of WPF controls
In this case I will search for "V" postfix and replace it with "View".

<span class="step">1</span>To rename all occurences in code open Notepad++ - Find in Files tool (Ctrl+Shift+F), set file filters to "*V.xaml *V.xaml.cs" and replace occurences.
   
    Find what: ([\W+])([A-Z]\w+)V([\W+])
    
    Replace with: $1$2View$3

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/step0_mass_rename_postfixed_wpf_control.png "Replace class names in source code")
   
<span class="step">2</span>For project files set file filters to "*.csproj".

    Find what: ([\W+])([A-Z]\w+)V(\.xaml\W+)
    
    Replace with: $1$2View$3

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/step1_mass_rename_postfixed_class.png "Replace file references in project files")
   
<span class="step">3</span>To rename files, open Total Commander - Find files tool (Ctrl+F) with "*V.xaml*" filter, then click "Feed to listbox" button. After that select all files and open Multi-Rename Tool (Ctrl+M).
   
    Search for: (\.*)V(\.xaml(\.cs)*)
    
    Replace with: $1View$2
    
![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/step3_mass_rename_postfixed_wpf_control.png "Rename files")

