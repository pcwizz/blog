+++
title = "Mutterings on mutt"
date = 2016-07-07

[taxonomies]
tags = [ "mutt", "email", "GNU/Linux", "workflow" ]
+++

WARNING: THIS IS NOT A TUTORIAL FOR N00BS, A HAND HOLDING OR A BUG REPORT IT IS
A DOCUMENTATION OF EXPERIENCE. IF I INTENDED IT TO BE ANY OF THOSE THING I WOULD
HAVE WRITTEN AND PACKAGED A SCRIPT, CHARGED YOU FOR CONSULTATION OR FILED A BUG
REPORT IN THE TRACKER. IF YOU DON'T UNDERSTAND SOMETHING READ THE MANUAL AND IF
YOU STILL DON'T UNDERSTAND DON'T DO IT!

## Introduction

A couple of weeks ago the heavily sand boxed and customized version of
Thunderbird found on Subgraph decided it didn't want to start any more. I
attempted and failed to get it working again, although I had not tried very
intently, as I really don't expect GNU+Linux systems to work (that's why this
server runs FreeBSD). I gave up on Thunderbird, and decided on trying mutt, as I
had been meaning to experiment with mutt for a long time. This is the story of
my experience with mutt on Subgraph (which is based on Debian testing): a tale
of obscurity: an experiment in documentation; I'm writing this article so you
might not struggle as I did to make mutt work.

## Configuration

The mutt configuration file is a simple key value type file with variables and a
couple of eccentricities. On the whole mutts own documentation on the
configuration file options and syntax is perfectly sound and easy to understand
to anyone who has edited a *nix configuration file before. If the configuration
file syntax is holding you back then you aren't ready to be using a console
based email client.

The key things your going to want to put in in your configuration file are you
name, email address, imap and smtp server details. Mutt's own documentation will
get you this far without hassle. You will probably also want to set a couple of
things like requiring TLS on connections to your servers and if you trust your
operating system and full disk encryption enough you can put you passwords in
there too.

## SASL

If your running Debian testing, you installed mutt from the repo and your server
uses SASL for authentication then your configuration isn't going to work yet.
Your going to have to install libsasl2 from the repository because it's not a
dependency of the mutt package, which is correct because if your not
authenticating with SASL mutt will work just fine or if you are using an
external mailer as opposed to mutts internal mailer mutt will work just fine.

Make sure libsasl2 is installed before you start researching error messages with
little success. It might be a useful contribution to add libsasl2 to the list of
optional mutt dependencies in the package; I can't remember whether I have a
Debian account or not and I'm too busy/lazy at the moment to check; I figure at
least having an article on the net might help the average mutt user suffering
from this issue.

## GPG

There is plenty of guff about setting up gpg with mutt mostly involving copying
some configuration from the internet into a file then including it in your main
mutt configuration file. It probably works if you store your keys on your
computer and protect them with a password or something, but if like me your keys
live on a nitrokey protected with a pin, however it isn't going to work without
an intense amount of fiddling for the paranoid -like me-.

In Debian testing at least all of these instructions you find on forums
(including the Debian forums) can be safely ignored in favour of using code
already baked into the mutt package. GPGME is a API for GPG that is a bit crusty
but otherwise functional and mutt can be compiled to use it as it is in the
Debian testing repository and many other distributions (so I've heard).

All you need to do to get GPG working with mutt is enable gpgme in you mutt
configuration as described on the Arch wiki entry for mutt. You will most likely
want to set automatic signing and encrypting of replies.

## HTML

I don't read HTML emails. When I get HTML emails I respond to them asking for
the same information in plane text. HTML doesn't play nice with PGP and I don't
understand what purpose they serve. If you need to send something formatted send
it as an attachment or like to it in the email body.

If you don't share my enlightened thinking I believe there are simple methods of
dumping HTML mail with external programs. I wouldn't bother though.

## Keyboard short cuts

The defaults seem fine for me, so I haven't changed them, I prefer to minimise
this sort of customisation to ensure I can still use other peoples set ups (if
required) even if I lose a tiny amount of potential work flow optimisation. The
question mark is the most important key to remember, it gives you a list of key
bindings in that view (mode or whatever they call the difference screens in
mutt), if your a vim user then your going to feel fairly comfortable rather
quickly.

## Vim

My chosen editor is vim, I use vim for everything that requires text to be
edited, it just works. If you like you could have mutt specific vim some
additional configuration using file types. The mutt page of the arch wiki covers
configuration vim for mutt. You might enjoy spell checking and 72 character text
width, however my vim configuration already has easy access to spell check and
I'm unlikely to ever send a table or something that is sensitive to text width
in an email body, the option is their if you want it.

## Multiple email address and PGP keys

I haven't bothered setting this up yet. All my mail is redirected into the
account I've set up and I'm only using PGP with that account at the moment. I
will at some point add the multiple outgoing email accounts. A task for some
spare cycles will be to create a key pair for my fsfe.org account on my
fellowship smart card. I have spotted a couple of ways to do it in the
documentation, but setting up keys to satisfy manner my paranoia finds
acceptable takes a good hour, so it might happen within a couple of weeks.

## Tor all of the things

Tor is also on my list of things I should configure mutt to use. I don't think
mutt's built in MTA understands proxing, so I have two options, I will have to
use an other MTA or wrap mutt in torsocks. I'm not entirely happy about the
prospect of giving precedences to a folder in my home directory in my path
variable, so I'll probably be able to live with typing "torsocks mutt".
