---
title: "How do you run multiple instances of Microsoft Teams?"
date: 2020-12-20
categories:
  - remote
tags:
  - microsoft teams windows10
---

Sometimes you may need to be run two or more instances of Microsoft Teams, as you have multiple accounts that you need to be logged into at the same time.
I found various solutions but they all fell short for me.
The hybrid nature of the solutions, where you run an instance of the Teams app and an instance of Teams in a browser - means you miss out on the video features, which is the main reason I use teams.

After some searching I found a blog post by [Daniel Smon](https://danielsmon.com), on how you can [run multiple instances of teams on Windows 10](https://danielsmon.com/2020/04/02/multiple-ms-teams-accounts-on-the-desktop).  
This built on a script that [Francois Hill](https://microsoftteams.uservoice.com/users/941189995-francois-hill) made, who added a [User Voice to use Multiple Teams Accounts at the same time](https://microsoftteams.uservoice.com/forums/555103-public/suggestions/17750851-i-want-to-use-multiple-teams-accounts-at-the-same).

This feature is being worked on and is due [Feb 2021](https://www.microsoft.com/en-gb/microsoft-365/roadmap?searchterms=68845&rtc=1&filters=&searchterms=68845) according the Microsoft Teams road map.

## <strong>Script</strong> 
This script allows you to have many instances of the Teams app running at once with the full functionality in all. Each instance has its own state persistence, so it will remember the Teams account you last selected and any customisations you've done for that account, e.g. backgrounds.

I modified the original so that all my Teams profiles are stored in the same place, which is `%APPDATA%`  
The original script stored the profile in `%LOCALAPPDATA%\Microsoft\Teams\CustomProfiles\`  

```bat
`The A Team.cmd`

@ECHO OFF

REM Uses the file name as the profile name
SET MSTEAMS_PROFILE=%~n0
ECHO - Using profile "%MSTEAMS_PROFILE%"

SET "OLD_USERPROFILE=%USERPROFILE%"
SET "USERPROFILE=%APPDATA%\Microsoft\Teams_%MSTEAMS_PROFILE%"

ECHO - Launching MS Teams with profile %MSTEAMS_PROFILE%
cd "%OLD_USERPROFILE%\AppData\Local\Microsoft\Teams"
"%OLD_USERPROFILE%\AppData\Local\Microsoft\Teams\Update.exe" --processStart "Teams.exe"
```
If your interested in the difference between `%APPDATA%` and `%LOCALAPPDATA%` [read here](https://www.thewindowsclub.com/local-localnow-roaming-folders-windows-10).


## <strong>System Requirements</strong> 
Its worth mentioning that Teams is an [Electron App](https://www.electronjs.org/), so it can be resource hungry - particulary on lower end machines.   
You'll struggle to run two instances of Teams on a 4GB, 4 core laptop.  

I've noticed that you need about 500MB of memory for each idle instance, this will increase to about 750MB-1GB when video calling or being in video meetings with numerous people. Teams will take advantage of GPU's, an integrated GPU works well.

{: .notice--primary}
<strong>References:</strong>  
<https://microsoftteams.uservoice.com/forums/555103-public/suggestions/17750851-i-want-to-use-multiple-teams-accounts-at-the-same>{:target="_blank"}  
<https://www.microsoft.com/en-gb/microsoft-365/roadmap?searchterms=68845&rtc=1&filters=&searchterms=68845>  
<https://www.electronjs.org/>  
<https://www.linkedin.com/pulse/run-multiple-instances-ms-teams-denis-molodtsov/>{:target="_blank"}  
<https://www.sharepointeurope.com/how-to-run-multiple-instances-of-microsoft-teams-using-microsoft-edge/>{:target="_blank"}  
<https://blogs.perficient.com/2020/04/22/how-to-open-multiple-instances-of-microsoft-teams/>{:target="_blank"}  
<https://www.risual.com/2020/04/instances-of-microsoft-teams/>{:target="_blank"}