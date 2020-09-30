---
title: "Error installing .Net 1.1 'Error: internal error 2709 feature GUID for use by Cartman'"
date: 2006-12-17
categories:
  - geekswithblogs
tags:
  - .net
---

I recently rebuilt a dev machine and was trying to install .Net 1.1 and got the following error:  
```Error: internal error 2709 feature GUID for use by Cartman```

It apparently happens when something goes wrong or you have an aborted installation of .Net 1.1 framework.

The fix for this can be downloaded from this address: <http://astebner.sts.winisp.net/Tools/dotnetfx_cleanup_tool.zip>{:target="_blank"}

Just run the tool and it cleans up everything, my install then proceded flawlessly.

{: .notice--primary}
<strong>Reference:</strong>  
Original fix: <http://forums.microsoft.com/MSDN/ShowPost.aspx?PostID=9409&SiteID=1>{:target="_blank"}  
<br/>
<strong>Originally posted on:</strong>  
<http://geekswithblogs.net/rwillgoss/archive/2006/12/17/101385.aspx>{:target="_blank"}
