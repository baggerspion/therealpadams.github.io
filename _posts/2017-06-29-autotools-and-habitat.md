---
layout: post
date: 2017-06-29 10:00
title: Autotools in the Habitat Context
categories: [Habitat, Autotools]
header:
  src: tools.jpg
  desc: Tools by Dorli Photography (https://flic.kr/p/8x4RCw)
---
_Personal Note:_ My first experience of the [GNU Build
System](https://en.wikipedia.org/wiki/GNU_Build_System) (a.k.a
"Autotools") was in cross-compiling GCC to run on SunOS[^1]. I learned
**a lot** about how to build software that day.

Twenty years later and Autotools almost looks "old hat". Most modern
languages have their own build chains and have no need for GNU's
venerable system, originally aimed at C programmers.

Even if you have never really made all that much use of Autotools
(perhaps even the name is only vaguely familiar) you _will_ recognise
the iconic process:

- ./configure
- make
- make install

Well, it's 2017 and Autotools _has_ survived the test of time. In this
blog post I present a brief introduction to the tools before going on
to show how they are used within the context of
[Habitat](https://habitat.sh) plan writing.

## Autotools 101

Autotools is a suite of applications from the GNU project which make
it easier to build software on different platforms. The primary tools
are:

- _Autoconf_: used to generate a `configure` script for automatically
  configuring the codebase for your platform;
- _Automake_: generates `Makefile.in` which is on the of the primary
  input for the `configure` script;
- _Libtool_: provides a consistent interface for accessing shared
  libraries.

Whilst not officially part of Autotools, I"m also going to throw the
following into the mix:

- _Make_: automated building of the application;
- _pkg-config_: provides interface for access to shared libraries at
  configure-time, as opposed to Libtool which provides the interface
  as `configure` is being generated.

I am not going into massive detail here because it is not needed. If
you'd really like to learn more about this toolchain, I highly
recommend Alexandre Duret-Lutz’s [Autotools
tutorial](https://www.lrde.epita.fr/~adl/autotools.html).

What is important is to know that this is a highly common build
system for those writing C/C++ and is pretty-much ubiquitous inside
the GNU project... including many of the `core/*` libraries already
available in Habitat.

## Using Autotools Inside Habitat

At the heart of your package building in Habitat is your `plan.sh`
file. In this you provide all of your metadata for the package
(e.g. packager's contact details, version number, origin etc), as well
as the specific details of how to build the software.

Most of the fine details of how to go about actually running the build
are contained within callback functions (e.g. `do_download()`,
`do_build()`).

If you want to get a feel for all of these callbacks and what they do,
run `hab plan init` and open the template `plan.sh` which has been
provided inside the generated habitat package folder.

For now, I'm not going to cover all of these, just the pertinent ones
for Autotools.

### Dependencies

A quick aside...

For any codebase that has been prepared using Autotools you will
probably want to include the following dependencies:

```bash
pkg_build_deps=(
  core/gcc
  core/make
  core/pkg-config
)
```

## Where Did `configure.log` Go?

## Footnotes

[^1]: Yup, you read that correctly. SunOS, not Solaris. This story takes place circa 1997.
