---
layout: post
title: Managing jekyll with Sublime3
subtitle: "jekyll-Sublime"
date: 2019-03-05
background: '/PATH_TO_IMAGE'
---

## Configuring sublime3 to manage Jekyll

### Package control in Sublime 3

* Press ```Ctrl + Shift + P``` (Windows/Linux) or ```Command + Shift + P``` (OS X)  
* Search for the **Package Control: Install Package** command
* Search packages for Jekyll and hit ```Enter``` to install the latest version

### User Settings
* Go to Prefrences > Package Settings > Jekyll > Settings - User
* Edit the file with these changes
* Also, path strings should follow your **system-specific** path convention. For example, Windows machines should have a path similar to C:\\Users\\username\\site\\_posts, while Unix/Linux systems should have a path similar to /Users/username/site/_posts.  
```
{
	"jekyll_posts_path": "C:\\Users\\username\\site\\_posts",
	"jekyll_uploads_path": "C:\\Users\\username\\site\\assets",
	"jekyll_drafts_path": "C:\\Users\\username\\site\\_drafts"
}
```
### Using the jekyll plugin
* Press ```Ctrl + Shift + P``` and type ```jekyll```
* You will be presented with the options that are avaialable to use