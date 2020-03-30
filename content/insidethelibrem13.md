+++
title = "Inside the librem 13"
date = 2016-03-18

[taxonomies]
tags = [ "hardware", "laptops", "purism" ]
+++

## Disclaiming bias

I have already taken the bottom my librem 13, as I like to act like the average
prole, so I can give an initial report on the internal layout and wire runs.

To be honest I took the bottom off to upgrade the memory and slot in an ssd, but
I thought it might be some next level autism to write about it. I'm not really
an expert on PCB design or laptop internal layouts, but I have taken lots of
things apart.

My favourite laptop for build quality is in fact my x200, as everything was
covered in a plastic dust/splash proof wrap, meaning everything had remained
extremely clean despite it being many years old. I don't really expect anything
other than thinkpads to compare to the exceptional attention to detail found in
the x200

I did spend a couple of summers during college/high school years churning out
workstations and servers wich is also why I have an unhealthy obsession with
cable ties, cable tension, bend radius and screw types. People very easily
overlook important factors such as cable tension tending towards over taught
cables that are both a pain in the ass to perform maintenance on and degrading
to the cables lifespan. That said I have no formal training in the matter only
habits I have contracted from fellow neckbeards or formulated myself.

## Initial inspection

It looks like pretty much any other modern laptop internal: A compact
motherboard situated in the top left, a respectable battery tacking up the
entirety of the palm rest, a single slot for sdram, a minipcie (populated with
the wifi card), a 2.5" sata drive and an m.2 sata connector to the right. As you
would expect of an i5 their is a fan positioned in the middle exhausting heat
out of a rear vent from a thick copper heat pipe. The rear vent appears to be
unfiltered and run right the way along the back of the case. The heatsink had a
small block of what looks to be thermal transfer foam to utilize the metal
bottom cover; the bottom of the laptop gets warm.

There was a fair bit of that matt black tape in use to keep wires in place,
mostly around the wifi module. It seems that some of the cable could have been
made a little shorty for tidiness, but it might be better in the long term for
maintenance. The wires for the cmos battery were not afflicted by the tape but
had been looped affectionately round the connector in one elliptical coil; I
dare not touch it for fear that it would never go down again.

The screws attached the board to the case seemed to be small black painted
screws. I didn't look around closely for a ground other than the one for the
wifi card which was a wire with the end trapped under a screw, not a screw one
would ever have reason to remove so it's fine with me.

## Populating the M.2 sata

It's a standard insert at 45 degrees then lower to horizontal and screw affair.
The only niggle is that they don't seem to populate the screw hole if you don'
order it with a M.2 sata drive; I had to find my own screw.

## Changing the RAM

They had put some kingston memory in there which is fine; I just wanted more
than I had ordered with the machine. There is only a single dim slot that works
in the way you think it does if you have changed ram in a laptop before. I went
for a 1.35v dim I think that is what was in there I didn't check.

