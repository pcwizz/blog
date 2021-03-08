+++
title = "Embedded Linux: A rant about Bitbake"
date = 2021-03-08

[taxonomies]
tags = [ "linux", "embedded", "yocto", "bitbake", "OpenEmbedded"]
+++

This blog post is me blowing off steam but maybe it is entertaining. Maybe you
also have this pain and we can form some in of support group for for those
affected by Bitbake or build systems more generally.

At my day job I spend a reasonable amount of time fighting with Bitbake.
Bitbake for those of you lucky enough not to maintain an Embedded Linux
distribution is the de facto tool for developing Linux distributions targeted
at embedded devices. Typical use cases range from space craft to loud speakers
despite Bitbake being a pain in the arse it is rather prolific and arguable the
right tool for some of those jobs. Typically one doesn't just use Bitbake, to
speed up development a bit, one uses components from the Yocto/OpenEmbedded
projects and/or some commercially supported bits and bobs.

## Layers

Bitabake build recipes that tend to output packages or file system images.
These recipes contain tasks Bitbake has a standard set of tasks that it
executes in a set order (e.g. `do_compile`, `do_install`) although one can
inject tasks anywhere and/or inject code into existing tasks; more on that mess
later. Recipes can import classes to get standard implementations of tasks for
example there is a class for Automake so writing recipes for projects that use
Automake is relatively easy. The most important feature however is the ability
to layer collections recipes on top of each over using Bitbake layers.

Layers are collections of recipes and/or modifications to recipes. The order of
the layering is controlled by the order they appear in the `bblayers.conf`.
Layers a kinda similar to layers in a Docker file but instead of just adding
files they inject arbitrary code into your build system. Commonly one starts of
with `poky` a layer from the Yocto project that contains all the recipes
required for a bootable system (not necessarily on your target board), adds
some other layers from The OpenEmbedded project to fluff out one's userland and
the board support package layer from one's SoC vendor, and lastly one's own
layer which which one tweaks things and blends everything together into an
image recipe that can be flashed to the board or shipped as an update bundle...

By the time you have a working Linux system on your board you have anywhere
between 5 and 20 layers. A change in any of these layers can impact any layer
above it in a variety of frustrating ways. If you targeting more than one board
and having more than one flavour of your distribution you will quickly learn to
expect collateral damage from layer imports. Lot's of layers love to touch
recipes like uboot and Linux. It is always nice to find out one your target
boards no longer boots because the layer you imported to do something a in a
flavour that runs on another board changed Squashfs from a built in to a
module...

Of course developer frustration isn't the only issue with layering arbitrary
Python/Bash scripts on top of each other. All of these layers come from from
public git repositories (unless you are using some commercially supported
layers) maintained by the community. While if fairly certain non of these
community maintained layers is intentionally malicious there is an undeniably
large supply chain risk associated with using them. Just one recipe you use in
one layer you use has to be be altered and the next time you update that layer
you image is compromised and you need to rebuild your build infrastructure
because god knows. You need to be monitoring the changes to the layers you use
but the complexity of the system doesn't make it easy to see how upstream
changes interact with your system. Most layers don't even sign commits so good
luck securing that supply chain by trust.

Supply chains are probably the least of your security problems of course. It
isn't like the versions of things like OpenSSH lag weeks behind security
updates, because you are relying on upstream to patch then to update the
upstream layer in your build pipeline and then to QA it mixed in with whatever
changes you have made. Good job no one connects IoT devices directly the
internet isn't it...

## `_append`

What the fuck? Seriously!

So in Bitbake you can put `_append` on the end of any function of variable and
literally append an arbitrary string to the end of it. What to prepend well
that's no problem either `_prepend` is the suffix for you. When Bitbake parses
a recipe it spits out a shell script for each task rendering in all of the
prepends and appends. While your at it you can also specify your target with a
suffix like `_intel-corei7-64` Which is ugly but compared with the rest of
Bitbake  mild irritation.  The `+=` operator is available or you can inject
your own tasks anywhere in the order you like. Why would you do that it boggles
the mind.

## If it's not automake it's a pain

This is possible more of a comment on new languages deciding to reinvent
dependency management in ever more broken ways. Whether it is Go up until
relatively recently being very picky out file paths, a rust projects tendency
to depend on half of crates.io and don't get me started on JavaScript. Bitbake
hasn't adapted well to this trend of language having their own dependency
management tooling that usually wants to grab things from the Internet and in
Rust's case run arbitrary code during the compile. I could be grumpy about the
modern languages ruining everything with their developer friendly tooling, but
their tolling is pretty nice if you are not trying to build an embedded Linux
image with poorly aging tooling.

## You probably don't need your own Linux Distribution

If your application doesn't need a custom kernel or boot loader hacks, you
don't need your own distribution. Do your self a favour and buy into a
commercially supported IoT distribution. There a plenty of options out there
for Embedded Linux you can just ship containers to such as BalenaOS or Ubuntu
core. Shipping containers is much less painful as a development process, there
is plenty of tooling available for scanning and securing them, and it is
significantly easier to hire a Dev who can build a container image than one who
knows a lot about software packaging.

Even if you do need your own distribution consider basing of a mainstream OS
like Debian and just patching and rebuilding their packages. It is much simpler
to deal with changes from one source than to keep up with variations in 10. You
might also find it easier and cleaner to upstream your changes where they are
generically useful.

## A note on other Linux toolchains

I haven't tried Buildroot or any of the other projects for making Linux images.
I'd be interested to know if they are worth looking into, but I'm wedded to
Yocto at the day job for the forcible future.
