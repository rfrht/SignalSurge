# SignalSurge
This is an open source (check licensing [here](https://github.com/rfrht/SignalSurge/blob/main/LICENSE)) Bandpass filter for VHF and UHF amateur radio bands, with a selectable LNA and a sequencer. Projected using Eagle CAD 9.

# Why?
I live in a metropolitan area and **very** RF-polluted. One of these days, only FFS I have added a FM bandstop filter and I found that, contrary to my belief, my reception was **way better**! That means:

1. The front-end radio filtering is less than great (even being an expensive FT-991A)
2. Strong out of amateur band signals (and there's **plenty** of them) will desensitize your radio
3. By filtering out unwanted signals, you give more **dynamic range** to your radio, improving your reception.
4. [See for yourself here](https://www.reddit.com/r/amateurradio/comments/1gxahma/noticeable_improvement_in_ft991a_2m_rx_when_using/).

That said, the objective of this project is to remove as much as possible signals outside 2m and 70 cm bands (like: FM broadcasts, air band, commercial VHF & UHF, and everything else inbetween), ensuring that the precious front-end gain on the radio built-in amplifier focus on amateur band signals **only**.

I could buy a [300 EUR filter](https://antennas-amplifiers.com/double-2x200w-bandpass-filter-144-148mhz-430-450mhz/), but... man, that's expensive. So why not expend EUR3,000 man hour-worth of my time and parts to build one?

# Features
* Two selectable amateur band (2m and 70 cm) [bandpass filter](#bpf-theoretical-performance) weeds off signals, ensuring that the transceiver front-end focus its gain figures on inband signals only
* High performance and selectable [low noise 15 dB amplifier](https://github.com/rfrht/SignalSurge/blob/main/rf-performance.md#amplifier-performance) after the bandpass filter, adding some oomph to weak signals
* The amplifier is shut down when the radio is transmitting by powering off its 5V rail (by cutting the regulator "Enable" line) to prevent any artifacts/ringing and damage. Also, contains a [RF switch completely isolating](https://github.com/rfrht/SignalSurge/blob/main/rf-performance.md#isolation-performance) the low signal part of the board during the TX for added protection.
* Specialty RF relay allowing 50W in VHF/UHF frequencies to flow to the antenna with low loss, while keeping [unparalleled isolation](https://www.reddit.com/r/rfelectronics/comments/1h5mthn/comment/m0de8n7/) to the downstream components (north of 60 dB)
* Relay default state (NC) is to bypass radio directly to antenna, allowing the board to be safely powered off
* [TX Inhibit](https://iw0ffk.wordpress.com/2018/09/21/tx-inhibit-how-to-simplify-the-tx-rx-sequencing/) functionality to prevent the radio to transmit while the relay isn't positioned
* Static bleeding and small [surge protection](https://www.digikey.com/en/products/detail/eaton-electronics-division/0603ESDA2-TR2/3681416) when switched to BPF/AMP
* [Co-Planar Waveguide (CPW)](https://resources.altium.com/p/pros-and-cons-of-different-high-frequency-transmission-line-types) design on RF lines for impedance control and good RF performance
* A [detachable miniature](https://github.com/rfrht/SignalSurge/blob/main/others/miniature-filter-board.jpg) VHF BPF filter that [fits perfectly](https://github.com/rfrht/SignalSurge/blob/main/others/miniature-filter.jpg) in [RTL-SDR](https://www.rtl-sdr.com/rtl-sdr-com-broadcast-fm-band-stop-filter-88-108-mhz-reject-now-for-sale/) [FM bandstop cases](https://github.com/rfrht/SignalSurge/blob/main/others/encased-miniature-filter.jpg). **NOTE:** This filter DOES NOT withstand TX power; it will **fry** the inductor! Do not transmit on it.

## Library
Eagle custom component library [here](https://github.com/rfrht/FT991A-PAT/blob/master/Schematic/aarf.lbr). Ensure to **also** fetch the latest library version (and update in your Eagle) when checking out new code.

Bill of Materials (CSV format, DigiKey format) [here](https://github.com/rfrht/SignalSurge/blob/main/others/SS.csv). Notice that there's a full RF Inductor and Capacitor kit specified in the BOM - and that is **costly**. If you have the suitable RF L/C components, fine! Otherwise (like me), buy a proper one.

# Schematic: 
![Schematic SignalSurge](https://github.com/rfrht/SignalSurge/blob/main/others/schematic.png)

# Board layout
[Rev. D board layout](https://github.com/rfrht/SignalSurge/blob/main/others/SS.png)

Rev. B finished board:

![Rev B Signal Surge finished board](https://github.com/rfrht/SignalSurge/blob/main/others/ss-board.jpg)

### Why the large exposed pads around the BPF/LNA?
The objective is to test an isolation layer, by covering the components with Kapton tape and covering it with copper tape, to mitigate external interferences.

# BPF performance
Check the [test results](https://github.com/rfrht/SignalSurge/blob/main/rf-performance.md) page.

# Amplifier performance
Check the [test results](https://github.com/rfrht/SignalSurge/blob/main/rf-performance.md#amplifier-performance) page.

# Relay Logic
Check the [relay logic](https://github.com/rfrht/SignalSurge/blob/main/others/relay-logic.md) page.

# KNOWN ISSUES
**Ensure** to check the [Issues backlog](https://github.com/rfrht/SignalSurge/issues)

# JOURNEY
* Feb 22 - Pushing Rev. D
* Feb 20 - Finished BPF and LNA tests
* Feb/2026 - Got the Rev. C boards
* Dec 30 - Refactored the coplanar waveguides (CPW)
* Dec 29 - Changed layout of BPF section, separating inductors at 90° - Gemini said it's a good practice
* Dec 28 - Reviewed the board layout, added descriptions to signal lines, refactored the detachable VHF BPF, fixed a pernicious board cut failure to render (it was my fault)
* Dec 25 - [Used Gemini to design a VHF BPF](https://github.com/rfrht/SignalSurge/blob/main/others/gemini-prompt.md) - which yielded [pretty good results](https://github.com/rfrht/SignalSurge/blob/main/rf-performance.md), *tips hat to AI*
* Dec 25 - Starting Rev. C. 
* Aug 31 - Tested the VHF BPF and it was way out of line. Shelved the project for a while.
* Aug 31 - Added pulldown lines in comparators, routing change on the back of the board, unified logic ICs into SC70-5 footprint, updated BOM to add new pulldown resistors, better footprint for BFP460 and NCP553, fixed wrong 0.22µf part (it was 0.022µf) in BOM
* Jul 09 - Added a detachable board (taking advantage of the spare board real estate) that fits a RTL-SDR Bandpass filter. Minor signal and label revisions, routing, better labeling for signal I/O by adding expected voltages, improved semiotics. Tentatively final.
* May 18 - Fixed the Rev. A defects, check the [changelog here](https://github.com/rfrht/SignalSurge/commit/eed25a111f6a104a3589aa6e2ec3a4192e9d5c3d)
* Mar 17 - Tested relay logic (it is sound!), voltage regulators. And also fried the first board by shorting the 13.8V line on the 3V line **before** the fuse.
* Mar 16 - Tweaked the filter to bring the peak 2m BPF performance to the center of the band (146 MHz). Done in trial and error.
* Mar 11 - The boards arrived and assembled the Rev. A board - the BPF works! A bit out of frequency, but it works. More testing and component tweaking in the make.
* Feb 19 - All parts inventoried and accounted for. Placed the board order
* Feb 07 - Bought the needed parts!
* Jan/2025 - Reached the "minimally lovable project" stage.

# CHANGELOG
* Rev. D: Changed 74XX pulldown lines to 100k, fixed UMD5N pins, small tweak in UHF filter (caps changed to 20 pF)
* Rev. C: New VHF BPF topology, added a test UHF BPF on the back of the board, improved the LNA board layout, fixed capacitor pads (was too small)
* Rev. B: Added test points, changed connectors to SMA (big signal) and U.FL (small signal and test)
* Rev. A: Initial release

# ACKNOWLEDGEMENTS
I'd like to thank the following contributors:
* The fine [r/rfelectronics](https://www.reddit.com/r/rfelectronics/) community for helping me [square out a RX mute/isolation](https://www.reddit.com/r/rfelectronics/comments/1h0pffq/help_me_reuse_this_circuit/) (tip: it was right under my nose) issue and [how to select the large signal switch](https://www.reddit.com/r/rfelectronics/comments/1h5mthn/help_select_high_power_50w_uhf_switch/) and [a critical review of the bandpass filters](https://www.reddit.com/r/rfelectronics/comments/1hk1mgi/sanity_check_in_vhf_uhf_bpf_filters/)
* The [r/AskElectronics](https://www.reddit.com/r/AskElectronics/) community for [the sanity checking on using a PNP transistor to switch a ground signal](https://www.reddit.com/r/AskElectronics/comments/1hanp3u/will_this_switch_when_tx_gnd_is_grounded/).
* The mighty and fine [**Gordon Hudson** (AD5GG)](https://www.qrz.com/db/AD5GG) for: guiding me on component layouts, deciding for a proper impedance control, his primer on CPWs (coplanar waveguides) and assorted awesomeness.

