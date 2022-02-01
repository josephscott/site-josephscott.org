---
title: Marmerine, a Memcached Server Implementation
description: None
date: 2022-02-01
tags:
  - marmerine
  - memcached
---

In December 2021 I got curious about the Memcached ASCII protocol.  After some time with the [docs](https://github.com/memcached/memcached/blob/master/doc/protocol.txt) and inspecting packets with [Wireshark](https://www.wireshark.org/), I started wondering how hard it would be to write a Memcached server.  Thus was born [Marmerine](https://github.com/josephscott/marmerine).

At this point I've got just barely enough of the protocol supported to be considered minimally functional.  Specifically it currently only supports:

- set
- add
- get
- delete
- flush_all
- quit

That gets it through the very basics of [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete): Create ( add ), Read ( get ), Update ( set ), and Delete ( delete ).  In the [Marmerine README.md](https://github.com/josephscott/marmerine/blob/trunk/README.md) I've listed all the commands for the ASCII protocol, but only filled out details for the ones it supports.

Expiring data works, but there are no restrictions yet on how much total data you can put in it.  Which brings me to the idea of storage.

### Storage

One of the first things that I wanted to try with this beyond just implementing the protocol was different storage features.  Memcached only has one way to store data, in memory.  For Marmerine I wanted to have multiple options, starting with disk and memory.  Enter [SQLite](https://www.sqlite.org/index.html), which has easy options for doing both.

By default Marmerine uses a disk based SQLite file.  I've also tried it using `:memory:`, and that works too.

To take this idea of storage abstraction one step further, the entire storage backend is implemented as a single class.  That makes it easy to drop in an entirely different storage backend with another class.  In the future I may try that out, if for no other reason than to flush out all the details.

### PHP

Look, I still spend a decent amount of time [working](https://automattic.com/) in PHP.  However, I did try something new to me in PHP for the TCP server parts of this: [Workerman](https://github.com/walkor/workerman).  Beyond that, I the wanted [innovation tokens](https://mcfunley.com/choose-boring-technology) focused on the Memcached ASCII protocol.  Between the protocol and Workerman, I'd call it one and half innovation tokens.

There are tests in PHP as well.  For that I used a new to me testing framework called [Pest](https://pestphp.com/).  That makes it more like two full innovation tokens.

Later on it could be interesting to implement this in other languages, to see how they compare.

### Testing

Having basic tests while working on command implementations was super helpful.  I could confirm everything worked as expected by running them against Memcached, then run them against Marmerine to see what broke.  Questions about the differences were resolved by dropping down to the real source of truth for a network protocol: packets over the wire.

More than once I would run a test against Memcached, then Marmerine, and go over the details of each packet to figure out why things were breaking.  Wireshark made that really easy.

### Possible Experiments

I mentioned the idea of different storage systems already.  Along with that, another aspect of Memcached that it might be fun to alter is preserving state.  Because it only stores data in memory, stopping or restarting Memcached means all of the data goes away.  In Marmerine, it doesn't have to be that way.

Right now Marmerine doesn't delete data between runs, because the data is stored on disk to a SQLite file.  Nothing happens to that file when you stop Marmerine, which I did so that I could easily inspect the database outside of Marmerine.

Leveraging the storage class flexibility would also be a way to provide a proxy mechanism to other systems.  For example, [twemproxy](https://github.com/twitter/twemproxy) proxies for Memcache and Redis.  In Marmerine it could proxy to potentially anything you had access to.

### Next

This is a playground for me.  I'll implement more of the protocol, and try a few non-standard Memcache experiments.  For now I'm just enjoying playing around in the world of Memcached server implementations.

Consider this very prototype level code, so be warned if you try it out.  Anything you put in it might never come out.
