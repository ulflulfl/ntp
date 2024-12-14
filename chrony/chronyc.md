# NTP chronyc

The output of chronyc (the client command of chrony) can be a bit cryptic at first sight.

Full details about chronyc can be found at the man pages, e.g.: https://manpages.ubuntu.com/manpages/noble/man1/chronyc.1.html

## chronyc sources

The `chronyc sources` command shows status details of the configured time sources.

To get the output with an update every 2s (watch) and a nice explanatory caption (-v) use:

> watch chronyc sources -v

If chrony runs in a container:

> sudo watch docker exec *containername* chronyc sources -v

Example output:

```
  .-- Source mode  '^' = server, '=' = peer, '#' = local clock.
 / .- Source state '*' = current best, '+' = combined, '-' = not combined,
| /             'x' = may be in error, '~' = too variable, '?' = unusable.
||                                                 .- xxxx [ yyyy ] +/- zzzz
||      Reachability register (octal) -.           |  xxxx = adjusted offset,
||      Log2(Polling interval) --.      |          |  yyyy = measured offset,
||                                \     |          |  zzzz = estimated error.
||                                 |    |           \
MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================================
#- GPS                           0   4   377    19   -100us[  -99us] +/-  100ms
#* PPS                           0   4   377    19   +269ns[ +996ns] +/-  492ns
^- time.cloudflare.com           3   6   377     8    -27ms[  -27ms] +/-   10ms
^- time1.google.com              1   6   377     9    -28ms[  -28ms] +/-   13ms
^- time2.google.com              1   6   377    10    -28ms[  -28ms] +/-   13ms
^? 120.25.115.20                 2   7   200   465   -374ms[ -313ms] +/-  177ms
^- 203.107.6.88                  2   6   377    10  +5088us[+5088us] +/-  202ms
^- ntp-server1.chrony_defau>     4   6   377     7    +47ms[  +47ms] +/-   74ms
^- ntp-server2.chrony_defau>     4   6   377     6  +5404us[+5404us] +/-   62ms
^? ntp-server3.chrony_defau>     0  10     0     -     +0ns[   +0ns] +/-    0ns
```

The first two sources are locally attached GPS and PPS (pulse per second) sources, all other are internet NTP servers.

From left to right:

- **M** (Source Mode):
  - **^** "parent" NTP server
  - **=** peer NTP server
  - **#** locally connected reference clock (GPS, PPS, DCF77, ...) -> always stratum 0
- **S** (Source State):
  - **\*** current best: Currently used time source for sync
  - **+** combined: Adds to the current best source
  - **-** not combined: acceptable time source currently not used for sync, but still polled
  - **X** may be in error: e.g. inconsistent with other sources
  - **~** too variable: Time variability too high
  - **?** unusable: e.g. if server is (temporarily) unreachable or before initial 3 packets were received
  - hint: with the typical poll rate of 6, a configured NTP source will initially stay at least 3*2^6=192s (~3 mins.) in the state ? before becoming "active". Use iburst for initial speed up.
- **Name/IP address** as you may guess ...
- **Stratum** how many NTP server "hops" this server is away from a reference time source (GPS, DCF77), will be 0 if its actually such a time source or initially for a never connected NTP server. Raised by 1 for each NTP server away from the time source. (TODO: For details on stratum/strata, see [ntp_stratum.md](ntp_stratum.md))
- **Poll** how often the source is requested, in 2^x seconds. The minimum for NTP servers is typically 6 -> 2^6=64s. Will raise automatically if a server is not available.
- **Reach** Reachability of a server. This is an octal (base 8) number: 0 is unreachable, 377 is best. Each poll interval, this number is shifted one bit to the left and the least significant bit is filled with 1 if the server could be reached and 0 if not. To "decode" it, convert the number to binary and look at the bits.
- **LastRx** time since last received packet from NTP server in seconds. If a packet was received from that server, it goes back to 0.
- **Last sample**
  - **adjusted offset** TODO: clarify
  - **measured offset** TODO: clarify
  - **estimated error** TODO: clarify
  - positive values indicate the local clock is faster than the source

## chronyc tracking

TODO: add details

## chronyc sourcestats

TODO: add details
