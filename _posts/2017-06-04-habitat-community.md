---
layout: post
date: 2017-06-04 10:00
title: Visualising The Habitat Community
categories: [Habitat]
header:
  src: blobs.png
  desc: Visualisation of Git activity
---
It's been a while since I did a Git [repo
visualisation](/2016/12/docker-2016/), which means I am definitely
overdue to talk about the [Habitat](https://habitat.sh) community. In
this blog post I will show-off the main repos for Habitat (at least
the main ones _I_ have been hanging out in) and describe what I have
found.

## Getting At The Code

I've been involved with Habitat for a few months now. I have been
lucky in that my work with Habitat coincided with ChefConf, which gave
me the opportunity to meet community members face-to-face. But had I
not been to ChefConf, how might I gain an understanding for who the
high-bus-factor contributors are?

In step the now-famous[^1] blue blobs.

The Habitat project lives on [Github](https://github.com/habitat-sh)
spread across 14 repos. The most active projects are "habitat" (the
main codebase of the Habitat application) and "core-plans" (the
repository of build-from-source packages providing the dependencies
for _your_ application).

### Habitat

Without further ado, here's the blue blobs for Habitat (click to
enlarge):

[![Habitat Blobs All Time]({{ site.imgroot }}Images/habitat.png)]({{ site.imgroot }}Images/habitat.png)

I've been staring at these blue blob visualisations for years now. As
a result I think the things that I find interesting in them has become
somewhat esoteric over time. That said, this visualisation has all the
hallmarks I expect of a new project:

- A period where only a few crucial individual get things kicked-off;
- Followed by a period of recruiting one-time-only contributors;
- Finally the start of a longer-term community.

So what's so interesting in here? Let's start with [Adam
Jacob](https://twitter.com/adamhjk). Adam appears to have worked alone
on Habitat for a whole 4 months before he was joined by someone
else. I assume Habitat was not some super-secret project for him, but
it _is_ very interesting that it was 4 months before someone else
joined in.

The other thing that is interesting here is the influence of Chef:
"contributors"[^2] such as "Delivery Server" and "The Friendly Habitat
Sentinels" speak to the presence of bots doing useful things around
the project. If this was a "pure community effort" these would not
typically appear until much later in the lifetime of the project. The
overall "blueness" of this visualisation is quite low; something I'd
expect of a newish project. What _is_ interesting, is that the people
who are contributing over extend periods are also contributing lots
(darker shades of blue). Many of these contributors are, of course,
employees of Chef.

### Core-Plans

The core-plans repo[^3] contains commonly-required build/run-time
dependencies for the applications that Habitat users are
packaging. You know, like [gcc](http://gcc.gnu.org). I _suspect_ we
are more likely-to find non-Chef-employee contributions in this repo.

[![Habitat Blobs All Time]({{ site.imgroot }}Images/core.png)]({{ site.imgroot }}Images/core.png)

At least my own contributions are a little more apparent!

The most striking thing here is the distribution of effort. There is a
reasonable amount of blue in that chart and it is nicely distributed
across many contributors. This _is_ the mark of a healthy community
effort. Of course, the most frequent contributors are Chefs.

### Other Stuff

Of course there is a lot more to the Habitat community than simply
what happens inside Git. Of particular note are:

- The Habitat Community [slack channel](http://slack.habitat.sh)
- The Community [page](https://www.habitat.sh/community/) which
  includes suggestions of tasks to work on and a visualisation of the
  overall roadmap.

## Conclusions

As always there is a huge amount of caveats attached to use of the
blue blobs; most notably, way more happens within a community than
simply what we see in Git. That said, these visualisations are a good
way for finding out who are the key contributors to a
community. Particularly useful knowledge when you reach out for help
for the first time.

Like I have done for [other communities in the
past](/2016/12/docker-2016/) I'll roll out some annual updates like
this for Habitat. Of course, for day-to-day community management,
something a little more robust is required.

Work is already in place for that and you can bet I'll be writing
about it when it is all in place.

## Footnotes

[^1]: Not really all that famous. If you want to make your own blue blobs visualisation, [the tool lives here](https://github.com/therealpadams/git-viz). One of these days I will get around to documenting it.
[^2]: I apologise for the use of inverted commas; yes, bots are people, too.
[^3]: aka "HabitatOS"