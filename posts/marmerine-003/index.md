---
title: Marmerine 0.0.3
description: None
date: 2022-05-26
tags:
  - marmerine
  - memcached
---

I tagged [version 0.0.3 of Marmerine](https://github.com/josephscott/marmerine/) this week.  There are number of changes touching various parts of the server:

- Implement the stats command ( only a subset of data is available )
- Implement the cas command
- Fix: do not try to repeatedly enable SQLite WAL
- New test: cas command
- New test: gets command
- New test: stats command
- CLI option: --port
- CLI option: --verbose
- SQLite schema update: added cas field
- SQLite: enable WAL
- Bump workerman to 4.0.37
- Bump phpstan to 1.6.7
- Bump pest to 1.21.3

The `stats` command is now in, and the data it provides is not identical to the original memcached.

To make it easier to run multiple copies of Marmerine at the same time it now supports the `--port` argument.  And for those that want to know what is going on underneath, there is `--verbose`.  That not shows the Memcached requests and responsed, it shows the SQLite queris.
