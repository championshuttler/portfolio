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

Let's build a simple package for this application, using as few tools and concepts as possible. This is not the proper way to build a package, but it helps you understand what a package is and how the basic process works. The fully proper way to build a package is quite complicated and involves many tools and concepts that build on top of each other, which we will get to in later.
