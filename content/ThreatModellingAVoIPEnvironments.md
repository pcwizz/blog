+++
title = "Threat modelling AVoIP environments"
date = 2024-06-09

[taxonomies]
tags = [ "AVoIP", "broadcast", "multicast", "AES67", "SMTPE 2110", "Art-Net", "security", "networking", "theat", "risk", "assement", "modelling", "ProAV"]
+++

In order to asses wether something is appropriately secured we must first
understand which threats we are defending against. This posts uses STRIDE and
DREAD to asses some of the typical the threats posed in an AVoIP environment.
the If you are not familiar with AVoIP you may wish to read my first post
introducing the topic.

## Spoofing

Spoofing is the ability to pretend to be something or someone other than
yourself. What this means in an AVoIP context is for example one microphone
pretending to be another or even some other device pretending to be a
microphone. In an ideal world we would desire the ability to verify the
authenticity of a device.

> Is this really the microphone a the lectern or is it some other device on the network masquerading as the audio source we seek?

In addition to the authenticity of devices we also want to be confident of the
authenticity of streams on the network. Once we have established that a device
is authentic we would also like to know that the stream containing all of the
audio channels from stage left is authentic and not another stream attempting to
deceive us.

Once we have established that the streams are authentic we are also concerned
that the data in those streams is authentic. If someone else can impersonate the
same stream with the same parameters as the legitimate stream we would like to
able to distinguish the legitimate from the illegitimate data and only work with
the legitimate data. I have given a talk about the opportunities to carry out
such an attack on Audio over IP protocols.

The real world consequences of spoofing could cause quite some chaos in many
AVoIP deployments. If someone mixing media streams can’t trust their
authenticity then the output is also potentially inauthentic.

Current AVoIP technology doesn’t do anything technical to address threats from
spoofing. It is assumed the network perimeter is secure and it is the
responsibility of the operator to select the correct streams from the correct
devices. It is a far from ideal state of affairs, however the madness that would
ensue would lead to relatively swift detection and remediation; an operator
would likely hear/see the difference.

## Tampering

Tampering is about the integrity of of our AVoIP streams. Can we trust that the
data sent (from an authentic source) is the data we receive and use. Tampering
can be done from a person in the middle position such as a switch or a router.
Mixing consoles and other processing devices tamper with content by design
however we expect this as intended behaviour so in this context we will place
such devices out of scope.

If an attacker can alter the content of our streams in flight across the network
we can’t rely on the integrity of what we are receiving. The implications of
this could be that a presenter is saying one thing and a different thing is
being played out to the audience.

Most smaller AVoIP networks don’t route streams so there would only be
relatively low power switches on the network with which to carry out this
attack. Larger installs for example at broadcasters definitely do have capable
routing hardware. There is also the option that the attacker gains physical
access to insert themselves a person in the middle.

Current AVoIP technology doesn’t have any technical counter measures to prevent
tampering and protect the integrity of the content. The integrity and security
of the network is assumed. Carrying out such an attack would likely attract
attention, however used subtly or burnt once for the right motivations it is
possible to imagine this capability being useful for someone.

## Repudiation

Repudiation is about wether an action can be repudiated or not. In other words
can we prove that a users did a thing (e.g. did the FoH engineer hit the mute
button). As we are dealing with a professional environment the target is for
actions to be non-repudiable.

In certain AV environments it is important that actions are not disputable for
regulatory purposes. Radio stations (in most countries) must not have long
silences otherwise they will face fines. In a theatre if a winch is operated
unexpectedly during a performance resulting in an injury it is important to be
able to establish why.

Non-repudability to the user level would require cooperation from the end
devices to provide logs of such actions. On the network we can use normal
network visibility tools such as collecting NetFlow (using NetFlow has short
hand for the type of data not necessarily the protocol from Cisco) data from
switches/routers. Network based collection can indicate as to wether something
odd was happening at the time (e.g. spoofing) although it doesn’t provide
non-repudability by itself.

Network collection in AVoIP networks is uncommon. Full take capture would be
fairly expensive when dealing with high bitrate audio and video, however
collecting NetFlow should be the norm even if it currently isn’t. There are also
some specific AVoIP data sources on the network that would also be valuable to
collect over time such as stream advertisements via SAP or mDNS.

