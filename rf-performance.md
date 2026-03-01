# Device under Test
![OOB VHF BPF](https://github.com/rfrht/SignalSurge/blob/main/others/dut.jpg)

# Power Consumption
The board consumes around 1.3 mA in TX mode, no loaded relays. In RX mode (relays on) and activating the amplifier, the power consumption jumps to around 40 mA. RX mode, no relays and no amp, the board consumes around 3 mA. The board has a PTC fuse set at 50 mA.

# RF Performance
The VHF filter is a 3-pole crafted by Gemini. The UHF design employs a Bessel-type bandpass filter, direct-coupled, Series Inductor.

## VHF Filter
Originally, this is the simulated filter performance, modeled by LTSpice:

![Theoretical VHF filter](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-gemini-2025-12-29-10-300-ltspice.png)

This is how the filter fared in the real world with a 10 MHz - 300 MHZ VNA sweep of the VHF BPF. It provides a very decent isolation for the lower bands, FM broadcast and other general out-of-band signals. The bold superior line is the actual filter performance with the amplifier on. Lower line, no amp.

![Out of box VHF BPF](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-amp-noamp-2026-02-22-10-300.png)

This is a sweep of the 2m amateur band - I'm very satisfied with the result, with a low insertion loss (~1.7 dB'ish) and how the filter peaked around 145 MHz:

![2m VHF BPF performance](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-2m-2026-02-22-144-148.png)

And this is a sweep in the broadcast band, which is my key pain- max -46 dB, min -30 dB of attenuation:

![FM Broadcast filter performance](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-2m-2026-02-22-76-108.png)

## UHF filter
Here is the Meraki projected filter performance:

![Out of box UHF BPF](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-uhf.png)

And here is what the UHF filter delivered on a sweep from 300-600 MHz:

![Larger UHF sweep](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-uhf-noamp-2026-02-22-300-600.png)

Zooming on UHF range, you can see the highest gain range at the end of the band - which is my region of interest, the repeater output at 439 MHz.

![70 cm band VNA sweep](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-70cm-2026-02-22-430-440.png)

## Gain and VSWR, end-to-end (SMA-to-SMA)
### VSWR figures
The VSWR tests were performed by injecting signal in `ANTENNA` SMA connector and terminating with a 50 ohm load at the `RADIO` SMA port. 

![60-500 MHz VSWR sweep](https://github.com/rfrht/SignalSurge/blob/main/others/test/vswr-60-500.png)

This is a VSWR sweep from 60 to 500 MHz (Revision C).

![2m amateur band VSWR sweep](https://github.com/rfrht/SignalSurge/blob/main/others/test/vswr-144-148.png)

The above image is the VSWR performance in 2m (VHF) amateur band.

![70cm amateur band VSWR sweep](https://github.com/rfrht/SignalSurge/blob/main/others/test/vswr-430-440.png)

The above image is the VSWR performance in 70cm (UHF) amateur band.


### Gain figures
The gain figures were measured by the VNA in S21 mode, injecting signal (Port 1) in `ANTENNA` SMA connector and the receiving (Port 2) in `RADIO` connector.

#### VHF, no amplifier
![VHF gain, end to end, no amplifier](https://github.com/rfrht/SignalSurge/blob/main/others/test/e2e-2m-amp-off.png)

#### VHF, amplified
![VHF gain, end to end, amplifier on](https://github.com/rfrht/SignalSurge/blob/main/others/test/e2e-2m-amp-on.png)

#### UHF, no amplifier
![UHF gain, end to end, no amplifier](https://github.com/rfrht/SignalSurge/blob/main/others/test/e2e-70cm-amp-off.png)

#### UHF, amplified
![UHF gain, end to end, amplifier on](https://github.com/rfrht/SignalSurge/blob/main/others/test/e2e-70cm-amp-on.png)

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
