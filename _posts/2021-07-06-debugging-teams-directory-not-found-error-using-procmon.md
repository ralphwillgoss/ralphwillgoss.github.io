---
title: "Debugging Microsoft Teams and a directory not found error using Procmon"
date: 2021-07-06
categories:
  - microsoft teams windows10
tags:
  - microsoft teams windows10
---
The following is a guide on how I debugged a directory not found error, which happened to me just after an update to Microsoft Teams.
This process can be used with any Windows application, where you are getting file or directory errors.  

# Problem
I currently run [mulitple instances of Microsoft Teams]({% post_url 2020-12-20-how-do-you-run-multiple-instances-of-microsoft-teams %}).  
This was working well until a recent update to Teams caused something to brake.  

I received an error that the `downloads` directory did not exist, however there was no information about the expected path:  
```
Uncaught Exception:
Error: Failed to get 'downloads' path
  at Object.<anonymous>
```
![Error Dialogue](/assets/images/posts/directory-not-found/error-dialogue.png)  

Checking the log path for Teams `C:\Users\ralph\AppData\Roaming\Microsoft\Teams`, didn't reveal anything.  
So how do we work out the path Microsoft Teams is trying to access, if its never displayed or logged?

# Solution
In order to work out the path Microsoft Teams is trying to access I'll use [Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) (also known as `Procmon`).  
`Procmon` is part of the [Sysinternals Utilities](https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite).  
Download the suite and install so the utilities are available on your windows `PATH`.  
I have a `C:\utils` folder which is part of my `PATH` and I put these types of tools there.

## Procmon Setup
We will need to Start `Procmon` and configure it so that it:
- captures events for the process I'm trouble shooting 
- only captures activity related to the file system

Start `Procmon` and then stop it from capturing events:
![Procmon start/stop](/assets/images/posts/directory-not-found/procmon-start.png) 

Then, we will clear all the events that have been captured:
![Procmon clear events](/assets/images/posts/directory-not-found/procmon-clear.png) 

For this example we are only interested in file system activity, so we will enable only that filter:
![Procmon - enable file system activity](/assets/images/posts/directory-not-found/procmon-filesystem-activity.png) 

Select **Filter** from the top menu and modify the filter settings so that I limit captured file events to the `Teams` process.  
I also know that the path will contain the word `download` which was displayed in the error message, so I'll add a filter for that too.  

So for my example the `Procmon` filter looks like this:  
![Process Monitor - filter settings](/assets/images/posts/directory-not-found/process-monitor-filter-settings.png)

Before starting the application ensure all events from `Procmon` have been cleared and `Procmon` is set to capture activity again.  
Starting Microsoft Teams again, `Procmon` will now capture file events which were not successful and have the keyword I specified in the **Path**.

Below you can see the `Procmon` output I captured and the path to the directory which Teams was trying to access:  
![Process Monitor - directory not found error](/assets/images/posts/directory-not-found/process-monitor-directory-not-found.png)  

I created a `Downloads` folder in my custom profiles location and Microsoft Teams loaded successfully.  
Checkout [Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) docs for more on what this great tool can do.

{: .notice--primary}
<strong>References:</strong>  
[Procmon](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon){:target="_blank"}  
[Sysinternals Suite](https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite){:target="_blank"}  
[How do you run mulitple instances of Microsoft Teams?]({% post_url 2020-12-20-how-do-you-run-multiple-instances-of-microsoft-teams %}){:target="_blank"}
