---
title: "How to fix camera brightness in Microsoft Teams"
date: 2020-11-30
categories:
  - remote 
tags:
  - microsoft teams windows10
---

I recently had a problem using my Microsoft LifeCam Cinema WebCam in Microsoft Teams.  
I would appear dark and have no way or adjusting it, sometimes it might randomly correct itself.  
My camera had no noticeable way of changing any of its properties.

## Suggested Fixes
As suggested on some forums, opening the Windows 10 Camera App should fix the contrast issue.  
However this did not work for me.

Alternatively, it was suggested to roll back the driver for camera which I didn't feel like attempting to track down an older version of the driver.

## Solution
The secret it turns out is to use **Skype** and its ability to modify your camera's settings.  
I simply needed to disable auto exposure but there was no native way to do that in Windows 10.

Using Skype I could modify it, here are the steps: 

1. Open up Skype and go to the `Settings` pop-out menu option:
    ![Skype Settings](/assets/images/posts/webcam/skype-settings.png)

2. Navigate to the `Audio & Video` menu item and click on the `Webcam settings` link:
    ![Audio & Video Settings](/assets/images/posts/webcam/settings.audio-video.png)

3. You should see the **Properties** dialog of your camera and the option to change your `Exposure` or `Focus`:
    ![image](/assets/images/posts/webcam/settings.webcam.png)

4. The **Auto** capability on the `Exposure` option, is what caused my camera to not adjust the contrast as expected.  
    From here you can experiment with which checkbox needs toggling off.  

{: .notice--primary}
<strong>References:</strong>  
[Camera brightness in Microsoft Teams](https://answers.microsoft.com/en-us/msteams/forum/msteams_sett-msteams_subother/camera-brightness-in-microsoft-teams/98181a50-e49f-44a5-ace8-a1509145da29)  
[Teams camera too dark](https://techcommunity.microsoft.com/t5/microsoft-teams/teams-camera-too-dark/m-p/1283139)