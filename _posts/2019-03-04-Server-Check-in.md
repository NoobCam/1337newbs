---
layout: post
title: "Server Check In"
subtitle: "SSH user list"
date: 2019-03-04
background: '/PATH_TO_IMAGE'
---


# Server Check In 
## SSH user list generation from log files on the blog


After neglecting the blog for awhile I decided to check in to see if everything was still up and operational. The SSL cert needed updating but it seems everything else was still running strong. I figured I would poke around the logs of the system to make make a user name list from the attempted SSH logins.
```
grep 'Failed password for invalid' /var/log/secure* | cut -d ' ' -f 12 | sort -u
```

Here are the results  
[ssh_users.txt](https://1337newbs.com/text/ssh_users.txt)  
I also pulled out the URL's that have been requested in the apache log.  
[urls.txt](https://1337newbs.com/text/urls.txt)
