---
layout: post
title:  "Bloodhound install guide"
---
![](../assets/BloodHound-White-on-Red.png)
# How to install BloodHound

This will be a quick tutorial on how to install Bloodhound on Kali Linux. lets get into it.  
### Update the system
Get the latest and greatest.
```
apt-get update
```
For those of you who feel like living on the edge.
```
apt-get dist-upgrade
```
### Change Neo4j password
Launch Neo4j
```
neo4j console
```
Browse to the web interface at **http://localhost:7474**

Login with the default credentials
* Username: neo4j
* Password: neo4j

Complete the password change in the browser

### Run BloodHounnd
Open a new terminal
```
bloodhound
```
BloodHound is now running.

### Login

* Database URL – bolt://127.0.0.1:7687
* Username – neo4j
* Password – your newly changed password  

Hopefully this was a nice and quick guide to help anyone out there having any issues getting up and running with the awesome tool that is Bloodhound.
