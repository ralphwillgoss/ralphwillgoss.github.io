---
title: "When debugging using Visual Studio 2010 the aspnet_wp.exe is greyed out"
date: 2010-12-21
categories:
  - geekswithblogs
tags:
  - asp.net visual-studio
---

When debugging with Visual Studio 2010, you may find that you can't attach to the aspnet_wp.exe process as its greyed out.

One solution is to look at the Debug Diagnostic Services, it may be automatically starting and creating new instances of DbgHost.exe – leading VS to think a debugger was already attached.

Disabling this service should fix the problem of aspnet_wp.exe being greyed out.

Originally posted at http://geekswithblogs.net/rwillgoss/archive/2010/12/21/143188.aspx
