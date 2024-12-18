# SignalSurge

This is an open source (check licensing [here](https://github.com/rfrht/SignalSurge/blob/main/LICENSE)) Bandpass filter for VHF and UHF bands, with a selectable LNA and a sequencer. Projected using Eagle CAD 9.

Schematic: 

![Schematic SignalSurge](https://github.com/rfrht/SignalSurge/blob/main/others/schematic.png)

## BPF performance
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
