---
date: 2017-04-26 12:00
title: Source Verification Using GnuPG In Habitat
categories: [Habitat, GnuPG]
header:
  src: mask.jpg
  desc: Carnival de Limoux by tagon (https://flic.kr/p/Cn7pP)
---
My adventures into [Habitat](https://habitat.sh) continue. This time,
I am writing a little about how Habitat handles source verification
and, more importantly, how you can improve it to make use of
[GnuPG](https://www.gnupg.org).

## Why Verify Sources?

Typically, when we package a piece of software, we need a good idea of
*what* we are packaging. And it is important that we actually package
what we think we are packaging.

There are many simple solutions for this problem and one of the most
common is to provide a hash of the released sources. You've probably
noticed a checksum provided alongside a source tarball you have
downloaded. If you hash the tarball and the result matches the
provided hash, then the tarball is what it says it is. Simple!

Not just for packaging, but for many reasons, it is pretty important
that developers provide such a mechanism for verifying that a release
tarball really is what it says it is.

## Verification In Habitat

Verification of sources in Habitat could not be simpler. Someone at
the start of your plan, you are going to have something like this:

{% highlight bash %}
pkg_source="https://cdn.crate.io/downloads/releases/crate-1.1.2.tar.gz"
pkg_shasum="8f22b6531b3d1c8602a880779bbe09e5295ef0959a30aff0986575835aadc937"
{% endhighlight %}

After Habitat automagically downloads the source file for you, it will
check it against the checksum you provide. If they do not match, it
will stop the build cycle.

As a nice feature, Habitat caches the downloaded source file. So long
as it has a local copy of the file which matches the provided
checksum, it does not bother to download the file again. Why would it?
Nice.

So that's how you verify a source file in Habitat. We're done here?

Well... not quite.

## In Steps GnuPG

Habitat gives us this handy mechanism for checking that a source file
is what it claims to be. What we do not know: does this file come from
a trusted source? Anybody could have generated that tarball and
provided it.

One of the most common solutions for the "can I trust where this came
from?" question is for the producer of the source file to create a
signature for the file using GnuPG. If you are new to GnuPG and file
signing, you can learn some more [over
here](https://www.gnupg.org/gph/en/manual/x135.html).

Out of the box, Habitat does not provide a mechanism for checking a
source's signature. That said, it is super simple to achieve. Take a
look at this partial plan:

{% highlight bash %}
pkg_build_deps=(core/gnupg)

do_download() {
  # Download the source file, as usual
  do_default_download

  # Now also grab the signature for the source
  # Provide the checksum so that file does not get downloaded with every build
  download_file "https://cdn.crate.io/downloads/releases/${pkg_name}-${pkg_version}.tar.gz.asc" \
                "${pkg-name}-${pkg_version}.tar.gz.asc" \
		"307d2bde0cd2b840c0edcaba4c73bbb52fb6d1834c412d8d136705fba7f3ec42" 
}

do_verify() {
  # Firstly perform the standard checksum-based verification
  do_default_verify

  # Now verify the signature file
  verify_file "${pkg-name}-${pkg_version}.tar.gz.asc" \
              "307d2bde0cd2b840c0edcaba4c73bbb52fb6d1834c412d8d136705fba7f3ec42"
    
  # Now do the GPG-based verification
  build_line "Verifying crate-${pkg_version}.tar.gz signature"
  export GNUPGHOME="$(mktemp -d -p $HAB_CACHE_SRC_PATH)"
  gpg --keyserver ha.pool.sks-keyservers.net --recv-keys 90C23FC6585BC0717F8FBFC37FAAE51A06F6EAEB
  gpg --batch --verify ${HAB_CACHE_SRC_PATH}/${pkg_name}-${pkg_version}.tar.gz.asc \
                       ${HAB_CACHE_SRC_PATH}/${pkg_name}-${pkg_version}.tar.gz
  rm -r "$GNUPGHOME"
  build_line "Signature verified for ${pkg_name}-${pkg_version}.tar.gz"
}
{% endhighlight %}

Helpfully, Habitat allows us to overload some of its basic
functionality. In this case, I have overloaded the default approaches
to downloading sources and verifying them. In both cases, I start by
actually carrying out the default behaviour. Then the magic kicks in.

In the case of the do_download() overload, I have also called the
download_file function in order to get my hands on the signature file
that relates to the source I have downloaded. Use of downlod_file() in
this manner is important: it ensures the file will not be downloaded
with every single build so long as the checksum is correct. It is,
therefore, advisable to use download_file() over, say `curl` or
`wget`.

It is in the do_verify() overload where things get interesting. After
performing the default verification, a temporary folder for stashing
GnuPG config/output is created. Once this is done, it is super simple
to grab the public key of the signer and test the signature before
trashing the temporary config altogether. If either of these GnuPG
steps had not succeeded, the overall do_verify() would fail and, thus,
the build, too.

## What Next?

I'd love to see this, somehow, as core functionality within Habitat. I
guess a `pkg_signing_key=` option should not be too hard to implement;
portability is certainly not an issue given GnuPG's availability on
Windows and Mac as well as Linux.

I'm really not sure how elegant, or not, this solution is. I'm happy
enough that it works.

A big shout-out to the maintainers of the [CrateDB
Dockerfile](https://github.com/crate/docker-crate/blob/2b402ff0c49a3334e5e72ba9f1f55c23f05bd88b/Dockerfile)
from whom I lifted the core ideas for this approach.