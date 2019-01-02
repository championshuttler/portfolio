---
layout: post
title: Building your Debian Package
date: 2018-12-24 15:09:00
description: How to build debian package

---

So you are Ubuntu/Debian user. You search for some package with `apt-cache search`. You install package with `apt-get install` . You already know that packages contain files , may have some dependencies and some basic `metadata` like we have in `manifest.json` file for `browser webextensions`.

A Debian package - a `.deb` file - sort of tar.gz or zip file containing metadata and files. 
Let's say you have a hello world application in Python with job only to print Hello World.

{% highlight python %}

#!usr/bin/env python
print("Hello World")

{% endhighlight %}

Let's build a simple package for this application, using as few tools and concepts as possible. This is not the proper way to build a package, but it helps you understand what a package is and how the basic process works. The fully proper way to build a package is quite complicated and involves many tools and concepts that build on top of each other, which we will get to in later.

## Creating the application

First, create a directory for this package and place the above application in `hello`:

```
bash
mkdir helloWorld
cd helloWorld
editor hello   # put the above source code in this file
chmod +x hello
```

## Creating the package root directory

Now that we have an application, let's build a package. The simplest tool for building a .deb file is `dpkg-deb`. It accepts a directory containing package metadata files and content files. Let's create this directory. We call it `packageRoot`.

```
bash
mkdir packageRoot
```

The package metadata must live in a file called `DEBIAN/control` under the package root directory. Let's create it:

```
bash
mkdir packageRoot/DEBIAN
editor packageRoot/DEBIAN/control
```

This is what `DEBIAN/control` should contain:

```
Package: hello
Version: 1.0.0
Architecture: all
Maintainer: Shivam Singhal <championshuttler@gmail.com>
Depends: python
Description: Shivam's Hello World Package written in python and prints greeting.

```

### Control file fields

These are the meanings of the fields:

- "Package" specifies the package name.

- "Version" specifies the package version number.

  Note that this consists of the application's own version number, followed by a dash, followed by a so-called _Debian package revision number_. The latter specifies the version number of the packaging work. Every time you modify the packaging work without modifying the application, you are supposed to bump this number but not the application's own version number. This is the first time we build a Debian package, so we specify "1".

- "Architecture" specifies on which computer architectures this package is installable. Since Python apps themselves are platform-independent, we specify "all". But if we were packaging a C program, then this could also contain the name of a specific architecture such as "amd64" (Debian's name for x86_64) or "i386".

- "Maintainer" specifies who maintains this package.

- "Depends" is a comma-separated string that specifies this package's dependencies.

- "Description" contains a summary on the first line, and a more verbose description on subsequent lines. The summary is what you see in `apt-cache search` while the more verbose description is what you see in APT GUIs such as the Ubuntu App Store or Aptitude.

  Note: the verbose description must be prefixed with a single space! And empty lines must contain a single dot character.

## Turning the package root directory into a package

Next, let's define the package contents. All files under the package root directory, except for `DEBIAN`, is considered part of the content. We want `hello` to be installed as `/usr/bin/hello`, so:

```
bash
mkdir -p packageRoot/usr/bin
cp hello packageRoot/usr/bin/
```

Now that the package root directory is finished, we turn it into a .deb file:

```
bash
dpkg-deb -b packageRoot hello_1.0.0_all.deb
```

## Verifying that it works

Success! You can now install the .deb file and verify that it works:

```
$ sudo gdebi -n ./hello_1.0.0_all.deb
$ hello
hello 1.0.0
```

## Conclusion

A Debian package is an archive file that contains metadata (such as name, dependencies, description) and files. We have learned how to write a basic metadata specification file (the `control` file).

Thanks for reading. Cheers!!

I code at <a href="https://github.com/championshuttler" target="blank">@championshuttler</a> and tweet at <a href="https://twitter.com/idkHTML" target="blank">@idkHTML</a>. Let me know if you need any help.
