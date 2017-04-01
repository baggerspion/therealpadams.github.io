---
layout: post
date: 2017-04-01 12:00
title: Playing In My Local Habitat
categories: [Docker, Habitat, Chef]
header:
  src: habitat.jpg
  desc: Habitat.sh logo
---
Earlier this week I attended the [Berlin Chef
Meetup](https://www.meetup.com/berlin-chefs/events/238210151/). I'm
hardly a regular at this group... the last time I attended was the
[Inspec](https://www.chef.io/inspec/) release party back in September
last year.

This meetup included a selection of talks, one of which gave me the
opportunity to see [Habitat](https://habitat.sh) in use for the first
time.

## What's Habitat All About?

Habitat is platform designed to support the entire DevOps
lifecyce... all the way from building to configuring, deploying and
running services. It even allows for some snazzy things like deploying
new confogurations to your services at runtime.

For a fuller perspective, here is the original alunch anouncement
video:

{% include youtube.html id="oxtRP1eYCns" %}

## Having A Play

Suitably enthused, I made my way through the ["Getting Started with
Habitat" tutorial](https://www.habitat.sh/tutorials/) which is easy
enough. With some much prepared in advance, I decided I wanted to
build and deploy my own service from scratch and so I decided to
create a super-simple "Hello, World!" service using Python and Flask.

The aim of my service was simple:

- Provide a service that returns "Hello, World!" when accessed over
  http 
- Make the service configurable at start so that it might say "Hi" to
  some specific name

Creating the service is easy enough and then getting Habitat to
package it, likewise. There was one small gotcha in that using Python
virtual environments inside Habitat caused me many, many headaches
(notably, installing dependencies was fine but Python itself would
baulk). To be honest, this may very well be PEBCAK. Regardless, I
ditched the venv and installed Flask without one.

After this, using Habitat to build and package is super simple:

{% include asciinema.html file="asciicast-110203.json" %}

The final step is then to checkout start-time configuration of the
packaged service. Habitat provides great functonality for this (you
can [see the docs
here](https://www.habitat.sh/tutorials/getting-started/mac/process-build/)).

Here you can see how I provide the name "Paul" to a new instance of
the service:

{% include asciinema.html file="asciicast-110205.json" %}

## What Next?

So far I am enjoying playing with Habitat. Definitely have a lot more
to learn but, even with the little I have covered, it is clear that
Habitat is a great tool for putting the entire DevOps lifecycle in the
hands of the service developer.

One of the super-sexy features of Habitat is reconfiguration of the
service _at runtime_! That's what I'm looking into next.