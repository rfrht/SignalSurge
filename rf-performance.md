# RF Performance

## VHF Filter (Rev. E1)
The VHF filter is a 3-Pole Top-Coupled Chebyshev Bandpass Filter. This is the first stab, simulated filter performance, modeled by LTSpice:

![Theoretical VHF filter](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-gemini-2025-12-29-10-300-ltspice.png)

This is how the filter on 10 MHz - 300 MHZ VNA sweep of the VHF BPF - this was tapped right at the BPF stage, without any extra other insertion loss (switches, amps, etc). It provided a very decent isolation for the lower bands, FM broadcast and other general out-of-band signals.

![Out of box VHF BPF](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-noamp-10-300.png)

This is the VSWR and insertion loss sweep of the 2m amateur band - tested with a fully assembled board, SMA-to-SMA:

![2m VHF BPF SWR performance](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-vswr-140-150.png)

And this is a insertion loss sweep in the broadcast band, which is my key pain - Almost the entire band under -60 dB:

![FM Broadcast filter performance](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-noamp-66-108.png)

## UHF filter (Rev. E1)
The UHF filter is a 2-Pole Capacitively Coupled Parallel LC Resonator. Here is the theoretical projected filter performance:

![Theoretical UHF BPF](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-uhf.png)

And here is what the UHF filter delivered on a sweep from 10-600 MHz (testing it right at the BPF, without any extra insertion loss):

![UHF filter wide sweep](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-uhf-noamp-10-600.png)

And this is how it is performing on 70 cm band, measuring insertion loss and VSWR in a fully assembled board, at the SMA connectors:

![70 cm band S11 and S21](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-uhf-noamp-420-440.png)


## Amplifier performance
Here's the [BFP460](https://www.infineon.com/cms/en/product/rf/rf-transistor/low-noise-rf-transistors/bfp460/) gain figures, measuring solely the amplifier stage, without any extra insertion loss:

![Amplifier performance](https://github.com/rfrht/SignalSurge/blob/main/others/test/amp-broadband-performance.png)

The filter delivered approx. 10 dB of gain. 

Individual fully assembled board results, with signal travesrsing the BPF, connectors, switchetc etc, for VHF and UHF bands:

### VHF
![Amplifier performance - VHF](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-1amp-100-200.png)

### UHF
![Amplifier performance - VHF](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-uhf-1amp-300-600.png)
