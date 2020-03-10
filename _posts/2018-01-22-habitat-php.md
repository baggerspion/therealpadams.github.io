---
date: 2018-01-22 22:30
title: Habitat Core/PHP, The Story Of The PR
categories: [Engineering]
image: core.jpg
---
Today I finally got around to submitting [my first PR for
2018](https://github.com/habitat-sh/core-plans/pull/1146). It's part
of a greater plan that I am cooking around Habitat; the
re-implentation of the `core/drupal` package. That's a story for
another day. Today I wanted to write a little about _this_ PR and its
part in the greater picture.

## The Existing Core/PHP Package

The current implementation of core/php is a pretty simple, plan-only
affair:

<details>
<summary>Click here to see the plan.</summary>
{% highlight bash %}
pkg_name=php
pkg_distname=php
pkg_origin=core
pkg_version=7.1.4
pkg_maintainer="The Habitat Maintainers <humans@habitat.sh>"
pkg_license=('PHP-3.01')
pkg_upstream_url=http://php.net/
pkg_description="PHP is a popular general-purpose scripting language that is especially suited to web development."
pkg_source=https://php.net/get/${pkg_distname}-${pkg_version}.tar.bz2/from/this/mirror
pkg_filename=${pkg_distname}-${pkg_version}.tar.bz2
pkg_dirname=${pkg_distname}-${pkg_version}
pkg_shasum=39bf697836e2760b3a44ea322e9e5f1f5b1f07abeb0111f6495eff7538e25805
pkg_deps=(
  core/coreutils
  core/curl
  core/glibc
  core/libxml2
  core/libjpeg-turbo
  core/libpng
  core/openssl
  core/zlib
)
pkg_build_deps=(
  core/bison2
  core/gcc
  core/make
  core/re2c
  core/readline
)
pkg_bin_dirs=(bin sbin)
pkg_lib_dirs=(lib)
pkg_include_dirs=(include)
pkg_interpreters=(bin/php)

do_build() {
  ./configure --prefix="$pkg_prefix" \
    --enable-exif \
    --enable-fpm \
    --with-fpm-user=hab \
    --with-fpm-group=hab \
    --enable-mbstring \
    --enable-opcache \
    --with-mysql=mysqlnd \
    --with-mysqli=mysqlnd \
    --with-pdo-mysql=mysqlnd \
    --with-readline="$(pkg_path_for readline)" \
    --with-curl="$(pkg_path_for curl)" \
    --with-gd \
    --with-jpeg-dir="$(pkg_path_for libjpeg-turbo)" \
    --with-libxml-dir="$(pkg_path_for libxml2)" \
    --with-openssl="$(pkg_path_for openssl)" \
    --with-png-dir="$(pkg_path_for libpng)" \
    --with-xmlrpc \
    --with-zlib="$(pkg_path_for zlib)"
  make
}

do_install() {
  do_default_install

  # Modify PHP-FPM config so it will be able to run out of the box. To run a real
  # PHP-FPM application you would want to supply your own config with
  # --fpm-config <file>.
  mv "$pkg_prefix/etc/php-fpm.conf.default" "$pkg_prefix/etc/php-fpm.conf"
}

do_check() {
  make test
}
{% endhighlight %}
</details>

- Preamble
- Dependencies
- Build / install instructions
- Run some tests

No rocket science here. But one major assumption: that the user wants
to import PHP as a library, rather than run PHP-FPM as a service.

I want to run PHP-FPM as a service. Or, at least, I want others to run
PHP-FPM as a service when they want to. Thankfully, Habitat allows me
to easily service-ify[^1] an existing package without disturbing the
existing users of that package[^2].

## The PR Content

So let's start with the obvious change I needed to make to the plan.sh
for core/php: adding the instruction for how to run PHP-FPM as a
service.

{% highlight bash %}
pkg_svc_run="php-fpm --fpm-config ${pkg_svc_config_path}/php-fpm.conf -c ${pkg_svc_config_path}"
{% endhighlight %}

Here I provide the name of the script to run and its required
options. The `-c` might be[^3] completely superfluous in the presence
of the `--fpm-config` option. So now, for anyone invoking `hab start
core/php` they will get a running PHP-FPM service. Right? Wrong. This
is only the beginning of our servicification[^4].

### Some Other Changes To Plan.sh

So what else needs to be done before this is a healthy service? A
couple of things:

- We should expose the port that is being used;
- libreadline needs to be shifted from being a build dep to a runtime
  dep[^5];
- We can do-away with the the whole `do_install()` function.

Why?

That do_install() served only one purpose, to put the default (is
provided during the build) PHP-FPM configuration in situ after the
build is completed. The problem here is that the Habitat Supervisor is
not in control of this config. So let's Habitat the heck out of this.

Starting, of course, with completely removing that do_install()[^6].

### A More Habitat-y Configuration

Hopefully you noticed that the plan.sh `pkg_svc_run` directive[^7]
included the `${pkg_svc_config_path}` helper[^8]. To successfully
provide this configuration, I only need to do two things:

- Drop a template for PHP-FPM into my build's /config folder;
- Create a default.toml to include default values for the most
  meaningful configuration options[^9].

I won't bore you with the details of PHP-FPM configuration. Gratuitous
Googling tells me that the options I have provided for _are_ the most
important in this case.

### One. Last. Thing.

PHP-FPM is run as `hab:hab` and we need to give that user the
permission to write its logs into `${pkg.svc_var_path}`. This is easy
enough to achieve with an init hook which creates
${pkg.svc_var_path}/log and chown's it.

## Fin.

So there you have it, the anatomy of a PR. And, in particular, how you
can easily turn a library into a service. I'm really not sure how
common this use case is[^10].

As I write this, [the
PR](https://github.com/habitat-sh/core-plans/pull/1146) is not yet
merged. However, if/when it is merged, all downstream packages based
on core/php should be unaffected and all new packages can make use of
core/php as a service: `hab start core/php`.

So now onto a new `core/drupal` which will do just that. Hopefully.

---

### Footnotes
[^1]: This is totally a word, right?
[^2]: Too much.
[^3]: Probably is.
[^4]: If we accept "service-ify", we accept "servicification", OK?
[^5]: Something I discovered only after I had loudly declared "Aha! I'm done!" in the office.
[^6]: I love removing code. Maintainance is way more costly that development.
[^7]: I totally made this up. I have no idea what to call this.
[^8]: Yup, I made up this name, too.
[^9]: The "Habitat Way"[^11] is to keep core/* package configuration small and meaningful. Not overly verbose.
[^10]: Not very.
[^11]: &trade;
