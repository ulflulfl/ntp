## Clock Sources

There are several possible clock sources (GPS, radio clock, ...) to get the - hopefully - accurate current time. Every stratum 1 NTP server uses such a (stratum 0) clock source.

Using a local clock source will be helpful if no internet connection is available or a high accuracy is needed that even good internet NTP servers can't provide (usually below 10 ms).

### Overview
Here is an overview of the clock sources:

| Source | Typical Accuracy Range | Remarks |
| --- | --- | --- |
| GPS | 1 - 50 ms |
| GPS+PPS | 15 ns - 1 ms | PPS: Pulse per second signal
| Radio clock | 5 - 25 ms | e.g. DCF77 |
| Atomic clock | ? | Probably too expensive for common use |
| Telephone modem | ? | Limited practical use today |

**Usually GPS (or better GPS+PPS) is used as the clock source for stratum 1 NTP servers today.**

### GPS

GPS can provide a pretty accurate time signal.

The serial data stream (RS232 or USB) provided by common GPS receivers includes the *GPS time* (https://en.wikipedia.org/wiki/Global_Positioning_System#Timekeeping). While that time is pretty accurate in general, the serial data transfer limits the accuracy to a typical 1 ms (or worse) due to latency, jitter and data processing time.

Modern GPS receiver chipsets may receive signals not only from GPS (USA), but also from similar systems like: Galileo (EU), GLONASS (Russia), BeiDOU (China), ... However, "GPS antennas" may only support a specific frequency range which isn't suitable for all systems. In question consult the data sheets.

Advantages:
- best accuracy 1 ms (up to 50 ms depending on HW/SW used)
- no internet connection needed
- usable worldwide
- medium priced (Aliexpress FC-NTP-Mini: <100 €, TimeMachines TM1000A: 350 €)

Disadvantages:
- clear sky view needed
- indoor use only with external antenna

### GPS+PPS
In addition to the GPS serial data (as mentioned above) providing maybe 1 ms accuracy, specialized GPS hardware can provide a PPS signal (https://en.wikipedia.org/wiki/Pulse-per-second_signal) that provides nanosecond accuracy.

The "trick" is to take the serial GPS data (even with a "poor" accuracy of maybe 50ms) for the coarse date&time and use a highly accurate "PPS one second pulse" with low latency/jitter HW to adjust the actual start of a second with high accuracy.

GPS receiver module data sheets often states a PPS accuracy of 10-20 ns, the GPS-Time itself should have a typical accuracy of 1-10 ns: https://web.archive.org/web/20121028043917/http://tf.nist.gov/time/commonviewgps.htm. Equipment like the MICROCHIP SyncServer S600 mentioned above are probably using such a setup together with dedicated HW to provide accuracy of 15 ns. But even low/medium priced PC HW together with well crafted GPS DIY can provide NTP accuracy < 50us which is far better than using serial GPS data alone.

Advantages:
- typical accuracy maybe 50 us (range highly depends on the HW/SW used: 15 ns - 1 ms)
- no internet connection needed
- usable worldwide
- DIY medium priced (GPS receiver 10-100 €, depending on the HW used)

Disadvantages:
- clear sky view needed
- indoor use only with external antenna
- ready made devices are high priced (MICROCHIP SyncServer S600: 18k €) - are there cheaper alternatives?

### Radio clock (DCF77)
There are many radio clock sources available around the world (see: https://en.wikipedia.org/wiki/Radio_clock and RFC5095 "Figure 12: Reference Identifiers"). Probably the best known are:

- DCF77, 77,5 kHz, Frankfurt (Mainflingen), Germany
- JJY, 40/60kHz, Japan
- WWVB, 60kHz, USA

Advantages:
- no internet connection needed
- indoor use possible (may or may not work)
- cheap DIY (Antenna < 10 € + "some spare parts")

Disadvantages:
- typical accuracy 5-25 ms (DCF77)
- location range limited (DCF77: 2000km around Frankfurt)
- weak reception possible (reinforced concrete, "big motor" interferences, ...)
- ready made devices 1000 € (DCF77: Gude 3011)

### Atomic Clock

To run an own atomic clock is at least costly only for "everyday use". Institutions that run their own atomic clock will probably now much better than I do about strengths and weaknesses ;-)

### Telephone modem

Telephone modems seems to have a relatively poor accuracy and used only on older existing setups.
