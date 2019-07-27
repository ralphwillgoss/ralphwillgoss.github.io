---
layout: post
title: "Troubleshooting 'Access is denied' when trying to run Enable-PSRemoting"
date: 2014-01-19
---

I recently had to trouble shoot why I was getting an "Access denied error" when trying to enable PSRemoting on hosted version of Windows 2008 R2.

The error received looked something like:
```
WinRM Quick Configuration Running command "Set-WSManQuickConfig" to enable
this machine for remote management through WinRM service
This includes:
    1. Starting or restarting (if already started) the WinRM service
    2. Setting the WinRM service type to auto start
    3. Creating a listener to accept requests on any IP address
    4. Enabling firewall exception for WS-Management traffic (for http only).
```

```
Access is denied. + CategoryInfo : InvalidOperation: (:) [Set-WSManQuickConfig],
InvalidOperationException + FullyQualifiedErrorId : 
WsManError,Microsoft.WSMan.Management.SetWSManQuickConfigCommand
```

I discovered that my hosting company had applied a group policy on a firewall rule,
which is modified in the 4th step when trying to enabled PSRemoting:<br/>
```4. Enabling firewall exception for WS-Management traffic (for http only)```

Steps to fix:<br/>
1) Click on start menu<br/>
    >> Administrative tools<br/>
    >> Windows Firewall and Advanced security<br/>

2) Click on inbound rules<br/>
    >> New rule<br/>
    >> choose the option "predefined" and select Windows Remote Management from the dropdown list<br/>
    >> Click next<br/>

3) Now, Deselect Windows Remote Management compatibility Mode(HTTP-In) and
    select Windows Remote Management Mode(HTTP-In)<br/>
    >> Click Next<br/>
    >> Allow the connection<br/>
    >> Finish<br/>

Here's a list of useful resources for debugging this, which I used before I discovered the above fix:<br/>

Trouble Shooting References:<br/>
Check that your account is in the Local Administrators group: ```powershell> whoami /all```

[Powershell team - Enable PSRemoting](http://blogs.msdn.com/b/powershell/archive/2009/04/30/enable-psremoting.aspx) (4 common trouble shooting steps)<br/>
[How to run powershell commands on remote computers](http://www.howtogeek.com/117192/how-to-run-powershell-commands-on-remote-computers) (Domain vs Workgroup setup)<br/>

Other Topics:<br/>
Trying to setup PSRemoting on SharePoint?<br/>
[Using PowerShell remoting technologies to manage a SharePoint farm](http://blogs.msdn.com/b/besidethepoint/archive/2010/05/26/powershell-remoting-for-sharepoint.aspx)<br/>

Trying to setup PSRemoting on Windows XP?<br/>
[Error enabling PSRemoting in Windows XP SP3](https://social.technet.microsoft.com/Forums/windowsserver/en-US/e1eabe4c-0796-420e-b03e-dffc71760b8d/error-enabling-remoting-in-winxp-sp3?forum=winserverpowershell&ranMID=24542&ranEAID=TnL5HPStwNw&ranSiteID=TnL5HPStwNw-jdIGdLg50bgZliUk5iQscw&epi=TnL5HPStwNw-jdIGdLg50bgZliUk5iQscw&irgwc=1&OCID=AID2000142_aff_7593_1243925&tduid=(ir__ybhseollh0kfrlt1kk0sohz30m2xjikpgw6ccuiu00)(7593)(1243925)(TnL5HPStwNw-jdIGdLg50bgZliUk5iQscw)()&irclickid=_ybhseollh0kfrlt1kk0sohz30m2xjikpgw6ccuiu00)<br/>

Are you setting up PSRemoting on a non English computer? - change it to English<br/>
[Reference 1](http://go.skimresources.com/?id=51334X1255744&xs=1&isjs=1&url=http%3A%2F%2Fsocial.technet.microsoft.com%2FForums%2Fscriptcenter%2Fen-US%2Fa1132146-575b-43c5-bcf0-fe14ad4bbb6a%2Fenablepsremoting-fails-access-denied&xguid=01DGFDC89BAZ9WS1KEMYR16TQJ&xuuid=58a982a0103b8921bc93df804b42d735&xsessid=&xcreo=0&xed=0&sref=http%3A%2F%2Fgeekswithblogs.net%2Frwillgoss%2Farchive%2F2014%2F01%2F19%2F155231.aspx&pref=http%3A%2F%2Fgeekswithblogs.net%2Frwillgoss%2Farchive%2F2014%2F01.aspx&xtz=0&jv=3.21.7-stackpath&bv=2.5.1) - lanuage not specified<br/>
[Reference 2](https://social.technet.microsoft.com/Forums/windowsserver/en-US/e1eabe4c-0796-420e-b03e-dffc71760b8d/error-enabling-remoting-in-winxp-sp3?forum=winserverpowershell&ranMID=24542&ranEAID=TnL5HPStwNw&ranSiteID=TnL5HPStwNw-.X9uCtnjnNkZYWPVoHrlIg&epi=TnL5HPStwNw-.X9uCtnjnNkZYWPVoHrlIg&irgwc=1&OCID=AID2000142_aff_7593_1243925&tduid=(ir__ybhseollh0kfrlt1kk0sohz30m2xjikpi96ccuiu00)(7593)(1243925)(TnL5HPStwNw-.X9uCtnjnNkZYWPVoHrlIg)()&irclickid=_ybhseollh0kfrlt1kk0sohz30m2xjikpi96ccuiu00) - Spanish/German<br/>

Are you using VirtualBox and trying to setup PSRemoting?<br/>
[Virtualbox and needing to enable enable CredSSP](http://werepoint.blogspot.co.uk/2012/03/setting-up-ps-remoting-for-all-that.html)<br/>
[VirtualBox and setting up PSRemoting gets access denied error](http://powershell.com/cs/forums/t/8167.aspx)<br/>

Originally Answered on:<br/>
Stackoverflow<br/>
[Unable to enable Powershell Remoting](http://stackoverflow.com/questions/10804639/unable-to-enable-powershell-remoting/21207458#21207458)<br/>
[Powershell enable-psremoting for remote machine](http://stackoverflow.com/questions/10332097/powershell-enable-psremoting-for-remote-machine/21207477#21207477)<br/>

Server Fault<br/>
[Enabling Powershell Remoting, Access is denied?](http://serverfault.com/questions/337905/enabling-powershell-remoting-access-is-denied/568228#568228)<br/>
[Enable-PSRemoting on Windows Server 2008 R2 error](http://serverfault.com/questions/530903/enable-psremoting-on-windows-server-2008-r2-error/568229#568229)<br/>

Originally posted on http://geekswithblogs.net/rwillgoss/archive/2014/01/19/155231.aspx