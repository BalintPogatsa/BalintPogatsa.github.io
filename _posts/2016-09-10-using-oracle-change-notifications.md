---
layout: post
title: Using Oracle Change Notifications
date: 2016-09-10
---

Recently I've had the pleasure to work on a project which was using Oracle's [Database Change Notifications](https://docs.oracle.com/cd/B28359_01/win.111/b28375/featChange.htm "Database Change Notification") technology to subscribe to database events (like INSERT, UPDATE, DELETE) from a C# application.

![alt text](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/img/database_notification.png "Database notifications")

Here I would like to summarize my experiences about it:

  * Don't use it.
  * If you really want to use it, the reasons against:

      * It's antipattern (this should be enough reason against it, a DB is just not for this purpose)
      * It's not reliable:

          * If the connection is broken between your application and the database, your subscription will be removed by the DB. You can say that you check if the connection is broken and on reconnect you subscribe again. That's fine, until the connection gets broken in both directions. However if your application can reach the DB, but for some network issues the DB cannot connect to your application, Oracle just removes your subscription. 
          * You can still work around this by implementing some kind of watchdog functionality by subscribing to a dummy table and inserting values periodically, or querying the DB table USER_CHANGE_NOTIFICATION_REGS to see if your subscriptions are still valid. <br/>

  * So what should be the solution? I think you should move the notification logic to the application level. This is possible if you can implement a service which can be the only place where your DB table gets updated. In that case you can also trigger all events after your changes. Otherwise you are left with the workarounds to make this Database Change Notification feature reliable or implement a polling logic.

Note: These kind of issues about unreliable network connection are tipically difficult to reproduce. I've managed to do it with the help of a small application called [https://jagt.github.io/clumsy/](https://jagt.github.io/clumsy/ "clumsy"), which can simulate several network issues.

