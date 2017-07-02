---
layout: post
date: 2017-07-08 10:00
title: Autotools in the Habitat Context
categories: [Habitat, Autotools]
header:
  src: tools.jpg
  desc: Tools by Dorli Photography (https://flic.kr/p/8x4RCw)
---
Well... it's 2017 and Autotools has survived the test of time. In this
blog post I present a _very_ brief introduction to Autotools before
going on to show how they are used within the context of
[Habitat](https://habitat.sh) plan writing.

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

What is important is to know that this is a highly common build system
for those writing C/C++ and is pretty-much ubiquitous inside the GNU
project... including many of the `core/*` libraries already available
in Habitat. In this blog post I am not going to write about how to
generate a create the build chain for your code using
Autotools. Instead, I focus solely on how to build Autotools-enabled
applications using Habitat.

## Using Autotools Inside Habitat

At the heart of your package building in Habitat is your `plan.sh`
file. In this you provide all of your metadata for the package
(e.g. packager's contact details, version number, origin etc), as well
as the specific details of how to build the software.

Most of the fine details of how to go about actually running the build
are contained within [callback
functions](https://www.habitat.sh/docs/reference/plan-syntax/#callbacks)
(e.g. `do_download()`, `do_build()`).

If you want to get a feel for all of these callbacks and what they do,
run `hab plan init` and open the template `plan.sh` which has been
provided inside the generated habitat package folder.

For now, I'm not going to cover all of these, just the pertinent ones
for Autotools.

### Build Dependencies

A quick aside...

For any codebase that has been prepared using Autotools you will almost certainly need to include the following dependencies:

```bash
pkg_build_deps=(
  core/gcc
  core/make
  core/pkg-config
)
```

### The Callbacks

In general, when using Autotools, the only callbacks we generally care
about are:

- `do_build()`
- `do_check()`
- `do_install()`

For `do_build()` we need to invoke `./configure` and `make`. More
often than not, `./configure` takes many options, the most common
being `--prefix`, which is used to determine the installed location of
the compiled software. Inside a Habitat artefact, our usual install
location is `$pkg_prefix`, making a typical `do_build()`:

```bash
do_build() {
  ./configure --prefix="$pkg_prefix"
  make
}
```

The other _fairly_ common option flag for the configure script is
`--disable-static`. When building libraries, Autotools usually
defaults to building both static and dynamic versions of the
library. In the context of Habitat, we can live without static libs,
so using this flag is recommendable. If you do not know the full set
of flags available to `configure`, you can run it using the `--help`
flag. You will find the script in /hab/cache/src/.

At this point our code is built. We might want to run some test before
install though, right? For this, we have the `do_check()`
callback. There is no default implementation for this, however the
most likely commands for running tests will either be `make test` or
`make check`.

Finally the install. Without fail, the magic incantation for this will
be `make install`, which also happens to be the default implementation
of `do_install()`.

### Sensible Defaults

So, to recap on callbacks, the default implementations are:

```bash
do_build() {
  ./configure --prefix="$pkg_prefix"
  make
}

do_install() {
  make install
}
```

If you have these exact hooks in your plan.sh, you can safely remove
them. This makes your life easier and greatly reduces on maintenance
load!

## Where Did `configure.log` Go?

So that's it? That's all there is to build Autotools-enabled
applications/libraries?

Well... no.

There's two things we probably (almost definitely) still need to do.

### Check I've Configured This Correctly

After your first build (successful or otherwise), you should head into
/hab/cache/src/ and look at the `configure.log` file for your
source. This is the output of the configure scripts and lets you know
of any flags it recommends you setting or dependencies that it thinks
is missing. Which leads us nicely onto...

Another thing to look out for in your configure script is if it
actually executed successfully. For reasons I never could understand,
you will sometimes find errors like[^2]:

```bash
configure: WARNING: 'missing' script is too old or missing
```

Despite a script/command not being available, `configure` often does
not baulk at this and will still successfully exit.

For any dependency that is missing you must include it
`pkg_build_deps` (`pkg_deps` if you'll need the dep at runtime). Even
then you might have a problem of a hardcoded path inside the
`configure` script. You can correct this using a symlink in the
`do_prepare()` callback and then remove than symlink in the `do_end()`
callback, e.g.

A common culprit is `file` not being available because the
configure script has it hard-coded to be in /usr/bin/file. 

```bash
do_prepare() {
  if [ ! -e /usr/bin/file ]
  then
    ln -sv "$(pkg_path_for core/file)/bin/file" /usr/bin/file
  fi
}

do_end() {
  if [ -e /usr/bin/file ]
  then
    rm /usr/bin/file
  fi
}
```

### My Dep's Dep

The output from `configure` will be pretty explicit if not all your
dependencies are in place, e.g.

```bash
checking for a usable sed... configure: error: no usable version of sed found
```

In Habitat it is important that the _entire_ tree of dependencies for
your application appear in either `pkg_deps()` or
`pkg_build_deps()`. Thankfully we have `pkg-config` to help us here.

If your application is not building because you suspect that your
dep-tree is not complete there is a solution:

- Establish which dependencies `pkg-config` currently knows about:

```bash
hab pkg exec core/pkg-config pkg-config --list-all
```

This should return a list of dependencies which matches what is
currently included in your `plan.sh` script.

For each of these you can then establish if all of its own
dependencies have been met:

```bash
hab pkg exec core/pkg-config pkg-config --print-errors <name of pkg>
```

You may need to run this command a few times per dependency as it will
only return the first missing dep each time. Stick with it! It will
eventually return 0, I promise!

## Conclusion

So that's it for a this very high-level intro to Autotools in the
context of Habitat. To recap:

- You might not need to write any callbacks, thanks to Habitat's
  sensible defaults.
- Use `configure --help` to understand what build-time configuration
  options are available for the codebase you are packaging.
- Use of `pkg-config` can help ensure all your dependencies are in
  place.

If you'd like to see a worked example, take a look at the [plan for
R](https://github.com/habitat-sh/core-plans/blob/master/R/plan.sh). It's
good[^3].

## Footnotes

[^1]: Yup, you read that correctly. SunOS, not Solaris. This story takes place circa 1997.
[^2]: If you know of a more "meta" warning than this, please send your screenshots to... actually, never mind.
[^3]: Even if I do say so myself.