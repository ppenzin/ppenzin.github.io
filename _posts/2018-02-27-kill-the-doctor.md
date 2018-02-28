---
layout: post
comments: true
title: "Kill The Doctor"
date:  2018-02-27 22:29:19
categories: [Open Source, Motivation]
---

LLVM has "Kill the doctor" [test utility][ktd] for disabling DrWatson when
tests are running (a scenario would be that a test would throw an exception and
DrWatson UI would pop up, freezing the build). Its file comment is worth
reproducing in full:

```
//===- KillTheDoctor - Prevent Dr. Watson from stopping tests ---*- C++ -*-===//
//
//                     The LLVM Compiler Infrastructure
//
// This file is distributed under the University of Illinois Open Source
// License. See LICENSE.TXT for details.
//
//===----------------------------------------------------------------------===//
//
// This program provides an extremely hacky way to stop Dr. Watson from starting
// due to unhandled exceptions in child processes.
//
// This simply starts the program named in the first positional argument with
// the arguments following it under a debugger. All this debugger does is catch
// any unhandled exceptions thrown in the child process and close the program
// (and hopefully tells someone about it).
//
// This also provides another really hacky method to prevent assert dialog boxes
// from popping up. When --no-user32 is passed, if any process loads user32.dll,
// we assume it is trying to call MessageBoxEx and terminate it. The proper way
// to do this would be to actually set a break point, but there's quite a bit
// of code involved to get the address of MessageBoxEx in the remote process's
// address space due to Address space layout randomization (ASLR). This can be
// added if it's ever actually needed.
//
// If the subprocess exits for any reason other than successful termination, -1
// is returned. If the process exits normally the value it returned is returned.
//
// I hate Windows.
//
//===----------------------------------------------------------------------===//
```

[ktd]: https://github.com/llvm-mirror/llvm/blob/master/utils/KillTheDoctor/KillTheDoctor.cpp

