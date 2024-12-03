# SignalSurge

This is an open source (check licensing [here](https://github.com/rfrht/SignalSurge/blob/main/LICENSE)) Bandpass filter for VHF and UHF bands, with a selectable LNA and a sequencer. Projected using Eagle CAD 9.

Schematic: 

![Schematic SignalSurge](https://github.com/rfrht/SignalSurge/blob/main/schematic.png)

## Maturity level
* Reached the preliminary "minimally lovable project".
* I think that with regards to the base components and schematic, this is pretty much it.

### To Do
* Pending sort out the high power side switching. Options: standard relay, RF relay (expensive), PIN diode, RF switch (hard to solder and/or expensive)
* If using a SPDT relay: build the bias network for `TX INH`
* Build the board layout

## CHANGELOG
* Rev. A: Initial release
