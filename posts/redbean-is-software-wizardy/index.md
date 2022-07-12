---
title: Redbean Is Software Wizardry
description: None
date: 2022-06-22
tags:
  - redbean
---

Every now and then you come across a piece of code that feels like magic.  Today that is [Redbean](https://redbean.dev/), it handles web requests with wizard like powers.

**Redbean is a single binary web server, written in C, that runs on natively on Linux, Mac, Windows, FreeBSD, OpenBSD, and NetBSD.**  All of that is made possible by [Cosmopolitan Libc](https://justine.lol/cosmopolitan/index.html).  And if that were not enough it also embeds Lua and SQLite.  How big is that binary you ask?  2.1 megabytes.

On my MacOS laptop it was three easy steps:

```bash
$ curl https://redbean.dev/redbean-2.0.11.com > redbean.com
$ chmod 755 redbean.com 
$ ./redbean.com
```

Then visit `http://127.0.0.1:8080/`.  The documentation at `http://127.0.0.1:8080/help.txt` covers the details of what redbean can do.

Oh, and it comes with TLS support out of the box, using an automatically generated self signed certificate by default.  Meaning those three commands above already include HTTPS.  That self signed certificate will generate a warning in browsers, Chrome says `NET::ERR_CERT_AUTHORITY_INVALID`.

But wait, there is more!

> redbean uses a protocol polyglot for serving HTTP and HTTPS on the same port numbers.

You read that right, it supports HTTP and HTTPS on the same port.  Going to `https://127.0.0.1:8080/` works right away.  Not something I'd generally look at for regular production services, but it makes getting things started locally very easy.

All in all, Redbean is a fascinating piece of software, one that I'll be keeping my eye on.
