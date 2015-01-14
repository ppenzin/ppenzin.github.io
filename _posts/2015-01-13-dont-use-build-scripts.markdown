---
layout: post
comments: true
title:  "Do not use scripting languages for building your project"
date:   2015-01-13 18:16:37
categories: [Builds, Design Decisions]
---

Every now and then I come across projects that use some sort of scripting
solutions to hold their builds together. What I mean is build scripts written
in Bash, Python, Perl or any other general-purpose scripting language. Usually
scripts are used as wrappers for makefiles or any other build tools and are
trying to make up for functionality those tools are lacking, though there are
those rare cases when they are used to invoke the compiler directly. This has
to stop and here is why:

- Is is wasteful. You are requiring people to carry around a shell or an
  interpreter (often legacy) just for the sake of running a handful of scripts
  (often just a single script).  Unless you are using those things for what
  they are supposed to be used, that is an unnecessary dependency. Moreover,
  building is fairly repetitive and tedious by nature, so you are looking at
  chasing a lot of little bugs that cause substantial trouble.
- It is not quite cross-platform. This is mostly true for Bash idioms and other
  _linuxisms_. Not everyone in the world has the same set up as you do, nor
  they need to have.
- It does not do you any good personally, unless you want to do program in
  scripting languages for a living. It is a much better investment of time to
  learn to do advanced stuff with popular build tools than hone your skills in
  languages that you would never use.

Here are some suggestions:

- Switch to more advanced build tools. CMake and Autotools help when
  Makefile-based projects get out of hand; Maven is a powerful tool for Java
  projects, and so on. Those tools have been around for a while and have become
  de facto standards. They have the common use-cases implemented and are
  more consistent that hand-rolled build systems.
- Write your own build modules: use extendable build tools (CMake for example)
  and write extensions for it. That way your build scripts are kept in a
  language designed for builds.
- Create your own build tool. If nothing helps, try to implement the custom
  functionality that you need as a complete product rather than set of
  scripts. Designing it will help you review your build process and might
  trigger some positive changes for the project. And if it succeeds you get
  another marketable app almost for free.

