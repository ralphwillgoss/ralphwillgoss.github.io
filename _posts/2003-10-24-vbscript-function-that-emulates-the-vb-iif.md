---
title: "A VBScript Function that Emulates the VB IIf()"
date: 2003-10-24
categories:
  - geekswithblogs
tags:
  - asp
---

If you're still using classic ASP, this is a simple little VBScript function that emulates the VB IIf().<br/>
It helps in intializing and making your code look a little cleaner.

```visualbasic
'# classic asp alternative to the inbuilt VB function IIf
function iif(boolEval, trueStr, falseStr)
  if boolEval then
    iif = trueStr
  else 
    iif = falseStr
  end if
end function

'# simple usage
dim UserID
UserID = iif(request("userid") = "", -1, request("userid"))
```

Originally posted on [DevX](http://www.devx.com/DevX/Tip/17670)
