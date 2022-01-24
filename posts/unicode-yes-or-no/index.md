---
title: Unicode Yes or No
description: None
date: 2022-01-31
tags:
  - unicode
---

I've recently been looking for a graphical way to provide an easy at a glance method for indicating yes or no in a Markdown file.  As in, is this feature supported/implemented, yes or no.  One option is to
reference an actual image file, but that was heavier than what I was wanted to do.

That brought me to Unicode options.  I narrowed it down to these two.

### Yes &#9989;

This is the [White Heavy Check Mark](https://compart.com/en/unicode/U+2705), also known as `U+2705`.  The HTML entity is:  `&#9989;`.

### No &#9940;

For this I went with the [No Entry](https://www.compart.com/en/unicode/U+26D4), also known as `U+26D4`.  The HTML entity is `&#9940;`.

---

I'm using the HTML entities for both of these, and so far I'm happy with the results.
