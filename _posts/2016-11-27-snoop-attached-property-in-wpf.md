---
layout: post
title: Snoop Attached Property in WPF
date: 2016-11-27
---

[Snoop, the WPF Spy Utility](https://snoopwpf.codeplex.com/ "Snoop, the WPF Spy Utility") is an excellent (must have) tool for WPF application developers. 
It's most used feature is to check the values of properties of WPF controls.

You just start your application and Snoop, you drag the second crosshair over the window of your application, 
you move your cursor over the control you want to snoop, you press CTRL + SHIFT and violá.

You see the visual tree of your application and the dependency properties of the selected control.
You can also modify their values and see the immediate effect.

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/snoop_attached_propery_1.png "Snoop spying screen")

Sometimes you also need to check values of some attached properties set on a control.
Unfortunately the actual version of Snoop (2.8.0) displays only attached properties from the Grid control.
(Of course this limitation makes sense.) So how can you overcome this issue?

There is a less used feature of Snoop that you can execute PowerShell commands if you navigate to the last tab of the application.

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/snoop_attached_propery_2.png "PowerShell tab")

Here, if you want to check i.e. the Canvas.Top attached property value, you can just choose the control from the tree and type:

    [System.Windows.Controls.Canvas]::GetTop($selected.Visual)
	
The same trick works also if you've implemented your own attached property:

    [WpfBasics.MyAttachedProperty]::GetFoo($selected.Visual)
	
Similarly, you can edit the values of these attached properties too:

    [WpfBasics.MyAttachedProperty]::SetFoo($selected.Visual, "test")
	
Besides that, you can achieve many different tasks with this PowerShell. See some examples [here](http://blog.scottlogic.com/2013/12/18/wpf-snoop-powershell.html "here")

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/snoop_attached_propery_3.png "Using the PowerShell tab")