Most smaller utility appliances don’t provide any ability to send logs. More
feature rich device may provide logs. Any logs would be better than no logs even
if it could be time consuming to configure log collection from every capable
endpoint.

## Information disclosure

Information disclosure is allowing access to data to unauthorized parties. In
AVoIP we are principally concerned with the content of our communications the
audio and video, however the metadata may also be interesting.

The media industry tends to concern itself with the protection of copyright they
therefor don’t take kindly to attackers being able to copy their content. Home
users may be familiar with HDCP which is employed to ensure that content can
only be played back on devices that are certified (among other criteria) to
comply with the industries demands.

Another concern is that an attacker may be able to snoop on a hot microphone or
camera feed which could create some reputational problems for everyone involved.

There is a proposal to integrate HDCP with AVoIP however at the time of writing
I’m not aware of any production implementations. HDCP also only addresses
information discloser In the broadest sense that the protected content will only
be reproduced on certified devices, the proposals do not provide any way to
further restrict access (for instance just devices that should be part of this
installation). As it stands you can plug in a device to an AVoIP network join a
multicast group and grab all of the streams.

There might be some benefit from HDCP in that devices will grow the capability
to decrypt/encrypt streams with low latency/jitter which can be repurposed with
a more appropriate key exchange and authorization system.

Currently defence against information disclosure relies on the network being
secure.

## Denial of service

Denial of service is an unavoidable problem especially for AVoIP. Where on
another network you could block multicast to reduce the opportunity for flooding
in AVoIP multicast is core functionality so one has to live with it to some
extent. What can be done is avoiding amplification bugs, segmentation, and
monitoring NetFlow against a baseline.

It is common for AVoIP to have 2 separate networks with each device connected to
once which does protect against accidental denials of service such as someone
building a L2 network loop.

## Elevation of privilledge

Elevation of privilege is interesting as there is no concept of privilege in
AVoIP. However It may be that one engineer may be permitted to change the EQ in
a mixing console another operator is not allowed to perform such an action. If
the operator has access to the network it may not be possible them to change the
EQ in the mixing console, but they can certainly route the stream via their own
device and apply the EQ they want there which is more of an authentication
bypass.

We can gather from this that network access in AVoIP is effectively root in a
*nix system or domain admin in the Windows world.

## DREAD

Now that we have looked at the categories of threats it is time to asses their
impact with some specific attacks. We will dispense with “Discoverability” as it
definitely isn’t very useful in a public blogpost. All scores are 1 (lowest) to
10 (highest). For “Affected users” we consider the audience (4 points),
performers (2 points), technicians (2 points) and copyright holders (2 points). 

| Attack | Damage | Reliability | Exploitability | Affected users | Total score |
|--------|--------|-------------|----------------|----------------|------------|
| Impersonating an audio source stream such as a microphone via mDNS/SAP. | 3 | 10 | 9 | 8 | 20 |
| Impersonating a devices configuration interface. | 1 | 10 | 5 | 2 | 18 |
| Impersonating an audio output stream such as a from a mixing console to speakers via mDNS/SAP. | 7 | 10 | 8 | 8 | 33 |
| Overwriting a stream from a man on the side position using packet timing. | 8 | 7 | 6 | 8 | 29 |
| Tampering with a stream from a man in the middle position. | 8 | 8 | 4 | 8 | 28 |
| Controlling DMX devices such as winches and hoists by sending art-net packets to the right addresses. | 10 | 10 | 10 | 8 | 38 |
| Recording audio/video output streams from the network. | 3 | 10 | 10 | 2 | 25 | 
| Recording audio/video input streams such as from “muted” microphones. | 4 | 10 | 10 | 4 | 18 |
| Flooding multicast groups with traffic to perform denial of service. | 3 | 10 | 10 | 8 | 21 |

## Conclusion

AVoIP has a large attack surface with potential impacts raging from copyright
infringement to physical injury and very few security considerations in the
protocol design. Network level mitigations are available and could be effective
in reducing risk through timely detection and response. However there is a lot
of work to be done to bring a concept of security to the ecosystem. Any progress
must take into account that most installs do not have a security person let
alone a network/security operation centre, so solutions must be accessible to
the technicians already in the building (to the extent that they can at least
identify an issue and call someone).
