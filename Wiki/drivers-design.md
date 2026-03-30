# Driver Design

## Overview
Ragnarock uses a three-transducer system built around a high-excursion low-frequency driver, a pair of compact mid/high drivers, and a large passive radiator. Each component is selected for its mechanical stability, material quality, and predictable behavior under high dynamic load.

---

# Low-Frequency Driver: Tang Band W6-1139SIF ("The Kraken")

## Mechanical Construction
- Diameter: 6.5 in
- Cone: Paper
- Surround: Wide-roll rubber
- Basket: Stamped steel
- Voice coil: 38.5 mm, 4-layer
- Former: High-temperature Kapton
- Magnet: Ferrite, 826 g
- Air gap height: 6 mm

The motor structure is optimized for linear force across the usable excursion range. The 4-layer coil and deep gap geometry maintain consistent BL even at high displacement.

## Performance Characteristics
- Frequency range: 1 to 500 Hz (measured)
- Xmax: 12 mm one-way (measured)
- Power handling: 50 W RMS, 100 W peak
- Sensitivity: 83 dB (1 W / 1 m)

The driver is capable of unusually high displacement for its size, which allows Ragnarock to achieve strong low-frequency output from a compact enclosure.

## Thiele/Small Parameters
- Fs: 35 Hz
- Qts: 0.40
- Qms: 2.65
- Qes: 0.47
- Vas: 11.78 L
- Mms: 39.9 g
- Cms: 599 µm/N
- Sd: 140 cm²
- Le: 0.47 mH
- Re: 3.6 Ω
- Bl: 8.47 T·m

These parameters support a compact, low-tuned alignment with strong mechanical control and predictable behavior under DSP-managed excursion limits.

---

# Mid/High Drivers: Samsung OEM 2-inch ("Bonnie & Clyde")

## Mechanical Construction
- Diameter: 2 in
- Cone: Paper
- Surround: Rubber
- Basket: Brass
- Voice coil: 0.5 in
- Xmax: approximately 2 mm
- Magnet: Ferrite

These drivers were manufactured by Samsung in the late 1990s and are known for their unusually high build quality for their size class. The brass basket provides rigidity and thermal stability, while the paper cone maintains low mass and smooth breakup behavior.

## Performance Characteristics
- Frequency range: 500 Hz and above
- Power handling: 20 W RMS, 44 W peak (combined pair)

The drivers operate above the dynamic crossover point and are protected by a 38-band compressor that prevents over-excursion and thermal stress.

## Thiele/Small Parameters
Not available. These drivers were OEM units and lack published T/S data. Their behavior is characterized empirically through DSP tuning rather than classical enclosure modeling.

---

# Passive Radiator: Dayton Epique E180HE-PR ("Riptide")

## Mechanical Construction
- Diameter: 181 mm frame (8 in class)
- Cone: Carbon fiber
- Surround: Large-roll rubber
- Basket: Cast aluminum
- Xmech: 19 mm one-way
- Sd: 136.1 cm²
- Vd: 258.6 cm³

The carbon fiber diaphragm and long-throw suspension allow the radiator to handle the full displacement demands of the low-frequency driver without compression or mechanical noise.

## Suspension and Tuning
- Base moving mass: 110 g
- Tuning mass options: 110 g, 143.5 g, 172 g, 200.7 g
- Cms: 0.48 mm/N
- Rms: 4.34 kg/s
- Qmpr: 3.5 to 4.7 depending on mass
- Vapr: 12.52 L
- System tuning: 31 Hz (measured)

The radiator is tuned to 31 Hz in Ragnarock, providing deep extension while maintaining transient accuracy.

---

# System Integration Notes

The three-transducer system is managed by a linear-phase dynamic crossover and multi-stage protection compressors. The LF driver and passive radiator form a low-tuned alignment that delivers strong bass output from a 9.5 L enclosure, while the dual 2-inch drivers provide wide dispersion and stable mid/high performance.
