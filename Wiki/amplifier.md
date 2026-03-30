# Amplifier

## Overview
Ragnarock uses a Fosi Audio v1.0G Class D amplifier module. The amplifier is selected for its efficiency, compact size, and ability to deliver high continuous power into a 4 ohm load.

---

## Electrical Characteristics
- Topology: Class D
- Power supply: 20 V at 5 A
- Load impedance: 4 Ω
- Rated output: 100 W RMS, 144 W peak
- THD at 100 W RMS: 0.04 percent (measured)
- Output power: input-side power measured repeatedly; output-side power not measured

The amplifier operates well within its thermal and electrical limits due to upstream DSP protection, which prevents clipping and excessive current draw.

---

## Behavioral Characteristics

### Thermal Behavior
The amplifier is mounted to a thermally conductive internal structure that spreads heat across the enclosure. Under sustained high output, temperature rise is gradual and predictable.

### Protection
The DSP manages:
- Peak limiting
- RMS limiting
- Frequency-dependent excursion control
- Thermal derating

This ensures the amplifier never enters audible clipping or shutdown during normal operation.

### Efficiency
While exact efficiency was not measured, typical Class D behavior suggests high conversion efficiency under load. The 20 V, 5 A supply provides adequate headroom for transient peaks.

---

## Integration Notes
The amplifier is matched to the 4 ohm LF driver and the combined impedance of the MF/HF drivers. DSP-based gain staging ensures that the amplifier operates in its linear region across all playback levels.
