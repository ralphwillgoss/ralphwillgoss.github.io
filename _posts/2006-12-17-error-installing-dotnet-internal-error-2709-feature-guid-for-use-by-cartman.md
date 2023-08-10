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

The fix for this is to use the `cleanup_tool.exe`, available for download from <https://github.com/openpeach/dotnetfx_cleanup_tool>{:target="_blank"}

Just run the tool and it cleans up everything, my install then proceded flawlessly.

{: .notice--primary}
<strong>Reference:</strong>  
<strong>Originally posted on:</strong>  
[geekswithblogs.net c/o archive.org](https://web.archive.org/web/20200803100505/http://geekswithblogs.net/rwillgoss/archive/2006/12/17/101385.aspx){:target="_blank"}
