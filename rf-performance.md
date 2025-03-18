# RF Performance
The choosen design employs a Bessel-type bandpass filter, direct-coupled, Series Inductor. While Bessel filters are known for its slow-fall filter skirts, it preserves the signal phase and integrity.

## VHF filter
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
