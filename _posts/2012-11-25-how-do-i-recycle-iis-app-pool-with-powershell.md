---
title: "How do I recycle an IIS App pool with Powershell?"
date: 2012-11-25
categories:
  - geekswithblogs
tags:
  - powershell
---

Reference implementation of a Powershell script to recycle app pools, in response to Rick's post:  
<http://www.west-wind.com/weblog/posts/2012/Oct/02/A-tiny-Utility-to-recycle-an-IIS-Application-Pool>{:target="_blank"}

``` powershell
#    File: RecycleAppPool.ps1
#    Author: Ralph Willgoss
#    Date: 2nd Oct 2012
#    Reference:
#    http://stackoverflow.com/questions/198623/how-do-i-recycle-an-iis-apppool-with-powershell
# =============================================================================
#    Iniatialise
=============================================================================
param ( )

=============================================================================
#   Main
=============================================================================

Write-OutPut ""
Write-OutPut "Starting Recycling App Pool"
Write-OutPut ""

$appPoolName = "AppPoolName" #$args[0]
$appPool = Get-WmiObject -namespace "root\MicrosoftIISv2" -class "IIsApplicationPool"
          | Where-Object { $_.Name -eq "W3SVC/APPPOOLS/$appPoolName" }
          
$appPool.Recycle()

Write-OutPut ""
Write-OutPut "Finished Recycling App Pool"
Write-OutPut ""
```

Alternatives:

``` powershell
# Windows 2003 & II6
C:\WINDOWS\system32>cscript.exe iisapp.vbs /a AppPoolName /r
```

``` powershell
# Windows 2008 IIS7
C:\WINDOWS\system32\inetsrv\appcmd recycle apppool "MyAppPool"
```

``` powershell
# cmdlet
Restart-WebAppPool
```
[Restart-WebAppPool cmdlet](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee790580(v=technet.10)){:target="_blank"}


{: .notice--primary}
<strong>Originally posted on:</strong>  
[geekswithblogs.net c/o archive.org](https://web.archive.org/web/20200808192711/http://geekswithblogs.net/rwillgoss/archive/2012/11/25/how-do-i-recycle-an-iis-app-pool-with-powershell.aspx){:target="_blank"}