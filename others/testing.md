# Test sequence
![Board picture - test sequence](https://github.com/rfrht/SignalSurge/blob/main/others/test-sequence.jpg)

* Start soldering the highlighted u.FL connectors and the VHF/UHF BPF discretes ONLY.

* Then, inject signal at u.FL connector `1` (marked `INP`) and measure S21 with port `2` (marked `BPF-O`). Do it BEFORE installing the RF switch, add some bodge wire to manually switch the VHF and UHF filters to establish a corretly working BPF.

* Next up, solder the voltage regulation session and ensure that you have a valid 3V output at test point `TP3V`. Solder all the components on the `VHF` input line and the 13.8V fuse/choke/1000pF cap. Do the finger test and ensure that your fuse isn't overheating.

* Then, solder the RF switches of the BPF section and test points 1 and 2 again. Inject 3V in pin `VHF` and ensure that you don't have significant losses between `INP` and `BPF-O`, and of course, establish a working RF switch, checking the band of interest performance for each test points.

* Now, solder the `LNA` section and the remaining ancillary circuits: `LNA CTRL` and `RELAY CTRL`. DO NOT solder the `SW-IS` isolation rf switches. When powering up, ensure that your fuse isn't heading with your digital body temperature sensor. 

* Now, inject signal on `BPF-O` and ensure that you are obtaining a good signal at the connector `3` (marked `AMP-O`) on amp off (no connection) and ~15 dB of gain when 3V feeding on `AMP_ON` pin.

* Moving further, solder the `SW-IS` RF switches, the 390 pF decoupling between `1` and `4` and the static/mitigation components. 

* Test points 4 and 5 aren't marked, but the relay footprint is just PERFECT to solder a u.FL connector. Notice the small TVS near to connector `5`.

* Inject signal between test points `4` and `5` (the signal input when measuring S21 should be at `4`). Measure again your gain and when the amplifier is disconnected.

* When grounding `TX_GND`, all relays should be off, the `IS` RF switches will isolate. The same result is yielded when feeding 3V to the `BYPASS` port.

* When injecting 3V to `AMP_ON`, you turn on the amplifier. The presence of a `TX_GND` or `BYPASS` turns it off.

* The test point `TP5V` should be only on when `AMP_ON` is powered. Otherwise, it is off.

* The line `TX_INH` sends 12V when `TX_GND` is asserted.

If you have any further questions, get in touch or file an issue.

Yea, it would be just easier if you bought that 300 EUR filter ;-)

# Specifications
## Power
* 1.3 mA in TX mode, no loaded relays.
* Without the relays, in RX and activating the amplifier, the power consumption jumps to 10 mA.
* RX mode, no relays and no amp, the board consumes around 3 mA.
* The board has a PTC fuse rated 50 mA.

## Signal routing
### RX
* The signal enters the SMA Antenna connector
* Enters the first Axicom HF3 relay, duly protected by 1N4148 flywheel
* Finds a Eaton TVS PolySurg surge protector
* Static draining via a 1 kohm and 1000 µH inductor
* Encounters a 390 pF decoupling capacitor
* First `IS` RF switch, pSemi 4259-63 (all of them are the same model). When in TX mode, any incoming signal is shunted to ground. In RX, move forward.
* `INP` u.FL test port
* Second RF switch, selects between VHF or UHF bandpass filter
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
* The `IS` RF switches are shunted to ground.
* The amplifier is de-energized by pulling off the `ENABLE` line from the 5V voltage regulator

## Radio Frequency Performance
### VHF
All data, except when noted otherwise, are with amplifier off. Data measured after traversing the full signal chain.
* Start of band gain: 144 MHz, -5.8 dB
* Best gain: 146.5 MHz, -4.7 dB
* End of band gain: 148 MHz, -5.1 dB

#### FM Broadcast rejection
* 76 MHz: -55 dB
* 108 MHz: -33 dB

#### Amplifier gain:
* Add 16 dB to the numbers.

### UHF
* Start of band gain: 430 MHz, -12.2 dB
* End of band gain: 440 MHz, -7.5 dB

#### Amplifier gain:
* Add 16 dB to the numbers.
