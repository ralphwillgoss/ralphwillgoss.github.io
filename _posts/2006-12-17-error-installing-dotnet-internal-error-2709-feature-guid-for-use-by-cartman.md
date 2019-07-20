---
layout: post
title: "Error installing .Net 1.1 'Error: internal error 2709 feature GUID for use by Cartman'"
date: 2006-12-17
---

I recently rebuilt a dev machine and was trying to install .Net 1.1 and got the following error:
```Error: internal error 2709 feature GUID for use by Cartman```

It apparently happens when something goes wrong or you have an aborted installation of .Net 1.1 framework.

The fix for this can be downloaded from this address: http://astebner.sts.winisp.net/Tools/dotnetfx_cleanup_tool.zip

Just run the tool and it cleans up everything, my install then proceded flawlessly.

I found this fix on this post originally: http://forums.microsoft.com/MSDN/ShowPost.aspx?PostID=9409&SiteID=1

Originally posted at http://geekswithblogs.net/rwillgoss/archive/2006/12/17/101385.aspx
