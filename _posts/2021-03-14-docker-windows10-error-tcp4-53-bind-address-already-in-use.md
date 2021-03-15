---
title: "Why is Docker returning the error - listen tcp4 0.0.0.0:53: bind: address already in use?"
date: 2021-03-14
categories:
  - windows10 docker pihole
tags:
  - windows10 docker pihole
---

I recently went to upgrade [pi-hole](https://pi-hole.net/) to its latest version.  
I noticed Docker for Windows needed an update, so I updated that first and then pulled the latest `pi-hole` image.  

When trying to start `pi-hole`, I recieved the following error:
```shell
ERROR: for pihole  Cannot start service pihole: driver failed programming external connectivity on endpoint pihole  
(d3c0b9ea0348be4cb3dd18f8e77aac290a7ef12e2d2dee604cdfd8c35800b119):  
Error starting userland proxy: listen tcp4 0.0.0.0:53: bind: address already in use
```

I had a quick look to see what was using port 53:
```shell
netstat -ano | findStr "53"
  TCP    0.0.0.0:5357           0.0.0.0:0              LISTENING       4
  TCP    [::]:5357              [::]:0                 LISTENING       4
  UDP    0.0.0.0:53             *:*                                    1976
  UDP    0.0.0.0:53             *:*                                    1976
```

Then a lookup to see which process this related too:
```shell
tasklist /fi "pid eq 1976"

Image Name                     PID Session Name        Session#    Mem Usage
========================= ======== ================ =========== ============
svchost.exe                   1976 Services                   0      8,988 K
```

Looking at task manager we discover the process is the **Internet Connection Sharing** Service:  

![Task Manager](/assets/images/posts/docker-dns/task-manager.png)

This has been raised on github, with the cause pointing to some issues with DNS port binding:  
[https://github.com/docker/for-win/issues/10601](https://github.com/docker/for-win/issues/10601)