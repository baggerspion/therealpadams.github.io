---
layout: post
date: 2017-06-23 10:00
title: The core/R Habitat Package
categories: [Habitat]
header:
  src: smart-cat.jpg
  desc: The latest in kitten eyeware by shadow planet (https://flic.kr/p/7fA7wr)
---
At the end of last month [I blogged
about](http://baggerspion.net/2017/05/habitat-R/) my work in producing
a `core/R` package for [Habitat](https://www.habitat.sh). Back then I
mostly wrote about my experience of sharing a
[PR](https://github.com/habitat-sh/core-plans/pull/571) with the
Habitat community; this PR has now finally been merged.

In this blog post I am writing about what is actually inside that PR
(it's waaaay more than _just R_) and about why it took a while to get
merged.

## R's Dep Tree

As you can see from
[here](https://github.com/habitat-sh/core-plans/blob/master/R/plan.sh)
R has a reasonable number of dependencies to track. The problem, when
I started working on this, is that many of them did not exist.

### Graphics Libraries

The biggest culprit was graphics libraries. Not entirely
surprsing, but it was time to fix this. In addition to R, I also had
to package:

- Cairo: a 2D graphics library
- Harfbuzz: a text shaping engine (whatever that is)
- Pango: text layout and rendering


### Making The Fonts Work

With all those font rendering / layout / whatever libraries, it was
kinda important to actually provide some fonts, too. So, in addition
to all of the graphics libraries, I also packaged the Liberation
Fonts.

To make use of these fonts, you need to install the package and then
invoke the font cache:

```
hab pkg install core/liberation-fonts-ttf
hab pkg exec core/fontconfig fc-cache $(hab pkg path core/liberation-fonts-ttf)
```

### The Pixman Problem

As part of this work, [Steven Danna](https://github.com/stevendanna)
noticed that the `core/pixman` package was not properly exporting its
pkglibs. Harmless enough fix...

Or is it?

In Habitat, even a change in what is effectively metadata requires the
package to be rebuilt. The patch of pixman, therefore, causes a
rebuild of `core/pixman`, which cascades new builds in the direction
of `core/zlib`...

Time to get
[comfy](https://habitat-sh.slack.com/archives/C0VBY57UP/p1497284860638662?thread_ts=1497271708.805488&cid=C0VBY57UP).

## There You Have It!

So with the merging complete, I am happy that you can now get your
hands on the following in Habitat:

- core/cairo
- core/harfbuzz
- core/liberation-fonts-ttf
- core/pango
- core/R

Getting your hands on R itself is easy enough:

```
hab pkg install core/R
hab pkg exec core/R R
```

As usual, I did not work alone on this. So shout outs to:

- Packaging contributions from:
  - Steven Danna
- Reviews from:
  - [Ian Henry](https://github.com/eeyun)
  - [Ben Dang](https://github.com/bdangit)
- Highly unpleasant build/merge from:
  - Ian Henry

Thanks all!

## What's Next, Paul?

I'm glad you asked: [GnuPlot](http://www.gnuplot.info)