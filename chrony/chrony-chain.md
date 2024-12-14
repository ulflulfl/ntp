# Chrony Chain Experiment

Experiment: Place a lot of chrony containers in a row and look at the chronyc "Stratum" and "Expected Error" values.

## Setup

Create a chain of chrony containers with:

[docker-compose-chrony-chain.yml](docker-compose-chrony-chain.yml)

> docker-compose -f docker-compose-chrony-chain.yml up -d

## After a while ...

> watch sudo docker exec ntp-client chronyc sources -v

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
^- ntp-server1.chrony_defau>     2   6   377    38  -2611us[-6942us] +/-   14ms
^* ntp-server2.chrony_defau>     3   6   377    19  -7320us[  -12ms] +/-   19ms
^+ ntp-server3.chrony_defau>     4   6   377    10  -9257us[-9257us] +/-   20ms
^+ ntp-server4.chrony_defau>     5   6   377     6    -11ms[  -11ms] +/-   19ms
^+ ntp-server5.chrony_defau>     6   6   377     9    -12ms[  -12ms] +/-   19ms
^+ ntp-server6.chrony_defau>     7   6   377    20    -13ms[  -18ms] +/-   21ms
^+ ntp-server7.chrony_defau>     8   6   377    15  -6886us[-6886us] +/-   33ms
^+ ntp-server8.chrony_defau>     9   6   377    20   +542us[-4034us] +/-   36ms
^+ ntp-server9.chrony_defau>    10   6   377    25    +14ms[+9472us] +/-   45ms
^+ ntp-server10.chrony_defa>    11   6   377    12    +20ms[  +20ms] +/-   39ms
^+ ntp-server11.chrony_defa>    12   6   377    21    +25ms[  +21ms] +/-   28ms
^- ntp-server12.chrony_defa>    13   6   377     8    +28ms[  +28ms] +/-   28ms
^x ntp-server13.chrony_defa>    14   6   377    26    +34ms[  +29ms] +/-   26ms
^x ntp-server14.chrony_defa>    15   6   377     8    +36ms[  +36ms] +/-   27ms
^? ntp-server15.chrony_defa>     0  10     0     -     +0ns[   +0ns] +/-    0ns
^? ntp-server16.chrony_defa>     0  10     0     -     +0ns[   +0ns] +/-    0ns
^? ntp-server17.chrony_defa>     0  10     0     -     +0ns[   +0ns] +/-    0ns
```

Learnings:

* Stratum 15 is the highest possible, as 16 is invalid
* In theory, the expected error (last column) should raise with each Stratum. In practice, the highest error as at Stratum 10 and is going down with higher Strata again

## During establishing

The establishing of the chain takes several minutes.

When looking at the `chronyc sources` output during the setup:

```
MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================================
^* ntp-server1.chrony_defau>     2   6   377    50    +60us[  +97us] +/-   15ms
^- ntp-server2.chrony_defau>     3   6   377    26  +7639ns[+7639ns] +/-   15ms
^- ntp-server3.chrony_defau>     4   6   377     8   -349us[ -349us] +/-   16ms
^- ntp-server4.chrony_defau>     5   6   377    11   -415us[ -415us] +/-   16ms
^- ntp-server5.chrony_defau>     6   6   377    19    -38us[  -38us] +/-   16ms
^- ntp-server6.chrony_defau>     7   6   377    24  +2798us[+2798us] +/-   18ms
^- ntp-server7.chrony_defau>     8   6    77    20  +3471us[+3471us] +/-   19ms
^? ntp-server8.chrony_defau>     9   6     3    29  +5113us[+5113us] +/-   29ms
^? ntp-server9.chrony_defau>     0   7     0     -     +0ns[   +0ns] +/-    0ns
^? ntp-server10.chrony_defa>     0   7     0     -     +0ns[   +0ns] +/-    0ns
^? ntp-server11.chrony_defa>     0   7     0     -     +0ns[   +0ns] +/-    0ns
...
```
