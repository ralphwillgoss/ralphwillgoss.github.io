---
title: "Resharper 3.0.x not recognised under Visual Studio 2008 Beta 2"
date: 2007-08-12
categories:
  - geekswithblogs
tags:
  - resharper visual-studio
---

If you install Resharper 3.0.x and discover that Visual Studio 2008 Beta 2 hasn't recognised it, you need to run the install again with a few special command line arguments:

`C:\WINDOWS\system32\msiexec.exe /i <full path to installer file>.msi VSVERSION=9.`

{: .notice--primary}
<strong>Originally posted on:</strong>  
[geekswithblogs.net c/o archive.org](https://web.archive.org/web/20200804040303/http://geekswithblogs.net/rwillgoss/archive/2007/08/12/114600.aspx){:target="_blank"}
