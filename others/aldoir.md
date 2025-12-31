# Prompt
I’m developing a radio frequency amplifier that needs 50V DC and this voltage is supplied by an on board AC to DC switching power supply. Should I share the same ground plane in the power supply with the amplifier stage?

# Answer
This is a critical design decision. **Yes, they must share the same electrical potential (DC reference), but you should essentially treat them as separate "zones" on your PCB.**

If you simply pour a single, unmanaged ground plane across the entire board, the high-frequency switching noise (ripple and spikes) from the power supply return currents will couple directly into your RF amplifier's ground reference, likely modulating your carrier or raising your noise floor.

Here is the best-practice approach to managing this shared ground connection.

### 1. The "Virtual Split" Strategy (Recommended)

Rather than physically cutting the copper (which can create accidental dipole antennas if you aren't careful), use a **continuous solid ground plane** but manage it through component placement. This is often safer than a physical split for RF designs.

* **Placement:** Place all SMPS components on one side of the board and all RF components on the other.
* **The "Invisible Wall":** Imagine a line drawn between the two sections. Do not route any traces across this line except for the main DC power rail.
* **Return Currents:** Electricity flows in loops. High-frequency return currents follow the path of least inductance, which is directly underneath the signal trace. By segregating the components, the noisy switching currents stay in the SMPS area and the RF return currents stay in the RF area. They naturally won't mix because their loops don't overlap.

### 2. The "Star Ground" Connection Point

If you prefer to physically separate the ground pours (creating two distinct copper islands), you must connect them at exactly **one point** to avoid ground loops.

* **Where to connect:** The connection point should be exactly at the **output capacitor of the switching power supply**.
* **Why:** This is the "quietest" point of the power supply. The switching noise loop circulates between the switch (MOSFET), the inductor, and the input/output caps. By tapping your RF ground off the output cap, you are pulling power from the stable DC reservoir, not the noisy switching loop.

### 3. Protection on the Power Rail (+50V)

Sharing the ground is only half the battle. You must prevent noise from entering through the +50V rail.

* **Pi-Filter:** Place a Pi-filter (Capacitor - Inductor - Capacitor) on the +50V line right where it crosses from the power supply zone into the RF zone.
* **Feedthrough Capacitors:** If the RF section is shielded (in a metal can), use a feedthrough capacitor to bring the +50V inside. This acts as an excellent low-pass filter, shunting high-frequency switching noise to the chassis/ground before it enters the amplifier.

### 4. Summary Checklist

| Feature | Recommendation | Why? |
| --- | --- | --- |
| **Ground Plane** | **Solid (Continuous)** | Splitting planes under RF traces creates return path discontinuities that radiate noise. |
| **Layout** | **Zoned** | Keep SMPS components tightly grouped. Keep RF components tightly grouped. Don't mix them. |
| **Connection** | **Common DC Point** | The SMPS output capacitor is the anchor point for the system ground. |
| **Shielding** | **SMPS Cage** | Consider placing a metal shield can over the SMPS section to prevent radiated coupling to the RF stage. |

**Critical Warning:** Never route an RF signal trace across a cut/split in a ground plane. The return current will be forced to detour around the cut, creating a large loop area that acts as an antenna, radiating and receiving noise.

**Would you like me to calculate the values for a Pi-filter on your 50V line to target your specific switching frequency?**
