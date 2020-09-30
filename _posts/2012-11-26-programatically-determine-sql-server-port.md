---
title: "How do I programatically determine which port a SQL Server is running on?"
date: 2012-11-26
categories:
  - geekswithblogs
tags:
  - sql
---

How do I programatically determine which port a SQL Server is running on?

``` sql
/*
Wrapper script for xp_readerrorlog
Author: Ralph Willgoss
Date: 2nd Oct 2012
This script cycles through all logs files, looking for the listening port.
Normally you have to specify the log file one by one, the script removes the need for that.

Param ref for: xp_readerrorlog
1. Value of error log file you want to read: 0 = current, 1 = Archive #1, 2 = Archive #2, etc...
2. Log file type: 1 or NULL = error log, 2 = SQL Agent log
3. Search string 1: String one you want to search for
4. Search string 2: String two you want to search for to further refine the results
5. Search from start time
6. Search to end time
7. Sort order for results: N'asc' = ascending, N'desc' = descending
*/

USE Master
GO
--  Get log count
DECLARE @logcount int
DROP TABLE #Result
CREATE TABLE #Result (ArchiveNo int, Date datetime, Size int)
INSERT INTO #Result
EXEC xp_enumerrorlogs
SET @logcount = (SELECT COUNT(*) FROM #Result)

-- Search the available logs
DECLARE @counter int
SET @counter = 0
WHILE @counter <= @logcount
BEGIN
   EXEC xp_readerrorlog @counter, 1, N'Server is listening on', 'any', NULL, NULL, N'asc'
   SET @counter = @counter + 1
END
GO
```

{: .notice--primary}
<strong>Originally posted on:</strong>  
<http://geekswithblogs.net/rwillgoss/archive/2012/11/26/programatically-determine-which-port-a-sql-server-is-running-on.aspx>{:target="_blank"}