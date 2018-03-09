---
author: sjohner
comments: true
date: 2016-01-15 23:14:42+00:00
layout: post
slug: new-blog-experience-and-ssl-encryption-by-default-using-lets-encrypt
title: New blog experience and SSL encryption by default using Let's Encrypt
wordpress_id: 1423
categories:
- Blog
tags:
- Encryption
- Linux
- SSL
- Theme
- Wordpress
---

As some may have noticed I did some changes to this blog recently. First I decided to change the theme of my blog to something more simplistic. After some searching around the web I found a great [Wordpress theme named Independent Publisher](https://github.com/raamdev/independent-publisher). I definitively recommend checking out this theme if you are running a [Wordpress](http://wordpress.org) blog. It’s  very reader-focused and also entirely open-source.

Besides that, I opted for enabling SSL for the whole site. Visitors will now automatically be redirected to the secure version of this site. Encryption is done by using free certificates from [Let's Encrypt](https://letsencrypt.org/).


<blockquote>Let’s Encrypt is a free, automated, and open certificate authority (CA), run for the public’s benefit. Let’s Encrypt is a service provided by the [Internet Security Research Group (ISRG)](https://letsencrypt.org/isrg/).

The key principles behind Let’s Encrypt are:</blockquote>





	
  * 


<blockquote>**Free:** Anyone who owns a domain name can use Let’s Encrypt to obtain a trusted certificate at zero cost.</blockquote>




	
  * 


<blockquote>**Automatic:** Software running on a web server can interact with Let’s Encrypt to painlessly obtain a certificate, securely configure it for use, and automatically take care of renewal.</blockquote>




	
  * 


<blockquote>**Secure:** Let’s Encrypt will serve as a platform for advancing TLS security best practices, both on the CA side and by helping site operators properly secure their servers.</blockquote>




	
  * 


<blockquote>**Transparent:** All certificates issued or revoked will be publicly recorded and available for anyone to inspect.</blockquote>




	
  * 


<blockquote>**Open:** The automatic issuance and renewal protocol will be published as an open standard that others can adopt.</blockquote>




	
  * 


<blockquote>**Cooperative:** Much like the underlying Internet protocols themselves, Let’s Encrypt is a joint effort to benefit the community, beyond the control of any one organization.</blockquote>





The Let's Encrypt project was initiated in 2012 by two Mozilla employees and started public beta back in December 2015. The project aims to make encrypted connections omnipresent. The goal is to significantly lower the complexity of setting up and maintaining encryption of websites. On a Linux web server, execution of only two commands is sufficient to set up HTTPS encryption, acquire and install certificates within 20 to 30 seconds. Certificates issued by Let's Encrypt are valid for only 90 days. However renewal can easily be automated using a cron job. Folks from [Digital Ocean](https://www.digitalocean.com/) wrote a nice [tutorial about how to use and automate Let's Encrypt on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-14-04). I recommend checking out Let's Encrypt by yourself. Let's Encrypt is removing any excuses for not encrypting Web applications and services.




