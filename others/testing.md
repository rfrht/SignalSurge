# Test sequence
![Board picture - test sequence](https://github.com/rfrht/SignalSurge/blob/main/others/test-sequence.jpg)

* Start soldering the highlighted u.FL connectors and the VHF/UHF BPF **discretes (caps & inductors) only**. Do not solder the RF switch yet.
-
* Then, inject signal at u.FL connector `1` (marked `INP`) and measure S21 with port `2` (marked `BPF-O`). Add some bodge wire at the RF swich footprint (requires good eyes and hands)  to manually switch the VHF and UHF filters to establish a correctly working BPF.

* Next up, solder the voltage regulation section and ensure that you have a valid 3V output at test point `TP3V`. Solder all the components on the `VHF` input line and the 13.8V fuse/choke/1000pF cap. Do the finger test and ensure that the fuse isn't overheating.

* Then, solder the RF switches of the BPF section and test points 1 and 2 again. Inject 3V in pin `VHF` and ensure that you don't have significant losses between `INP` and `BPF-O`, and of course, establish a working RF switch, checking the band of interest performance for each test points.

* Now, solder the `LNA` section and the remaining ancillary circuits: `LNA CTRL` and `RELAY CTRL`. When powering up, ensure that the fuse isn't heating with your digital body temperature sensor. 

* Now, inject signal on `BPF-O` and ensure that you are obtaining a good signal at the connector `3` (marked `AMP-O`) on amp off (no connection) and ~15 dB of gain when 3V feeding on `AMP_ON` pin.

* Moving further, solder the 390 pF decoupling between `1` and `4` and the static/mitigation components. 

* Test points `4` and `5` aren't marked, but the relay footprint is just PERFECT to solder a u.FL connector. Notice the small TVS near to connector `5`.

* Inject signal between test points `4` and `5` (the signal input when measuring S21 should be at `4`). Measure again your gain with the amplified turned off.

* When grounding `TX_GND`, all relays should be off. The same result is yielded when feeding 3V to the `BYPASS` port.

* When injecting 3V to `AMP_ON`, you turn on the amplifier. The presence of a `TX_GND` or `BYPASS` turns it off.

* The test point `TP5V` should be only on when `AMP_ON` is powered. Otherwise, it is off.

* The line `TX_INH` sends 12V when `TX_GND` is asserted.

If you have any further questions, get in touch or file an issue.

Yea, it would be just easier if you bought that 300 EUR filter ;-)

# Specifications
## Power
* 1.3 mA in TX mode, no loaded relays.
* RX mode (relays on) and activating the amplifier, the power consumption jumps to 39 mA.
* RX mode, no relays (`BYPASS` mode) and no amp, the board consumes around 3 mA.
* The board has a PTC fuse rated 50 mA.

## Signal routing
### RX
* The signal enters the SMA Antenna connector
* Enters the first Axicom HF3 relay, duly protected by 1N4148 flywheel
* Finds a Eaton TVS PolySurg surge protector
* Static draining via a 1 kohm and 1000 µH inductor
* Encounters a 390 pF decoupling capacitor
* `INP` u.FL test port
* Second RF switch, selects between VHF or UHF bandpass filter
* Enters the selected BPF
* Third RF switch, the exit section from the bandpass filter
* `BPF-O` test port
* Fourth RF switch, selects between the LNA section or bypass it, no amplification
* Fifth RF switch, LNA/bypass exit
* `AMP-O` test port
* Sixth RF switch, another relay isolation. When in TX, shunted to ground. In RX, move forward
* Second Axicom HF3 relay, duly protected by 1N4148 flywheel
* Exit to radio port via SMA connector

### TX
* The signal enters the board via the SMA Radio connector
* Enters the first Axicom HF3 relay, duly protected by 1N4148 flywheel
* The signal is moved to the second Axicom HF3 relay, duly protected by 1N4148 flywheel
* Exit to the antenna
* The amplifier is de-energized by pulling off the `ENABLE` line from the 5V voltage regulator

## Radio Frequency Performance
### General Specifications
* **Operating Bands:** 144 - 148 MHz (VHF) / 430 - 440 MHz (UHF)
* **RF Connectors:** Through-hole SMA (Edge-launch style)
* **Trace Impedance:** 50-Ohm Grounded Coplanar Waveguide (GCPW)
* **TX Power Handling (Bypass Mode):** 50 Watts CW/SSB
* **Supply Voltage:** 13.8V DC (Main power) / 5V & 3V (Internal LDO logic rails)
* **Diagnostic Test Points:** 5V, 3V, LNA On, Relay On, Relay Logic On, Amp On, Radio TX, Force Bypass

