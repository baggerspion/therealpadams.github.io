---
layout: post
date: 2017-02-11 12:00
title: Playing With The Nextcloud Docker Experience
categories: [Docker, Free Software]
header:
  src: clouds.jpg
  desc: clouds by Robert Meeks https://flic.kr/p/abTYg
---
Ever since its roots as ownCloud within the [KDE](http://www.kde.org)
project, I have always been fan of Nextcloud. On a personal level, I
am also a fan of the team behind it. Back in my [Kolab
Systems](http://kolabsys.com) days I even advocated an integration of
Nextcloud (then ownCloud) and Kolab as strong, Free alternative to
Sharepoint/Google Apps.

Since I am currently [looking for new work](/hire/index.html) I wanted
to spend some time improving some skills. In this case, learning more
about building Docker containers.

## The Nextcloud Docker Container

I picked Nextcloud to work with because it is non-trivial, but not a
nightmare, to setup from source. There is also an existing [official
Docker image for Nextcloud](https://hub.docker.com/_/nextcloud/) which
gave me something to work from.

Easy enough to get hold of: ```docker pull nextcloud```

Simply running this command immediately gave me an idea of where I
might learn a ting or two: the image is over 600mb! That is
massive. Unnecessarily massive.

So there was my challenge:

- Learn how to write Dockerfiles and create my own images
- Emulate and shrink the Nextcloud official image

Docker's own [documentation for writing
Dockerfiles](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/)
is pretty good and was certainly helpful while I tried to comprehend
the
[Dockerfile](https://github.com/nextcloud/docker/blob/master/11.0/apache/Dockerfile)
for the official Nextcloud image.

### Immediate Thoughts

On first glance, there were a couple of things that immediately struck
me as things that might be improved:

- Complex base image
- Use of pecl

#### The Base Image

The Nextcloud image is based upon [an official PHP
image](https://github.com/docker-library/php/blob/75854bb7e97d3e01c9b3597674450be43521e296/5.6/fpm/Dockerfile)
which, in turn, has [Debian Jessie](https://www.debian.org) as a base
image. I wanted to change this to Alpine Linux because it has a tiny
base image (it is under 4mb!). This, however, forced me to look into
how Alpine packaging works.

#### Use of pecl

The Nextcloud image uses [pecl](https://pecl.php.net) to build/install
some PHP extensions for Redis, memcached and apcu. For many reasons, I
wanted to switch this to using the base distribution's own packaged
version. Most importantly, I knew very little about pecl and how (if
at all) it cleans up sources after building them.

## The Results

My Dockerfile can be found on
[Github](https://github.com/therealpadams/nextcloud-docker/blob/master/Dockerfile). At
this stage it is still not complete enough to use (although it does
provide a fully-working Nextcloud instance), so I am not going to make
it available on [Docker Hub](https://hub.docker.com). That said, even
incomplete, there are some nice results:

### Huge Size Reduction

My original mission was to see if I could reduce the enormous 600mb+
image size. Given my lack of experience, I would have been happy to
shave off a few mb. I never expected this...

[![Docker Image Size Comparison](http://res.cloudinary.com/baggerspion/image/upload/v1486835095/Images/image-size.png)](http://res.cloudinary.com/baggerspion/image/upload/v1486835095/Images/image-size.png)

My image (at the top) represents a ~45% disk usage reduction over the
official image!

### Not Production Ready

Most of the size benefits came from using Alpine as my base image, I'm
sure. I *suspect* there was some benefit in ditching pecl, too. Here
be dragons...

In order to ditch pecl, I used Alpine native packages for the PHP
extensions. Sadly, two of these packages
([redis](https://pkgs.alpinelinux.org/package/edge/testing/x86_64/php7-redis)
and
[memcached](https://pkgs.alpinelinux.org/package/edge/testing/x86_64/php7-memcached))
are only available in the *testing* repository for Alpine Edge (not
the stable distribution). So the benefits of native packaging are
totally offset by the overall image *potentially* being less stable.

Alpine Edge also does not seem to have a php7-imagick package. So I
had to borrow the php5 package. I assume this does not entirely work.

## Have A Play

[![Docker Nextcloud Result]({{ site.imgroot}}Images/nextcloud-result.png)]({{ site.imgroot }}Images/nextcloud-result.png)

So this work is not entirely finished:

- Make use of PHP-fpm as advised [here](https://wiki.alpinelinux.org/wiki/Apache_with_php-fpm)
- Poke the Alpine Linux community for a php7-imagick package
- The entrypoint script needs hardening
- Create a volume for any local data (notably Sqlite)
- Add support for HTTPS and kill plain HTTP

The stuff *I* can do will really not take very long; I should get that
wrapped up in the next couple of days. However, we will need to wait
for Alpine Edge to stabilise before I could actually recommend anyone
using my Dockerfile in production.

Meanwhile, feel free to have a play with my Dockerfile and let me know
what you think!