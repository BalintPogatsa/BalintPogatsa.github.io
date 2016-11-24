---
layout: post
title: Restrict user to single app on Windows
date: 2016-11-24
---

Let's say you have a requirement, that in the production environment, the user should be restricted to access only the Windows desktop application you provide.
The user shouldn't be able to open i.e. a browser or change any system settings or access the task manager.
This kind of functionality is referred to many times as kiosk mode. 
(I.e. Google Chrome has a built in kiosk mode, which can restrict the user by starting it with the command line argument: --kiosk)

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/kiosk.jpg "Kiosk computer")

One way to achieve this is to modify system settings from your application (like modifying registry entries, hiding task bar, etc.).
Which can be pretty tricky and the solution doesn't seem very robust.

In my opinion a much better way to achieve this is to configure a specific Windows user account, where:
- task manager is disabled
- task bar is disabled
- the application starts up when the user logs in
- the user gets logged out when the application closes
 
Luckily this is not a complicated task at all.
The task manager can be disabled, by setting one registry entry (you can find an article, [how to edit registry entries of another user](http://www.ghacks.net/2008/03/12/windows-tip-edit-user-registry-of-other-users/ "how to edit registry entries of another user")):

        HKEY_USERS\ HKEY_[name of the user]_USER\Software\Microsoft\Windows\CurrentVersion\Policies\System
        
        Create DisableTaskMgr dword value under System key -> Set the value to 1

One possible solution to disable the task bar and also make the application start on login is to replace the default shell application (explorer.exe) with your application.

So the only requirement left is to log out the user after the app closes. The best way I've found for this is to use a small vbs script, which
- starts the application
- waits until it's execution is finished
- logs off the user
 
        'logoffAfterClosed.vbs
 
        set oShell=createobject("wscript.shell")
        sCmd=WScript.Arguments(0)
        oShell.run sCmd,1,true 'true forces it to wait for process to finish
        sCmd="shutdown /l"
        oShell.run sCmd,0,false '0 makes the command line hidden

If you set this script as the shell application, all of the mentioned requirements are set:

        HKEY_USERS\ HKEY_[name of the user]_USER\Software\Microsoft\Windows NT\CurrentVersion\Winlogon
        
        c:\Windows\System32\wscript.exe d:\logoffAfterClosed.vbs d:\MyDesktopApp.exe

I've tried this solution on Windows 7, but it should work on other Windows operating systems too.