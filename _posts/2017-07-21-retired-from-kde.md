---
date: 2017-07-21 10:00
title: Retired From KDE
categories: [KDE]
header:
  src: kde-committers.png
  desc: A visulisation of activity in a KDE repo
---
Many of you reading this are probably already aware, long-time
maintainer of glibc Roland McGrath has recently
[retired](https://www.theregister.co.uk/2017/07/10/glibc_maintainer_roland_mcgrath_steps_down/)
from maintaining that project. Inspired by his words, I wanted to say
a few things about why I no longer contribute to KDE; a project I
"retired" from some time ago now.

Recently two very good friends of mine, both long-term KDE
contributors, inquired if I was going to be attending this year's
Akademy (the annual KDE conference). Neither were particularly
surprised when I said I wasn't.

I was surprised that they asked.

## Getting Into KDE

My first experiences of KDE were many moons ago; sometime in the very
early 00s I guess. I had installed Linux on an old machine and was not
particularly enjoying the desktop experience.

There wasn't a desktop.

I cannot remember which distro this was. It had come off some
magazine's cover CD. There was X. And the tiling window manager which
allowed me to fill my screen with x-term. This, for a long time, was
just how I got stuff done. Emacs, Mutt, Lynx and some weird
terminal-based MP3 player were my jam.

Some time later I was reading another magazine (Linux Format?) and it
had a review of a recent beta of KDE 2. The sources were included on
the cover CD. KDE looked kinda nice. Less boxy and purple than the
only other *nix desktop I had seen, CDE. Until I finished my
undergraduate degree KDE was my go-to desktop.

## Getting Deeper

For a while I had reverted to using a tiling window manager and a
screenful of x-term. This was just a convenient way for me to get
through my PhD and my day job.

During my PhD I was studying Free Software community productivity
metrics. I was also working on research into software quality funded
by the European Commission. KDE eV (the governance body[^1] for KDE)
was also taking part in that project. At this time KDE was _almost_
ready to release KDE 4. It was an exciting time to get involved.

So I installed whatever the Debian stable KDE desktop
(3.102193393392393[^2]) of the time and really enjoyed the
experience. Having rediscovered my love for KDE and having met some of
the active community, I dived in deeper.

KDE became high on my list of projects to study during my PhD. The
community was going through major changes: not only was KDE 4 on its
way in, but KDE SVN was on its way out.

## Gitting Ready For Change

Around 2006 I discovered [Ade de
Groot's](https://en.wikipedia.org/wiki/Adriaan_de_Groot_(software_developer))
tool for visualising contributions to SVN; it was part of the English
Breakfast Network[^3]. His version of this tool utilised Python's SVN
bindings to read the repo data. Git instinct told me this tool would
work faster if it parsed SVN logs rather than read the repo data
through a library. I turned out to be right and this was a formative
moment in my career.

I created a generic SVN log parser for use by this visualisation tool
and used the same parser for other purposes; mostly other
visualisations and data plotting. The ultimate aim was to expose to
the KDE community what we could learn about social interactions within
the community from, arguably, its most important communication tool:
the version control system.

KDE SVN was[^4] truly enormous. It was pretty much the largest SVN
repo in the wild. One very large central repo which represented the
entire body of KDE code/artefacts. Around this time the strains of
using such a repo with such a huge (and growing) community were
prompting discussion about distributed VCS.

These were remarkably mature and structured discussions. Git was, by
no means, a foregone conclusion. Other distributed VCS were given
headroom and this was the first (and, basically, last) time I played
with Mercurial and Bazaar. The discussions were, for the most part,
very technical. I raised my voice to talk about the potential social
impacts of switching from SVN to distributed VCS. _Any_ distributed
VCS.

## Joining KDE eV

I spoke at Akademy and other KDE events (including the KDE 4 launch at
the Googleplex) about the research I was doing; either my PhD or the
EC-funded stuff. I blogged. I dented[^5]. My work was positively
received and gearheads would actively reach out to me for
more-detailed analysis of their corner of KDE.

I was encouraged to join KDE eV and I did. Given that I had made
precisely 0 code contributions[^6] to KDE this, to me, felt like an
achievement.

Since day one of my involvement, KDE eV had somewhat of an identity
crisis. It was really not 100% clear what it did... but anyone who
had been involved with KDE for more than 6 months was highly
encouraged to join. Before long it had become bloated; lots of members
contributing almost nothing and the few people wanting to do something
not getting enough support to do it.

KDE has switched to Git and the social changes were a-happenin'. The
KDE project was starting to lose its social cohesion. Post KDE
4.0-release blues, the switch to GIT and a lack of care from KDE eV
all contributed here. Other things, too. No one thing started the KDE
community's cohesion degradation. But we felt it. We even went though
a rebranding... KDE was not a desktop project, it had become a suite
of projects and the desktop was just one of them.

KDE had evolved and I had not.

## Cohesion Degredation?

One of the metrics I worked on during my PhD was a simple use of graph
theory to measure how well-connected a community is. The contribution
I made here was intriguing: as project get bigger they become less
cohesive, but through careful community management, luck and clever
structure, KDE avoided this. 

The last time I _properly_ attended Akademy (the KDE community
conference) was back in 2014. I'd been frustrated for some time with
my inability to drive home the message that the switch to GIT had o be
managed properly. I'd been frustrated that nobody seemed to have
noticed that my warnings were coming true.

So I gave [a
talk](https://conf.kde.org/en/Akademy2014/public/events/167) that
year.

Deep down, I knew this was my last public outing on behalf of KDE. It
was. After my talk a lot of people came up to discuss the mic I had
just dropped. But as the days and weeks passed after the event, the
message disappeared. And so did I.

## So Why Are You Telling Us This Now?

This year's [KDE conference](https://akademy.kde.org/2017) starts
tomorrow. Two of my all-time best buddy KDE community members reached
out to see if I was turning up.

They knew I wasn't.

While we briefly reminisced by email, one of them pointed out that my
talk from 2014 had recently come up in conversation on a KDE mailing
list. That, 3 years later, the talk was being used as part of a great
discussion about change in the project.

I'm really not sure what my emotion about that was. But, I did not
feel compelled to join the discussion. I did not feel a need to remind
people about what I was trying to achieve all that time
ago. Nope. Instead, I went and pushed some changes to a core plan I
had been working on for [Habitat](https://habitat.sh), the new home
for my free time.

## The Thanks

To all my friends in KDE:

Enjoy Akademy. Enjoy the opportunity to do some navel gazing. Enjoy
the food, the drinks, the sun. Hack. Break shit and put it back
together again. Remind yourselves of why KDE is special. Remind
yourselves of why it is important. Very important.

I thank you all for the time we spent together.

We were all part of the solution.

## Footnotes

[^1]: Countdown to flamewar... 3... 2... 1... I know many will object to me calling KDE eV a "governance body" but, no matter how you cut it, that _is_ what it is. At least it should be, imo.
[^2]: There were _approximately_ this number of KDE 3 releases.
[^3]: Is the EBN still a thing?
[^4]: Is?
[^5]: Is identi.ca still a thing?
[^6]: Thanks to the EBN, I did actually fix a spelling error in a comment in a .h file for Marble[^7].
[^7]: This makes me a true C++ h4xX0r, right?