---
layout: post
title: Mass rename class names in your project
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
- Renaming all occurences in code and references in project files 
   
In Notepad++ press Ctrl+Shift+F to open Find in Files tool.
In the opened dialog choose your project directory, set Regular expression as search mode, set file filters (c*.cs) and specify the regular expression
If you first click "Find All", you can review what changes you will make

> Find what: ([\W+])[Cc]([A-Z]\w+)([\W+])
> 
> Replace with: $1$2$3


![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/step0_mass_rename_prefixed_class.png "Replace class names in source code")
   
Repeat quite similarly for project files (*.csproj)

> Find what: ([\W+])[Cc]([A-Z]\w+\.cs)([\W+])
> 
> Replace with: $1$2$3

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/step1_mass_rename_prefixed_class.png "Replace file references in project files")
   
- Renaming files
   
Use Find Files tool of Total Commander to list all the files that you need to rename and click "Feed to listbox" button
![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/step2_mass_rename_prefixed_class.png "Find files to rename")
   
Select all files and open Multi-Rename Tool (Ctrl+M)
In the dialog I was using a regular expression to match files starting with "c" or "C" and the rest of the filename beginning with a capital letter.
In that regular expression if you specify a group in curly brackets, you can refer to it by it's number in the Replace with field.
   
> Search for: [cC]([A-Z].*\.cs)
> 
> Replace with: $1
    
You can also preview the changes to be sure you rename everything as you planned.
![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/step3_mass_rename_prefixed_class.png "Rename files")
   
## Change postfix of class names
The steps are pretty similar to removing prefixes so I will just show the steps you need
In my case I will search for "VM" postfix and replace it with "ViewModel"

- Renaming all occurences in code and references in project files 
   
Notepad++ - Find in Files tool (Ctrl+Shift+F).
File filters (*VM.cs)
   
> Find what: ([\W+])([A-Z]\w+)VM([\W+])
> 
> Replace with: $1$2ViewModel$3

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/step0_mass_rename_postfixed_class.png "Replace class names in source code")
   
For project files (*.csproj)

> Find what: ([\W+])([A-Z]\w+)VM(\.cs[\W+])
> 
> Replace with: $1$2ViewModel$3

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/step1_mass_rename_postfixed_class.png "Replace file references in project files")
   
- Renaming files
   
Total Commander - Find files tool (Ctrl+F) with *VM.cs filter - Feed to listbox
Select all files and open Multi-Rename Tool (Ctrl+M)
   
> Search for: (\.*)VM(\.cs)
> 
> Replace with: $1ViewModel$2
    
![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/step3_mass_rename_postfixed_class.png "Rename files")
   
## Change postfix of WPF controls
In this case I will search for "V" postfix and replace it with "View"

- Renaming all occurences in code and references in project files 
   
Notepad++ - Find in Files tool (Ctrl+Shift+F).
File filters (*V.xaml *V.xaml.cs)
   
> Find what: ([\W+])([A-Z]\w+)V([\W+])
> 
> Replace with: $1$2View$3

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/step0_mass_rename_postfixed_wpf_control.png "Replace class names in source code")
   
For project files (*.csproj)

> Find what: ([\W+])([A-Z]\w+)V(\.xaml\W+)
> 
> Replace with: $1$2View$3

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/step1_mass_rename_postfixed_class.png "Replace file references in project files")
   
- Renaming files
   
Total Commander - Find files tool (Ctrl+F) with *V.xaml* filter - Feed to listbox
Select all files and open Multi-Rename Tool (Ctrl+M)
   
> Search for: (\.*)V(\.xaml(\.cs)*)
> 
> Replace with: $1View$2
    
![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/step3_mass_rename_postfixed_wpf_control.png "Rename files")   