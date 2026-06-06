# Device under Test
![OOB VHF BPF](https://github.com/rfrht/SignalSurge/blob/main/others/dut.jpg)

# Power Consumption
The board consumes around 1.3 mA in TX mode, no loaded relays. In RX mode (relays on) and activating the amplifier, the power consumption jumps to around 40 mA. RX mode, no relays and no amp, the board consumes around 3 mA. The board has a PTC fuse set at 50 mA.

# RF Performance
The VHF filter is a 3-pole crafted by Gemini. The UHF design employs a Bessel-type bandpass filter, direct-coupled, Series Inductor.

## VHF Filter (Rev. E1)
Originally, this is the simulated filter performance, modeled by LTSpice:

![Theoretical VHF filter](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-gemini-2025-12-29-10-300-ltspice.png)

This is how the filter fared in the real world with a 10 MHz - 300 MHZ VNA sweep of the VHF BPF. It provides a very decent isolation for the lower bands, FM broadcast and other general out-of-band signals.

![Out of box VHF BPF](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-noamp-10-300.png)

This is the VSWR sweep of the 2m amateur band:

![2m VHF BPF SWR performance](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-vswr-140-150.png)

And this is a insertion loss sweep in the broadcast band, which is my key pain - Almost the entire band under -60 dB:

![FM Broadcast filter performance](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-noamp-66-108.png)

## UHF filter (Rev. E1)
Here is the theoretical projected filter performance:

![Theoretical UHF BPF](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-uhf.png)

And here is what the UHF filter delivered on a sweep from 10-600 MHz:

![UHF filter wide sweep](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-uhf-noamp-10-600.png)

And this is how it is performing on 70 cm band (insertion loss and VSWR):

![70 cm band S11 and S21](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-uhf-noamp-420-440.png)


## Isolation performance
When the board is on TX or bypass mode, the amplifier is disabled, and there's a front-end RF switch right after the relay, adding extra signal isolation during transmission and high power voltages. This is the isolation figure during TX/Bypass mode. 10-500 MHz sweep.

![Isolation performance from RF switches](https://github.com/rfrht/SignalSurge/blob/main/others/test/isolation-2026-02-20-10-500.png)


## Amplifier performance
Here's the theoretical [BFP460](https://www.infineon.com/cms/en/product/rf/rf-transistor/low-noise-rf-transistors/bfp460/) gain figures as per the application notes:

![Amplifier performance](https://github.com/rfrht/SignalSurge/blob/main/others/test/bfp460-gain-fig.png)

The filter delivered approx. 16 dB of gain, nicely, neatly and linearly. Check the results for VHF and UHF amplification:

### VHF
![Amplifier performance - VHF](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-amp-noamp-2026-02-22-10-300.png)

### UHF
![Amplifier performance - VHF](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-uhf-amp-noamp-2026-02-22-300-600.png)
