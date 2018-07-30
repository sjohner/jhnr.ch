---
author: sjohner
comments: true
date: 2013-01-15 12:36:16+00:00
layout: post
published: false
slug: using-bitbucket-source-control-with-visual-studio-authoring-extensions-vsae
title: Using BitBucket Source Control with Visual Studio Authoring Extensions (VSAE)
---

If you are developing your Management Packs in Visual Studio with VSAE, chances are good you have various projects on your hard drive. Maybe you want to have your Projects available at home as well as in your Office. Also it would be nice if co-workers could contribute on a Project. Working in a Team on the same Management Pack is maybe a little tricky. I was asking myself if some Kind of source control repository would also work for Visual Studio with Authoring Extensions installed so that my Management Pack Projects are available to others too and moreover i could Access them online everywhere.

I started to use Atlassian [bitbucket.org](http://www.bitbucket.org/) as a source control repository. Mainly because it offers free accounts with unlimited numbers of private repositories. These repositories can additionally be shared among up to five users in case of the free accounts. However BitBucket Forces you to use either Git or Mercurial as revesion control System.

Installation of Git in Visual Studio 2010 is pretty straitforward. Go to your Visual Studio Extension Manager

[![toolsextensionmanager](/images/toolsextensionmanager1.png?w=250)](/images/toolsextensionmanager1.png)

Search for Git in the Online Gallery

[![Extension Manager](/images/extensionmanager.png?w=460)](/images/extensionmanager.png)

Install Git Source Control Provider and Git Extensions. Git Extensions will download a seperate Installer. Go through the wizard

If everything went well you should now see some Git Options in your Contextmenu in Visual Studio. One of them should be _Create Git Repository_

[![CreateGitRepository](/images/creategitrepository.png?w=285)](/images/creategitrepository.png)

Commit Solution, you will probably get a warning about missing configuration

Create new Repository on BitBucket

Right-click on your Visual Studio solution and open Git Bash. This will open a command line window

[![opengitbash](/images/opengitbash.png?w=300)](/images/opengitbash.png)

```
git remote add origin https://sjohner@bitbucket.org/sjohner/test.git
git push -u origin --all
```