### VHF Pre-Selector (146 MHz)
* **Topology:** 3-Pole Top-Coupled Chebyshev Bandpass Filter
* **Center Frequency:** 146.0 MHz
* **Passband Insertion Loss:** ~2.0 dB
* **VSWR (146 MHz):** 1.40:1 (Return Loss: -15.6 dB)
* **Out-of-Band Rejection:** >60 dB at 100 MHz (FM Broadcast Band)
* **Component Highlights:**
* 5.6 pF Series I/O Matching Capacitors
* 2.4 pF Series Inter-tank Coupling Capacitors
* Orthogonal 56 nH (Outer) and 47 nH (Center) SMD Inductors
* 11.5 pF (Outer) and 18 pF (Center) Shunt Capacitance

### UHF Pre-Selector (439 MHz)
* **Topology:** 2-Pole Capacitively Coupled Parallel LC Resonator
* **Center Frequency:** 439.0 MHz
* **-3 dB Bandwidth:** ~32.5 MHz (426.0 MHz - 458.5 MHz)
* **Passband Insertion Loss:** ~0.00 dB (Lossless resonance peak)
* **VSWR (439 MHz):** 2.21:1 (Return Loss: -8.48 dB)
* **Component Highlights:**
* 4.7 pF Series I/O Matching Capacitors
* 1.86 pF Inter-tank Coupling (Synthesized via 2.4 pF + 8.2 pF in series)
* 20.0 pF Shunt Capacitors

### Low Noise Amplifier (LNA) Stage
* **Active Component:** Infineon BFP460 (SiGe NPN RF Transistor) in SOT-343 package.
* **DC Bias Sweet Spot:** $V_{ce}$ = 3.0V, $I_c$ = ~4.5 mA
* **Raw Gain:** ~15 dB (Fixed)
* **System Noise Figure:** ~2.0 dB to 2.5 dB
* **Stability:** Unconditionally stable (Resistive feedback network via 430-ohm C-B loop and 300-ohm collector load masks internal NF slightly in exchange for a perfectly flat 50-ohm response).

## SPECIFICATIONS
**Overview**
The SignalSurge Rev. E is a high-performance, safe RF front-end and pre-selector designed to sit inline between a transceiver (e.g., Yaesu FT-991A) and the antenna system. It provides high-Q bandpass filtering, low-noise amplification, and an isolated 50-Watt hardware bypass.

**RF Path & Switching**
* **Architecture:** 50-ohm Coplanar Waveguide over Ground (GCPW) on 1.6 mm FR4.
* **Master T/R Switching:** TE Connectivity HF3 Mechanical Relays (~80 dB isolation).
* **Internal Routing:** pSemi PE4259 Solid-State RF Switches (0.5 dB insertion loss).
* **High-Power Bypass:** 1.5 mm GCPW traces capable of handling 50 Watts RF without heating.
* **Fail-Safe Routing:** Hardware logic defaults relays to the NC (Normally Closed) 50W Bypass path upon power loss or transmission.

**Filtering (Pre-Selectors)**
* **Topology:** 2-Pole, Capacitively Top-Coupled Bandpass Filters.
* **VHF Band (146 MHz):** High-Q wirewound inductors (68 nH) with orthogonal placement and ground-plane keep-outs to eliminate parasitic coupling.
* **UHF Band (439 MHz):** VNA-tuned tank circuit (8.2 nH / 8.2 pF) optimized for maximum out-of-band rejection and minimal insertion loss.

**Active Stage (Low Noise Amplifier)**
* **Component:** Infineon BFP460 SiGe RF Transistor.
* **Topology:** Broadband Resistive-Feedback network for unconditional stability across VHF/UHF.
* **Gain:** ~13 to 15 dB (damped for optimal dynamic range and receiver overload protection).
* **Biasing:** Highly linear voltage-collector feedback operating at ~4.5 mA / 3.0V ($V_{CE}$).

**Logic & Control**
* **Control Core:** SN74AHC CMOS Logic (AND/NOR interlocking).
* **Trigger Levels:** 3V logic thresholds with 1k series / 10k pulldown RFI filtering on all external jumper inputs.
* **Inputs:** Radio TX (Ground-to-TX via MMBTRA104SS pre-biased PNP), Force Bypass (3V), LNA Enable (3V), Band Select (3V).
* **Relay Drivers:** UMD5N Dual Digital Transistor (High-Side switching configuration).

**Power & Protection**
* **Input Power:** 13.8V DC (Fused via 100 mA PTC).
* **Regulation:** MIC5205 3.0V LDO (16V Max Input) for logic; TCR1HF50 5.0V LDO for the LNA.
* **ESD Protection:** Eaton 0603ESDA-TR2 Polymer TVS Diodes (0.05 pF ultra-low capacitance) on the RX path.
* **Static Bleed:** Continuous precipitation static drain via 1000 µH / 1k resistor network.
* **Transient Protection:** Snubbed flyback catch diode (1N4148 + 0.1 µF) shunted to ground on the relay coils to protect logic from -200V inductive spikes.
* **Radio Protection:** TX Inhibit line to avoid radio transmission if the relays aren't properly positioned.
