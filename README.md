# SignalSurge
This is an open source (check licensing [here](https://github.com/rfrht/SignalSurge/blob/main/LICENSE)) Bandpass filter for VHF and UHF amateur radio bands, with a selectable LNA and a sequencer. Projected using Eagle CAD 9.

# Why?
I live in a metropolitan area and **very** RF-polluted. One of these days, only FFS I have added a FM bandstop filter and I found that, contrary to my belief, my reception was **way better**! That means:

1. The front-end radio filtering is less than great (even being an expensive FT-991A)
2. Strong out of amateur band signals (and there's **plenty** of them) will desensitize your radio
3. By filtering out unwanted signals, you give more **dynamic range** to your radio, improving your reception.
4. [See for yourself here](https://www.reddit.com/r/amateurradio/comments/1gxahma/noticeable_improvement_in_ft991a_2m_rx_when_using/).

That said, the objective of this project is to remove everything outside 2m and 70 cm bands (like: FM broadcasts, air band, commercial VHF & UHF, and everything else between), ensuring that the precious front-end gain on the radio built-in amplifier focus on amateur band signals **only**.

I could buy a [300 EUR filter](https://antennas-amplifiers.com/double-2x200w-bandpass-filter-144-148mhz-430-450mhz/), but... man, that's expensive. So why not expend EUR3,000 man hour-worth of my time to build one?

# Features
* Two selectable amateur band (2m and 70 cm) [bandpass filter](#bpf-theoretical-performance) weeds off signals, ensuring that the transceiver front-end focus its gain figures on inband signals only
* High performance and selectable [low noise 15 dB amplifier](#amplifier-performance) after the bandpass filter, adding some oomph to weak signals
* The amplifier is shut down when the radio is transmitting by powering off its 5V rail (by cutting the regulator "Enable" line)
* Specialty RF relay allowing 50W in VHF/UHF frequencies to flow to the antenna with low loss, while keeping [unparalleled isolation](https://www.reddit.com/r/rfelectronics/comments/1h5mthn/comment/m0de8n7/) to the downstream components (north of 60 dB)
* Relay default state (NC) is to bypass radio directly to antenna, allowing the board to be safely powered off
* [TX Inhibit](https://iw0ffk.wordpress.com/2018/09/21/tx-inhibit-how-to-simplify-the-tx-rx-sequencing/) functionality to prevent the radio to transmit while the relay isn't positioned
* Static bleeding on antenna side when switched to BPF/AMP
* Small [surge protection](https://www.digikey.com/en/products/detail/eaton-electronics-division/0603ESDA2-TR2/3681416) on antenna and radio sides
* [Co-Planar Waveguide (CPW)](https://resources.altium.com/p/pros-and-cons-of-different-high-frequency-transmission-line-types) design on RF lines for impedance control and good RF performance

## Library
Eagle custom component library [here](https://github.com/rfrht/FT991A-PAT/blob/master/Schematic/aarf.lbr)

Bill of Materials (CSV format, DigiKey format) [here](https://github.com/rfrht/SignalSurge/blob/main/others/SS.csv). Notice that there's a full RF Inductor and Capacitor kit specified in the BOM - and that is **costly**. If you have the suitable RF L/C components, fine! Otherwise (like me), buy a proper one.

# Schematic: 
![Schematic SignalSurge](https://github.com/rfrht/SignalSurge/blob/main/others/schematic.png)

# Board layout
![Rev A Signal Surge](https://github.com/rfrht/SignalSurge/blob/main/others/ss-board.jpg)

For full board image, [click here](https://github.com/rfrht/SignalSurge/blob/main/others/SS.png)

# BPF performance
This is how the filter performed out-of-box, with theoretically calculated components.

* 2m band:
![VHF BPF performance](https://github.com/rfrht/SignalSurge/blob/main/others/vna-sweep-vhf.jpg)

* 70cm band:
![UHF BPF performance](https://github.com/rfrht/SignalSurge/blob/main/others/vna-sweep-uhf.jpg)

## BPF theoretical performance
Here's the calculated theoretical filter performance, for VHF and UHF bands:

![VHF BPF performance](https://github.com/rfrht/SignalSurge/blob/main/others/bpf-vhf.png)
![UHF BPF performance](https://github.com/rfrht/SignalSurge/blob/main/others/bpf-uhf.png)

## Performance notes
Notice that the theoretical performance involved ideal components, no parasitics, infinite Q, etc components. The filter will be refactored with [lower value components](https://github.com/rfrht/SignalSurge/blob/main/others/cap-kit.jpg) to bring the top of the curve to the target area.

# Amplifier performance
Here's the theoretical [BFP460](https://www.infineon.com/cms/en/product/rf/rf-transistor/low-noise-rf-transistors/bfp460/) gain figures as per the application notes:

![Amplifier performance](https://github.com/rfrht/SignalSurge/blob/main/others/bfp460-gain-fig.png)

# Maturity level / Status
* Mar 11 - The boards arrived and assembled the Rev. A board - the BPF works! A bit out of frequency, but it works. More testing and component tweaking in the make.
* Feb 19 - All parts inventoried and accounted for. Placed the board order
* Feb 07 - Bought the needed parts!
* Jan/2025 - Reached the "minimally lovable project" stage.

# To Do
* Bring the filter fC to the center of amateur radio bands
* Add a u.fl port on BPF in and out lines

# Relay Logic
## BPF and Relay control
### Truth table (NOR)
| RADIO_TX | FORCE_BYPASS | Signal Level | Behavior |
:---|---|---|---:
| 0 | 0 | 1 | VIA BPF |
| 0 | 1 | 0 | BYPASS |
| 1 | 0 | 0 | BYPASS |
| 1 | 1 | 0 | BYPASS |

### RX/Via BPF states:
| Circuit | Connected to  | Power |
:---------|---------------|--------:
| Relays  | NO | + |
| Isolation | NO | + |

### TX/Bypass states:
| Circuit | Connected to  | Power |
:---------|---------------|--------:
| Relays  | NC | - |
| Isolation | NC | - |

## LNA 
### Truth Table (AND)
| BPF_ON | LNA_ON | Signal Level | Behavior |
:---|---|---|---:
| 0 | 0 | 0 | OFF |
| 0 | 1 | 0 | OFF |
| 1 | 0 | 0 | OFF |
| 1 | 1 | 1 | ON |

### LNA On
| Circuit | Connected to  | Power |
:---------|---------------|--------:
| V.REG.  | EN | + |
| LNA SW  | NO | + |


### LNA Off
| Circuit | Connected to  | Power |
:---------|---------------|--------:
| V.REG.  | EN | - |
| LNA SW  | NC | - |

## CHANGELOG
* Rev. A: Initial release (Testing it)

## ACKNOWLEDGEMENTS
I'd like to thank the following contributors:
* The fine [r/rfelectronics](https://www.reddit.com/r/rfelectronics/) community for helping me [square out a RX mute/isolation](https://www.reddit.com/r/rfelectronics/comments/1h0pffq/help_me_reuse_this_circuit/) (tip: it was right under my nose) issue and [how to select the large signal switch](https://www.reddit.com/r/rfelectronics/comments/1h5mthn/help_select_high_power_50w_uhf_switch/) and [a critical review of the bandpass filters](https://www.reddit.com/r/rfelectronics/comments/1hk1mgi/sanity_check_in_vhf_uhf_bpf_filters/)
* The [r/AskElectronics](https://www.reddit.com/r/AskElectronics/) community for [the sanity checking on using a PNP transistor to switch a ground signal](https://www.reddit.com/r/AskElectronics/comments/1hanp3u/will_this_switch_when_tx_gnd_is_grounded/).
* The mighty and fine [**Gordon Hudson** (AD5GG)](https://www.qrz.com/db/AD5GG) for: guiding me on component layouts, deciding for a proper impedance control, his primer on CPWs (coplanar waveguides) and assorted awesomeness.
