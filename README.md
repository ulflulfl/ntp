# ntp

![md-linkcheck](https://github.com/ulflulfl/ntp/actions/workflows/md-linkcheck.yaml/badge.svg)

Infos and experiments around the **N**etwork **T**ime **P**rotocol.

* [accuracy](accuracy/Readme.md) infos and typical data about NTP time accuracy
  * [clock-sources.md](./accuracy/clock-sources.md) use GPS (or alike) to get the current time locally
* [chrony](chrony/Readme.md) some experiments and explanations
  * [chronyc.md](chrony/chronyc.md) the chrony client cli program
  * [chrony-chain.md](chrony/chrony-chain.md) experiment: create a chain of chrony containers and look at the output
* [packets](packets/Readme.md) NTPv4 packet content explained
