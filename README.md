# SignalSurge

This is an open source (check licensing [here](https://github.com/rfrht/SignalSurge/blob/main/LICENSE)) Bandpass filter for VHF and UHF bands, with a selectable LNA and a sequencer. Projected using Eagle CAD 9.

Schematic: 

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
