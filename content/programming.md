+++
title = "Programming"
date = 2020-04-21

[taxonomies]
tags = [ "programming", "Go", "Rust" ]
+++


The following content contains thoughts on programming mostly in Go and Rust.
I make no claim on these thoughts being original or coherent.
My hope is that this is somehow interesting.
Enjoy!

Let's start with why Go is a suboptimal language.
Go is preferable to many other languages,
but I increasingly find myself cursing its flaws.
Go has great support for concurrency,
on the one hand it gives your nice tools to do everything right,
on other hand in lures developers into creating inordinately complex webs of
channels and routines for what are essentially synchronous tasks.
Error handling is explicit,
which is an excellent reminder that unexpected things happen,
however errors can be simply ignored with an `_` as they are merely convention.
Go has one of the best garbage collectors implementations available,
but there are still plenty of ways to introduce unsoundness in the language,
not leaking is only part of the struggle.
Interfaces are great,
compose-able interfaces are better,
implicitly implementing interfaces is insanity.
Sometimes I just need Generics,
or even just macros,
instead Go gives me a templating library and go generate.

Now that that venting is over with,
it's worth considering why Go is so popular despite the frustrations.
The headline arguments are:
it's fast (relative to other GC'd languages),
it's a small languages (not so many incantations to remember),
there is already a library for everything,
it is established enough now that "enterprises" have caught up.
The honest answers are:
Go routines sound cool,
managers have hear of it,
the tool chain isn't massive pain to setup.

What do I actually care about when programming?
I don't care so much about performance,
at least not at first,
because that is premature optimisation.
I'm not that bothered about syntax,
as long as my editor has good language integration.
What I really want to know is:
can I build a mental model of what is going on without loosing my mind,
are there ergonomic ways to do common things,
will the code fail quietly at runtime.
That last thing especially,
I can live with pretty much anything,
but if it as obscure runtime behaviours I'm more likely to resent that tool.

Programs should fail early and loudly,
preferably before I've left my editor,
hopefully on build,
and almost never in production.
When Rust berates me for trying to do something unsound I thank it,
by nagging me at build time it has saved me production debugging stress,
hours invested fighting the borrow checker are worth every second.
With Go the feedback is often a bit more delayed,
waiting for unit tests or even integration tests before showing strangeness,
of course it being Go many bugs are racy (-race doesn't catch everything).
Well at lease compiled languages are a step up from interpreted languages,
days spent attaching IRB to poorly maintained Ruby apps are best avoided,
interactive debuggers (Delve, rust-lldb) are more than enough.

Code should be in declarative style,
imperative code is reserved for edge cases.
Rust strikes a nice balance,
most thinks can be down very clearly declaratively,
while I still have the option to write imperative code as required.
Objects should be immutable by default,
having to say I want this to be mutable encourages good design,
most of the time readability is a higher priority than performance.
Unit tests should be extensive,
most dumb manual checks done during development should become an automated test,
it save you time, me time, and makes the test gods happier.

Software these days rarely exists in isolation,
integration test everything exhaustively,
or prepare for a game of debugging in production.
Staging environments are not integration tests,
staging is where you hope to catch holes in your tests.
If tests "take to long" your development process is probably broken,
otherwise think about parallelising tests.

This concludes my jumble of thoughts for the moment.
Happy hacking!
