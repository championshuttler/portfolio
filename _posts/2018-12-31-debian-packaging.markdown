---
layout: post
title: Building your Debian Package
date: 2018-12-30 15:09:00
description: an example of a blog post with some code
---
So you are Ubuntu/Debian user. You search for some package with `apt-cache search`. You install package with `apt-get install` . You already know that packages contain files , may have some dependencies and some basic `metadata` like we have in `manifest.json` file for `webextensions`. 

A Debian package - a `.deb` file - sort of tar.gz or zip file containing metadata and files. 
Let's say you have a hello world application in Python with job only to print Hello World.

{% highlight python %}

#!usr/bin/env python
print("Hello World")

{% endhighlight %}
