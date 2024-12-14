# NTP network packets

How NTP appears on the network ...

## TCP / UDP Ports

NTP:

* NTP uses port **udp/123** for the time sync

NTS (network time security):

* NTS uses port **tcp/4460** for initial crypto negotiation
* NTS uses port **udp/123** for the time sync (just like NTP)

In the beginnings of NTS (around year 2020), tcp/123 was used by some servers, but that should hopefully be corrected by now.

## NTP variations

**Typical NTP** traffic consists of a single NTP client request UDP packet which is responded by the NTP server with a single response UDP packet. This sequence is repeated by the client with the current poll interval (see below).

I haven't tried **special NTP modes** like symmetric, broadcast or control messages.

If **network time security (NTS)** is used, initial "crypto negotiation" is done using TLS (much like HTTPS) in a few packets over TCP. The actual time sync uses "normal" NTP packets, appended by extension fields containing crypto infos (e.g. so called "cookies").

## Poll Interval and Initial Burst "iburst"

The NTP request is repeated by the client in the current poll interval, which has a typical default of 2^6=64 seconds. That interval is dynamically adjusted by the client, e.g. if a server is currently not reachable, the poll interval will be raised to avoid unnecessary network traffic.

At startup, the client requires at least two (TODO: Or was it three?) server responses to get a reliable initial time. Together with the default interval of 64 seconds this takes more than two minutes. If "iburst" (initial burst) is used by the client, it will send a few requests in a short time to speed up this initial time sync.

## Packet details

Details of NTP network packet content.

![ntpv4_packets.drawio.svg](ntpv4_packets.drawio.svg)
