# SignalSurge

This is an open source (check licensing [here](https://github.com/rfrht/SignalSurge/blob/main/LICENSE)) Bandpass filter for VHF and UHF bands, with a selectable LNA and a sequencer. Projected using Eagle CAD 9.

## Why?
I live in a metropolitan area and **very** RF-polluted. One of these days, only FFS I have added a FM bandstop filter and I found that, contrary to my belief, my reception was **way better**! That means:

1. The front-end radio filtering is less than great
2. Out of amateur band signals (and there's **plenty** of them) will desensitize your radio
3. By filtering out unwanted signals, you give more **dynamic range** to your radio, improving your reception.
4. [See by yourself here](https://www.reddit.com/r/amateurradio/comments/1gxahma/noticeable_improvement_in_ft991a_2m_rx_when_using/).

That said, the objective of this project is to remove everything outside 2m and 70 cm bands (like: FM broadcasts, air band, commercial VHF & UHF, and everything else between), ensuring that the precious front-end gain by the radio built-in amplifier focus on amateur band signals **only**. 

## Features
* Two selectable sharp amateur band (2m and 70 cm) [bandpass filter](#bpf-theoretical-performance) weeds off signals, ensuring that the transceiver front-end focus its gain figures on inband signals only
* Selectable [low noise 15 dB amplifier](#amplifier-performance) after the bandpass filter, adding some oomph to weak signals
* The amplifier is shut down when the radio is transmitting by powering off its 5V voltage regulator (by cutting the regulator “Enable” line)
* Specialty RF relay allowing 50W in VHF/UHF frequencies to flow to the antenna with low loss, while keeping [unparalleled isolation](https://www.reddit.com/r/rfelectronics/comments/1h5mthn/comment/m0de8n7/) to the downstream components (north of 60 dB)
* Relay default state (NC) is to bypass radio directly to antenna, allowing the board to be safely powered off
* [TX Inhibit](https://iw0ffk.wordpress.com/2018/09/21/tx-inhibit-how-to-simplify-the-tx-rx-sequencing/) functionality to prevent the radio to transmit while the relay isn’t positioned
* Static protection on antenna side
* Small [surge protection](https://www.digikey.com/en/products/detail/eaton-electronics-division/0603ESDA2-TR2/3681416) on antenna and radio sides

## Schematic: 

![Schematic SignalSurge](https://github.com/rfrht/SignalSurge/blob/main/others/schematic.png)

## Preliminary board layout
![Rev A Signal Surge](https://github.com/rfrht/SignalSurge/blob/main/others/SS-thumb.png)

For full board image, [click here](https://github.com/rfrht/SignalSurge/blob/main/others/SS.png)

## BPF theoretical performance
Here's the calculated theoretical filter performance, for VHF and UHF bands:

![VHF BPF performance](https://github.com/rfrht/SignalSurge/blob/main/others/bpf-vhf.png)
![UHF BPF performance](https://github.com/rfrht/SignalSurge/blob/main/others/bpf-uhf.png)

We shall see how that work out in the real world.

## Amplifier performance
Here's the theoretical BFP460 gain figures as per the application notes:

![Amplifier performance](https://github.com/rfrht/SignalSurge/blob/main/others/bfp460-gain-fig.png)

## Maturity level
* Reached the "minimally lovable project" stage.
* I think that with regards to the base components and schematic, this is pretty much it.
* Still untested, unbuilt - and yes, it is still green.

## Library
Custom component library [here](https://github.com/rfrht/FT991A-PAT/blob/master/Schematic/aarf.lbr)

## To Do
* Finish the board layout (WIP)

## CHANGELOG
* Rev. A: Initial release (in the making)

## ACKNOWLEDGEMENTS
I'd like to thank the following contributors:
* The fine [r/rfelectronics](https://www.reddit.com/r/rfelectronics/) community for helping me [square out a RX mute/isolation](https://www.reddit.com/r/rfelectronics/comments/1h0pffq/help_me_reuse_this_circuit/) (tip: it was right under my nose) issue and [how to select the large signal switch](https://www.reddit.com/r/rfelectronics/comments/1h5mthn/help_select_high_power_50w_uhf_switch/)
* The [r/AskElectronics](https://www.reddit.com/r/AskElectronics/) community for [the sanity checking on using a PNP transistor to switch a ground signal](https://www.reddit.com/r/AskElectronics/comments/1hanp3u/will_this_switch_when_tx_gnd_is_grounded/)
* The mighty and fine [**Gordon Hudson** (AD5GG)](https://www.qrz.com/db/AD5GG) for: guiding me on component layouts, deciding for a proper impedance control, his primer on CPWs (coplanar waveguides) and assorted awesomeness.
