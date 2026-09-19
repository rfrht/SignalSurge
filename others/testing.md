# Device under Test
![OOB VHF BPF](https://github.com/rfrht/SignalSurge/blob/main/others/dut.jpg)

# Test sequence
![Board picture - test sequence](https://github.com/rfrht/SignalSurge/blob/main/others/test-sequence.jpg)

* Start soldering the u.FL connectors `1` and `2`. Then the VHF/UHF BPF **discretes (caps & inductors) only**. Do not solder the RF switch just yet.

* Start testing with your VHF filter. Solder a bodge wire at the BPF RF switches footprints: They are `SW_BPF_IN` AND `SW_BPF_OUT` - requires good eyes and hands - to manually switch the VHF and UHF filters. Start bridging the u.FL with the **VHF BPF**.

* Now, measure the BPF performance. Inject signal from VNA Port 1 (Source) into the `INP` port (marked 1 in the image) and connect the VNA port 2 at the port `BPF-O`. Ensure that you have results similar to my [test results](https://github.com/rfrht/SignalSurge/blob/main/rf-performance.md).

* When all is fine, change your bodge wire to **switch it manually to UHF**. Redo the above test.

* When all is fine, remove the bodge wire from the `SW_BPF` RF switches. You might want to use solder wick to make your component footprint plain and remove excess solder. 

* Next up, solder the LOWER SECTION of the board: everything south of the BPF.

* Ensure that you have a solid 3V output at test point `TP3V`. Ensure that your fuse or any components (try with your fingers) aren't overheating.

* Feed 3V to the line `AMP_ON` header. Check the testpoint `TP_LNA_ON` have 3V and if your 5V regulator is throwing out a 5V output at the `TP5V` test point or the 5V terminal.

* Moving to the LNA. Solder a bodge wire at the `SW_LNA_IN` and `SW_LNA_OUT` pads, to bridge the U.FL lines `2` and `3` with the LNA stage.

* Inject signal from VNA Port 1 (Source) into the `BPF-O` connector, and measure the amplified output using VNA Port 2 (Receiver) at the `AMP-O` connector to correctly read S21 forward gain. Ensure that you are obtaining a good gain. If you mix the port order you will have a WRONG read! VNA port ordering is key here: Port 1 at `BPF-O`, Port 2 at `AMP-O`! Compare with my tests. And finally, ensure you soldered properly your BFP460. If you aren't getting any amplification, ensure that you didn't mix the VNA ports, have powered the LNA by powering the header `AMP_ON`.

* Remove the bodge wires. You may now solder the LNA RF switches.

* Redo the tests between the U.FL `BPF-O` and `AMP-O`, both ways: Amplifying and on bypass mode. Validate our outputs to establish a correctly working RF switch. NOTICE: they are FRAGILE! I damaged a number of them with a hot iron.

* Solder the BPF RF switches. Establish a working BPF switching between the u.FL connectors `INP` and `BPF-O`. Flip the switches by toggling the `VHF` line with 5V. Tip: You can tap 5V from the board header by turning on the amp by feeding 3V to the `AMP_ON` header.

* Solder the remaining parts at the UPPER SECTION of the board: First the SMD components, then the SMA port and finally the relays.

## Operational tests:

* When grounding `TX_GND`, all relays should be off. The same result is yielded when feeding 3V to the `BYPASS` port.

* When injecting 3V to `AMP_ON`, you turn on the amplifier. The presence of a `TX_GND` or `BYPASS` overrides and turns it off.

* The test point `TP5V` should be on only when `AMP_ON` has 3V at the header. Otherwise, it should be off.

* The line `TX_INH` sends 13.8V when the board is in RX mode (BPF/LNA engaged). When `TX_GND` or `BYPASS` are asserted, the board falls back to hardware bypass and the `TX_INH` line drops to 0V, safely allowing the radio to transmit.

* At the back of the board, there are the following tools: 2 pole filter, 3 pole filter and u.FL SOLT calibration pads.

If you have any further questions, get in touch or file an issue.

Yea, it'd be just easier if you bought that 300 EUR filter ;-)

## Power Consumption & Expected Current Draw
When troubleshooting the SignalSurge board, a standard multimeter placed in series with the main 13.8V power supply is your best diagnostic tool. The board operates at very low currents, making it easy to spot a blown component, a dead short, or a failed logic gate just by reading the milliamp (mA) draw.

The board is protected by a 50 mA PTC resettable fuse. If your board draws significantly more than the values below, immediately disconnect power and check for solder bridges/blobs/shorts or reversed diodes.

### Baseline Current Measurements
| Operational State | Expected Draw | Diagnostic Meaning |
| --- | --- | --- |
| **TX Mode / Manual Bypass** | **~0.5 mA** | **Quiescent State.** The Axicom HF3 relays are de-energized (hardware bypass) and the 5V regulator is either in shutdown or drawing minimal quiescent current. If this reads 0 mA, check your primary power connection. If it reads more than 1 mA, check the relay flyback diodes for leakage or shorts. |
| **RX Mode (BPF Only)** | **~18 mA** | **Relays Engaged.** The relay logic has successfully commanded the board to listen. The ~17.5 mA jump is the coil current required to physically hold the relays open, routing the RF signal through the PE4259 switch matrix and passive filters. |
| **RX Mode (BPF + LNA)** | **~24 mA** | **Full Active System.** The BFP460 amplifier is energized. The difference between this and the previous state is **~6 mA**. If the current jumps to > 30 mA here, the BFP460 is likely damaged or oscillating. |

### Being pedantic about power modes
* **Logic Override Test (`AMP_ON` + `TX_GND`):**
If `AMP_ON` is enabled and you trigger `TX_GND`, the current should instantly drop from **24 mA back to around 1 mA**.
* **VHF vs. UHF Logic Test:**
Toggling the FT-991A from 2-meters to 70cm (triggering BCD Pin 4) DOES NOT change the current draw. Switching between the PE4259 routing paths should result in a statistically zero difference in current (microamps). If you see a noticeable mA jump when switching bands, one of the RF switches is likely shorted to ground.
* **`TX_INH` Load Test:**
When connecting `TX_INH` to the radio, the `TX_INH` line sends +13.8V back to the radio and the total board current rises by around 1 mA when physically connected to the transceiver.

# Specifications
## Signal routing
### RX
* The signal enters the SMA Antenna connector
* Enters the first Axicom HF3 relay
* Finds a Eaton TVS PolySurge surge protector
* Static draining via a 1 kohm and 1000 µH inductor
* Encounters a 390 pF decoupling capacitor
* `INP` u.FL test port
* First RF switch, selects between VHF or UHF bandpass filter
* Enters the selected BPF
* Second RF switch, the exit section from the bandpass filter
* `BPF-O` test port
* Third RF switch, selects between the LNA section or bypass it, no amplification
* Fourth RF switch, LNA/bypass exit
* `AMP-O` test port
* Finds a Eaton TVS PolySurge surge protector
* Second Axicom HF3 relay
* Exit to radio port via SMA connector

All relays are protected by a 100 ohm current limiter resistor, a 1N4148 flywheel diode and a 0.1µF capacitor.

### TX
* The amplifier is de-energized by pulling off the `ENABLE` line from the 5V voltage regulator when detecting the `TX_GND` signal
* The signal enters the board via the SMA Radio connector
* Enters the first Axicom HF3 relay
* The signal is moved to the second Axicom HF3 relay
* Exit to the antenna


## Radio Frequency Performance
### General Specifications
* **System Impedance:** 50 ohms
* **RF Switching Matrix:** Four cascaded pSemi PE4259 ultra-high isolation RF switches
* **Switch Matrix Insertion Loss (Bypass Mode):** ~1.5 dB at 144 MHz; ~3.0 dB to 4.1 dB at 440 MHz
* **System Architecture:** Independent electronic selection for VHF/UHF filter paths and LNA amplification, backed by a hardware-level transmit ground (`TX_GND`) fail-safe bypass

### VHF Bandpass Filter (2-Meter Band)
* **Topology:** 3-pole capacitively-coupled LC bandpass filter built with Johanson High-Q wirewound inductors and C0G low-ESR capacitors
* **Center Frequency Target:** 144.0 MHz – 148.0 MHz
* **Passband Insertion Loss:** 1.33 dB at 144.9 MHz
* **Passband Return Loss (S11):** Better than -10 dB, dipping to -23.7 dB (VSWR 1.13:1)
* **FM Broadcast Rejection (88–108 MHz):** -59 dB to -69.4 dB attenuation. This extreme low-side skirt prevents local 100 kW commercial FM transmitters from driving the active stage into non-linear intermodulation.
* **Aviation Band Rejection (110–130 MHz):** >45 dB of attenuation below 110 MHz

### UHF Bandpass Filter (70-Centimeter Band)
* **Topology:** 2-pole capacitively-coupled LC bandpass filter using Johanson High-Q components
* **Center Frequency Target:** 439.0 MHz (Optimized for Brazilian repeater outputs)
* **Passband Insertion Loss:** 1.05 dB to 1.12 dB at 439.0 MHz
* **Absolute Peak Insertion Loss:** 0.90 dB near 444.0 MHz
* **Impedance Matching:** Utilizes 8.2 pF input/output series matching capacitors to overcome the inherently low characteristic impedance of the 3.9 nH core tanks. This ensures that the ~1 dB of measured loss is strictly reflective mismatch rather than absorptive thermal dissipation, preserving signal integrity before amplification.

### Low Noise Amplifier (LNA) Stage
* **Active Component:** Infineon BFP460 wideband NPN RF transistor
* **Topology:** Wideband shunt-shunt feedback LNA with a passive resistor-divider bias network
* **DC Operating Point:** Conservatively biased at approximately 4 mA Collector Current (Ic) and 3.2V Collector-Emitter Voltage (Vce). This low-power state ensures absolute unconditional stability and preserves the transistor's low intrinsic noise figure (~1.1 dB).
* **Raw Active Gain:** +15 dB to +16 dB broadband gain across both VHF and UHF frequencies
* **Net System Gain (Filter + Matrix + LNA):** +11.97 dB at 144 MHz and +11.90 dB at 439 MHz. The amplifier completely overcomes the combined insertion losses of the passive filters and the 4-stage switch matrix.

## System Protections & Operational Logic

### Hardware & Transmit Protections
SignalSurge is designed to sit directly in the primary RF path of a 50W transceiver. To prevent accidental destruction of the highly sensitive receive components, the board employs multiple layers of hardware-level protection:
* **Fail-Safe RX Architecture:** The primary signal path is controlled by robust Axicom HF3 mechanical relays. Their unpowered, default state is a direct hardware bypass (Antenna -> Radio), ensuring the transceiver can safely transmit even if the board loses power.
* **Transmit Inhibit (`TX_INH`):** When the active RX chain (filters and LNA) is engaged, the board outputs a continuous +13.8V signal to the Yaesu FT-991A's TUN/LIN port. This physically inhibits the radio from transmitting, ensuring 50W of RF is never dumped into the pSemi PE4259 switches (which have a maximum rating of 2W / +33 dBm).
* **Absolute TX Override (`TX_GND`):** Grounding the `TX_GND` line forces the Axicom relays into bypass mode and actively pulls the `ENABLE` line on the 5V regulator low, instantly de-energizing the BFP460 amplifier. This signal trumps all other board logic.
* **ESD & Static Drainage:** Both the antenna and radio ports are guarded by Eaton TVS PolySurge protectors. The antenna port also features a 1 kR resistor and 1000 µH inductor network to bleed off atmospheric static charge before it can reach the solid-state switching matrix.
* **Relay Coil Suppression:** Every mechanical relay coil is isolated with a 100 R current limiting resistor, a parallel 1N4148 flyback diode, and a 0.1 µF decoupling capacitor to suppress inductive voltage spikes during switching.
* **Overcurrent Defense:** The main power rail is protected by a PTC resettable fuse to guard against dead shorts.

### Automated Band Logic & Routing
The board utilizes a hybrid switching approach to maximize both power handling and receive isolation.
* **Solid-State RX Routing:** While the Axicom relays handle the high-power TX bypass, the internal receive path is entirely governed by four pSemi PE4259 RF switches. These provide ultra-high isolation between the VHF filter, UHF filter, and LNA stages without the mechanical wear or contact bounce of traditional relays.
* **Yaesu BCD Integration:** SignalSurge achieves "set-and-forget" automation by reading the BCD encoder outputs from the FT-991A's TUN/LIN port. By monitoring Pin 4 (Band Data B), the board automatically flips the internal solid-state switches between the 144 MHz and 430 MHz bandpass filters exactly as you change bands on the radio dial.
* **Manual Overrides:** Independent +3V header lines for `AMP_ON` and `BYPASS` allow the operator to manually force the LNA on or drop the entire board into hardware bypass.

