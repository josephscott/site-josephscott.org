---
title: ttfb-curl
description: None
date: 2022-04-19
tags:
  - curl
  - ttfb-curl
---

Every now and then I run into a situation where I need to gather TTFB ( Time To First Byte ) data over many runs.  Because of the very limited scope I usually throw together a simple Bash script with <a href="https://curl.se/">curl</a> doing all the requests.  The <a href="https://curl.se/docs/manpage.html">time_starttransfer</a> variable that is exposed for each request makes it simple enough.

This came up again and I decided it was finally time to write a decent version, one that provided all the data points that I was looking for.  The end result is <a href="https://github.com/josephscott/shell/blob/master/bin/ttfb-curl">ttfb-curl</a>.

Here is an example of the output:

```
$ ttfb-curl https://www.amazon.com/ 

Making request 100 of 100

*** TTFB of 100 requests, in seconds ***

min    = 0.125706
avg    = 0.174524
max    = 0.521788
stddev = 0.0563159

p50 = 0.154990
p75 = 0.176796
p90 = 0.256915
p95 = 0.278385
p99 = 0.328828

*** Buckets ***

| 0.0 -> 0.1 = 0
| 0.1 -> 0.2 = 85
| 0.2 -> 0.3 = 10
| 0.3 -> 0.4 = 4
| 0.4 -> 0.5 = 0
| 0.5 -> 0.6 = 1
```

It provides all the result breakdowns that I'm usually looking for in this situation, especially the percentiles and the buckets.

There isn't really anything too exciting happening on the inside.  The standard deviation and buckets are done in awk.  I linked to the Stack Overflow threads that I copied those from, or were heavily inspired by, in the source code comments.

Now I can reach for this the next time this comes up, instead of writing and throwing away another Bash script.
