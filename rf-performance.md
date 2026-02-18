# RF Performance
The VHF filter is a 3-pole crafted by Gemini. The UHF design employs a Bessel-type bandpass filter, direct-coupled, Series Inductor. While Bessel filters are known for its slow-fall filter skirts, it preserves the signal phase and integrity.

## Revision C
### VHF Filter
Originally, this is the simulated filter performance, modeled by LTSpice:

![OOB VHF BPF](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-gemini-2025-12-29-10-300-ltspice.png)

This is how the filter fared in the real world with a 10 MHz - 300 MHZ VNA sweep of the VHF BPF. It provides a very decent isolation for the lower bands, FM broadcast and other general out-of-band signals.

![OOB VHF BPF](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-gemini-2025-12-25-10-300.png)

This is a sweep of the 2m amateur band - I'm very satisfied with the result, with a low insertion loss (~1.7 dB'ish) and how the filter peaked around 145 MHz:

![2m VHF BPF performance](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-gemini-2025-12-25-144-148.png)

And this is a sweep in the broadcast band, which is my key pain- max -46 dB, min -29 dB of attenuation:

![FM Broadcast filter performance](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-gemini-2025-12-25-76-108.png)

### UHF filter
To be tested.

### Amplifier performance
To be tested.

## Revision A
### VHF filter
This is how the filter performed out-of-box:
![OOB BPF](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-oob-2025-03-13-120-150.png)

After further tweaking, the filter top gain was brought to the 2m band:
![tuned-perf](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-tuned-2025-03-16-60-200.png)

Zooming in on 2m band:
![2m band](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-tuned-2025-03-16-144-148.png)

This is how the filter fared on FM broadcast band:
![fm bc performance](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-tuned-2025-03-16-80-120.png)

And this is a sweep of RTL-SDR FM bandstop filter:
![RTL-SDR FM VNA sweep](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-rtlsdr-2025-03-16-80-120.png)

Another sweep, putting the FM broadcast and 2m band in perspective:
![VNA sweep with broadcast](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-amplified-2025-03-17-60-150.png)

This is a wide sweep (1 MHz to 1 GHz). Notice another peak on 54 MHz. If I wanted to design a filter for both bands I'm positive I would't succeed :-P
![filter performance, wide sweep](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-tuned-2025-03-16-1-1000.png)

Zooming in on 54 MHz peak (notice that this is not intended):
![6m band performance](https://github.com/rfrht/SignalSurge/blob/main/others/test/bpf-vhf-tuned-2025-03-16-40-60.png)

## Amplifier performance
After following the Application Note, I wasn't able to get that linear sweep - instead it was **very** bumpy:
![Amplifier performance](https://github.com/rfrht/SignalSurge/blob/main/others/test/amp-broadband-performance-2025-03-17.png)

More to come.
