---
layout: post
date: 2017-04-15 12:00
title: A New Habitat For Crate
categories: [Docker, Habitat, CrateDB]
header:
  src: field.jpg
  desc: Habitat by Batara (https://flic.kr/p/8pYqF6)
---
A few weeks back [I wrote
about](http://baggerspion.net/2017/04/habitat-intro/) playing with
[Habitat.sh](https://www.habitat.sh) after seeing the Chef team
present it at their Berlin meetup. After writing my own super-simple
Python+Flask service and writing a plan for it, I decided it was time
to up my game a little and create a Habitat plan for something a
little meatier.

It will probably be of no surprise that I decided to run with
[CrateDB](https://crate.io).

Actually, before CrateDB, I ran with Jenkins. But that is a story for
another day.

## I Love It When A Plan.sh Comes Together

Writing a plan for CrateDB did not turn out to be too difficult:

- There was an existing core/elasticsearch plan already in place
- Crate.io provides plenty documentation on how to configure CrateDB

I continue to be a hige fan of CrateDB and so, if I was going to write
a decent Habitat plan for CrateDB, it had to be an *epic* plan for
CrateDB and support as much of the CrateDB config as I could.

Of course, before getting to the config, I needed to get a plan
writen. Given I had an official ElasticSearch plan to hand, this was
never going to be too hard. Take a look...

{% highlight bash %}
pkg_name=crate
pkg_origin=therealpadams
pkg_version="1.1.2"
pkg_maintainer="Paul Adams <paul@baggerspion.net>"
pkg_license=('Apache-2.0')
pkg_source="https://cdn.crate.io/downloads/releases/crate-1.1.2.tar.gz"
pkg_shasum="8f22b6531b3d1c8602a880779bbe09e5295ef0959a30aff0986575835aadc937"
pkg_deps=(core/jre8)
pkg_bin_dirs=(crate/bin)
pkg_lib_dirs=(crate/lib)
pkg_exports=(
    [http]=network.port
    [transport]=transport.port
    [postgres]=postgres.port
pkg_exposes=(http transport postgres)
pkg_description="CrateDB is an open source SQL database with a ground-breaking distributed design."

do_build() {
    return 0
}

do_install() {
    install -vDm644 README.rst ${pkg_prefix}/README.rst
    install -vDm644 LICENSE.txt ${pkg_prefix}/LICENSE.txt
    install -vDm644 NOTICE ${pkg_prefix}/NOTICE
    install -vDm644 CHANGES.txt ${pkg_prefix}/CHANGES.txt

    mkdir -p ${pkg_prefix}/crate
    cp -a bin lib plugins ${pkg_prefix}/crate
    rm ${pkg_prefix}/crate/bin/*.bat
}

do_strip() {
    return 0
}
{% endhighlight %}

Nothing to controversial in there, but some things to note. The
CrateDB tarball contains everything it needs to run with the exception
of the JRE itself. So core/jre8 is provided as a runtime dependency
and most of the early steps in the plan (e.g. do_download(),
do_verify()) can be kept as the default. do_build() needs to be
overwritten because, in CrateDB, there is no Makefile to be
run. Instead, all I needed to do, just as with the core/elasticsearch
plan is install the tarball contents in the correct location and
remove any cruft.

## Monster Config

So that was the easy part; next I needed to wrestle with the CrateDB
config. One of the nice things about Crate.io and CrateDB is the care
taken over documentation. The config for the version of CrateDB for
which I am writing this plan can be found [over
here](https://crate.io/docs/reference/en/1.1.2/configuration.html).

That said, it was not quite as simple a religiously writing a Habitat
template for that config documentation. There appears to be a little
disparity between what is documented online and the default config
shipped inside the CrateDB tarball:

- Some config in the tarball is not documented at all
  (e.g. "discovery.zen.ping.unicast.hosts", a very common piece of
  config)
- Some config in the tarball has different syntax from the docs
  (e.g. "index.number_of_replicas" in the tarball, versus
  "number_of_replicas" in the docs).

I dedcided to produce a template that follows the online docs very
closely. I know there are definitely some config options missing from
those docs but, for now, I will leave them out of my template.