---
date: 2017-05-22 12:00
title: ChefConf 2017 - Day -1
categories: [Habitat, Chef]
header:
  src: chefconfheader.jpg
  desc: My badge after registration at ChefConf 2017
---
After a fairly last-minute decision to attend, I am currently in
Austin for [ChefConf 2017](https://chefconf.chef.io/2017). As you
might have guessed by now, I am mostly here for the
[Habitat](https://habitat.sh) content. I'm also here to meet more of
the Chef community and learn more about the full suite of Chef tools.

## The Great Package Update

Preparations for ChefConf were failty uneventful. I am going to be
talking in the Habitat Zone<sup>&trade;</sup> about my work on
packaging [Drupal](https://www.drupal.org). Just in case, I also
wanted to prepare a talk on my work on packaging
[CrateDB](https://crate.io).

Then [Habitat 0.24 was released](https://forums.habitat.sh/t/habitat-0-24-0-released/334)...

<center>
<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Updating <a href="https://twitter.com/habitatsh">@habitatsh</a> plans at 19:00 on the night before travelling to <a href="https://twitter.com/chef">@chef</a> <a href="https://twitter.com/hashtag/chefconf?src=hash">#chefconf</a> because you find they are not Habitat 0.24 compatible. <a href="https://t.co/kvzyXxYsD0">pic.twitter.com/kvzyXxYsD0</a></p>&mdash; Paul Adams (@therealpadams) <a href="https://twitter.com/therealpadams/status/865979012300111872">May 20, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script></center>

In Habitat 0.24 `hab service` became `hab svc` and this borked the
`endocode/drupal` and `baggerspion/drupal` plans. In the unikely even
that you have been working with these packages, I'm sorry for the
diruption. New builds are now available for both. I suspect `hab
service load...` is supposed to still work and the need to update my
packages has exposed a bug.

My first flight left Berlin pretty early and so no work was happening
on that leg of the journey. After I arrived to EWR I was able to do
some `hab pkg instal1` grab all my dependencies and fix the
packages. The new builds are now available on the public depot.

## Arrival: Austin

I was fairly exhausted by the time I reached Austin.

In my tiredness I was really struggling to find the taxi rank at the
airport (public health warning: no Uber in Austin, yo). I searched
outside the terminal for anyone semi-official who could point me in
the right direction. The person I found was totally confused by the
question. No shit: they were the taxi rank attendant and I was
standing right next to the taxi rank.

After getting to my hotel and having a much-needed shower, I headed
out to find food and music. Nice thing about downtown Austin: there is
absolutely no problem finding either.

## What's Ahead?

So this is my first ChefConf. I even have a "My 1st ChefConf" sticker
to prove it! On Tuesday (the first day of the main conference) I will
be giving a talk in the Habitat Zone. For the rest of the conference,
I will be soaking up any/all Habitat content in the programme.

Talks I will definitely be attending:

- ["Getting Started with Habitat"](https://guidebook.com/guide/89460/event/15817761/)
- ["Habitat in Production"](https://guidebook.com/guide/89460/event/15817761/)
- ["Ephemeral Apps with Chef, Terraform, Nomad, and Habitat"](https://guidebook.com/guide/89460/event/15476612/)
- ["Python Applications with Habitat"](https://guidebook.com/guide/89460/event/15476612/)
- ["Kubernetes & Habitat"](https://guidebook.com/guide/89460/event/15476612/)

On Thursday there is a day-long hackday. I will continue work on my
Habitat packaging and will gladly give help to anyone starting out
with the technology. Just come and poke me for help!

I _will_ be going to Franklin's for BBQ. At some point I am hoping to
go to Elm Street Tattoo (legendary tattoo parlour, for the
uninitiated) to take photos, if they will let me.

## The Thanks

I always enjoy taking part in tech conferences. Especially when I am
working with new technologies and want to meet the community around
it.

My very existence at ChefConf has been heavily subsidised by both
[Chef](https://www.chef.io) and [Endocode](https://endocode.com). I'm
hugely greatful to both that I can be here.