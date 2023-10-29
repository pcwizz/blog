+++
title = "Introduction to AV over IP for IT Professionals"
date = 2023-10-29

[taxonomies]
tags = [ "AVoIP", "broadcast", "multicast", "AES67", "SMTPE 2110", "Art-Net", "security", "networking", "IPMX", "ProAV"]
+++

While numerous introductions to AV over IP exist for individuals within the AV industry, there is a scarcity of resources tailored for regular IT professionals. As AV over IP is deployed in more and more environments it comes into contact with the broader IT community. With broader adoption comes a need for IT professionals to understand AV.  In this post I aim to define AV over IP and provide a brief background on this topic.

## What is AV?

AV typically refers to Audio and Video. For the purpose of this discussion, I also include lighting, rigging, and other systems commonly associated with audio and video setups.

## Where is AV Used?

AV technology is pervasive in the modern world, finding application in various settings such as:

- Theatres
- Concerts
- Conferences
- Broadcasters
- Houses of Worship
- Airports
- Train stations

Surprisingly, there is a technological overlap among all these diverse locations.

## How did AV work before IP?

Previously, each element operated on its own independent system, often with its
unique cabling and idiosyncrasies.

Audio was transmitted as analogue signals over XRL, eventually transitioning to
digital through AES3 and various other proprietary vendor-specific protocols,
some of which happened to use Ethernet cables but did not communicate using
Ethernet. Microphones and other audio sources were connected to stage boxes,
which were then linked to the mixing console. The mixing console provided the
control plane for the operator, who then routed the signals to the speakers
through amplifiers.

Video was transmitted over SDI, HDMI, and HDBaseT, with the latter effectively
functioning as HDMI over Ethernet cables but not as Ethernet. Notably, HDMI and
HDBaseT allowed for the tunneling of Ethernet and USB, enabling the possibility
of IP over AV. Specialized video switcher hardware interacted with a video
mixing console.

Lighting, rigging, and related systems used DMX512, which could be
daisy-chained from a lighting desk or, at times, a laptop to various nodes
(such as lights, fog machines, winches, etc.). Each daisy chain represented a
universe with 512 channels, quickly consumed by separate color channels like
red, green, blue, pan, and tilt. Consequently, typical lighting desks supported
multiple universes. Although DMX had its dedicated cables, there was also a
standard for utilizing Ethernet cables.

## Why Use IP for AV?

Dealing with different types of cables posed significant challenges, leading
even non-IP-based AV technologies to incorporate alternative wiring, such as
cat5e. The ability to use cat5e also meant leveraging existing venue cabling or
deploying future-proof cabling, even if the specific usage was unclear at the
time.

Moreover, the issue of distance was resolved in non-IP AV through media
converters, facilitating the transition from, for instance, SDI to fiber.

An inherent pain point uniquely addressed by IP for AV (apart from passive
optical solutions) was the multiplexing of multiple elements over the same
infrastructure. With IP, audio, video, and lighting signals could all be
transmitted over a single cable, eliminating the need for additional wiring.

One of the standout features of IP for AV was its extensibility and
flexibility. The network topology could be adapted to suit the venue and the
specific requirements of the event. Granting access to audio streams for
various engineers, such as monitoring, front of house, and broadcast engineers,
was as straightforward as providing them with subscription access to the
streams on the network.

IP also introduced the ability to route data. While some Ethernet (L2) based AV
solutions could be switched, the option for routing was especially valuable for
larger deployments.

## General Approach of AV over IP (AVoIP)

Open vendor interoperable standards for audio, video, and lighting over IP
extensively utilized multicast. Multicast not only reduced bandwidth usage on
links, thus cutting costs, but also decentralized the responsibility of
accessing multimedia data across the network. This approach transformed the IP
network into an AV routing matrix, a function previously relegated to separate
devices.

By leveraging the power of multicast, AVoIP solutions enabled efficient
distribution of multimedia data across the network, facilitating seamless
communication and collaboration between various AV devices. The shift towards
using the IP network as an AV routing matrix marked a significant evolution in
the field, simplifying the overall AV infrastructure and operation. The
evolution of AV technology represents a departure from the traditional IT
comfort zone when it comes to network design and usage.

Many IT professionals instinctively lean towards minimizing multicast traffic.
Over decades, conventional IT practices have favoured centralized unicast
solutions due to their ease of management, debugging, and security. In a
typical network environment, anything that relies on multicast is often
perceived as an inconvenience, leading administrators to block anything beyond
the essentials, such as DHCP, in order to avoid potential complications.
Dealing with misbehaving devices flooding the network with broadcast or 'local
multicast' is far from ideal. As a result, it's common practice to block
consumer-focused technologies like mDNS within office networks, further
reducing network noise and complexity.

Given this context, it's hardly surprising that there's a substantial lack of
observability, manageability, and security when it comes to multicast. For
those who expect encryption and authentication in AV over IP, it's important to
note that these features are still in the discussion stage. Since the AV
industry is one of the rare sectors where multicast serves a legitimate
purpose, it falls upon the industry to drive the maturation of multicast
technology, bridging the gap to unicast protocols in terms of security and
reliability.

## How the AV industry operates?

The disparity in networking capabilities goes beyond the technical realm,
encompassing cultural and knowledge gaps that have significant implications.

In many theatres and live performance spaces, the absence of dedicated IT
personnel is a common reality. In cases where an IT professional is present,
their responsibilities typically revolve around tasks like managing Wi-Fi
networks and printers for the box office.

The individuals tasked with constructing networks in liver performance spaces
are often AV technicians. Their primary objective is to establish a functional
and reliable system that operates seamlessly, with minimal need for further
attention. Consequently, the training AV technicians receive in networking is
often tailored to the specific requirements of creating a minimal functional
setup. For them, the network serves as a tool to facilitate their core
responsibilities, such as managing lighting, sound, or video production.

In the broadcast sector, while there is typically a greater abundance of
resources, the regulatory environment and the intricate corporate structures of
most broadcasting companies necessitate meticulous long-term planning for any
technological changes. The repercussions of downtime, potential fines, license
revocation, and loss of advertising revenue compel broadcasters to adopt a
cautious approach to technology transformation.

To mitigate these risks, engineers within broadcasting organizations undergo
extensive training to master specific technology stacks, often navigating their
entire careers within a single technological framework. While broadcasters do
maintain dedicated IT and security teams, the risk assessment typically leans
heavily toward maintaining the status quo to ensure operational stability.

## Conclusion

The AV industry has discovered innovative ways to utilize IP technology to
fulfill its specific requirements, and this trend is expected to persist and
evolve further. However, there are developmental gaps that we, as IT
professionals, have previously encountered in other contexts, presenting an
opportunity for us to contribute our expertise and guide the AV industry toward
the right path. While addressing these gaps may require a significant time
investment, it is essential that we explore temporary mitigations that do not
disrupt current operations.

Moving forward I’m going to continue sharing my thoughts and trying to get in
contact with the standards setting bodies (e.g. IPMX, SMTPE, AES). If you have
some thoughts or contacts to share  please reach out.
