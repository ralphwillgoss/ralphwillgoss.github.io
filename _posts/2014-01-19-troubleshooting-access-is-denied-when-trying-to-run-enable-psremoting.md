---
title: "Troubleshooting 'Access is denied' when trying to run Enable-PSRemoting"
date: 2014-01-19
categories:
  - geekswithblogs
tags:
  - powershell
---

I recently had to trouble shoot why I was getting an "Access denied error" when trying to enable PSRemoting on hosted version of Windows 2008 R2.

The error received looked something like:
```powershell
WinRM Quick Configuration Running command "Set-WSManQuickConfig" to enable
this machine for remote management through WinRM service
This includes:
    1. Starting or restarting (if already started) the WinRM service
    2. Setting the WinRM service type to auto start
    3. Creating a listener to accept requests on any IP address
    4. Enabling firewall exception for WS-Management traffic (for http only).

Access is denied. + CategoryInfo : InvalidOperation: (:) [Set-WSManQuickConfig],
InvalidOperationException + FullyQualifiedErrorId : 
WsManError,Microsoft.WSMan.Management.SetWSManQuickConfigCommand
```

I discovered that my hosting company had applied a group policy on a firewall rule,
which is modified in the 4th step when trying to enabled PSRemoting:  
```4. Enabling firewall exception for WS-Management traffic (for http only)```

Steps to fix:  
1) Click on start menu  
    >> Administrative tools  
    >> Windows Firewall and Advanced security  

2) Click on inbound rules  
    >> New rule  
    >> choose the option "predefined" and select Windows Remote Management from the dropdown list  
    >> Click next  

3) Now, Deselect Windows Remote Management compatibility Mode(HTTP-In) and
    select Windows Remote Management Mode(HTTP-In)  
    >> Click Next  
    >> Allow the connection  
    >> Finish 

{: .notice--primary}
<strong>Originally Answered on:</strong>  
Stackoverflow  
[Unable to enable Powershell Remoting](http://stackoverflow.com/questions/10804639/unable-to-enable-powershell-remoting/21207458#21207458)  
[Powershell enable-psremoting for remote machine](http://stackoverflow.com/questions/10332097/powershell-enable-psremoting-for-remote-machine/21207477#21207477)  
<br/>
Server Fault  
[Enabling Powershell Remoting, Access is denied?](http://serverfault.com/questions/337905/enabling-powershell-remoting-access-is-denied/568228#568228)  
[Enable-PSRemoting on Windows Server 2008 R2 error](http://serverfault.com/questions/530903/enable-psremoting-on-windows-server-2008-r2-error/568229#568229)  
<br/>
<strong>Originally posted on:</strong>  
[geekswithblogs.net c/o archive.org](https://web.archive.org/web/20200814113120/http://geekswithblogs.net/rwillgoss/archive/2014/01/19/155231.aspx){:target="_blank"}