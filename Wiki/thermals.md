# Thermal Management

## Overview
Ragnarock is engineered to maintain stable performance during extended high-output operation. Thermal behavior is managed through a combination of passive heat dissipation, enclosure-integrated heat spreading, and DSP-based protection. The system is designed to avoid abrupt shutdowns and to maintain consistent sound quality even under sustained load.

---

# Heat Sources

## Amplifier Module
The Class D amplifier generates heat primarily through switching losses and conduction losses. At 100 W RMS output, measured THD is 0.04 percent, indicating operation within the linear region. The amplifier is powered by a 20 V, 5 A supply, which defines the maximum thermal envelope.

## Low-Frequency Driver
The Tang Band W6-1139SIF voice coil dissipates heat during high-excursion operation. The 4-layer coil and deep gap geometry allow for efficient heat transfer into the motor structure.

## DSP Hardware
The Raspberry Pi and HiFiBerry DAC/ADC generate minimal heat relative to the amplifier and driver. Their thermal load is negligible in the context of the full system.

---

# Passive Cooling System

## Enclosure Heat Spreading
The amplifier is mounted to a thermally conductive internal structure that distributes heat across the hardwood enclosure. Aspen has moderate thermal conductivity, which helps prevent localized hotspots.

## Internal Airflow
The 9.5 L enclosure provides sufficient internal air volume for natural convection. The passive radiator and driver movement promote gentle air circulation, which aids in heat distribution.

---

# DSP-Based Thermal Protection

## Real-Time Monitoring
The DSP monitors:
- Amplifier load behavior
- Driver excursion
- Long-term RMS energy
- Frequency-dependent stress

Although the system does not include direct temperature sensors, thermal behavior is inferred from electrical and mechanical load patterns.

## Thermal Derating
When sustained high output is detected, the DSP gradually reduces gain in the low-frequency band. This reduces voice coil heating and amplifier current draw. The derating curve is designed to be smooth and unobtrusive.

## Recovery Behavior
Once thermal load decreases, full output capability is restored automatically. Recovery is rapid due to the efficiency of the passive cooling system.

---

# Design Intent
The thermal management system is designed to:
- Maintain consistent performance during extended playback
- Prevent audible clipping or shutdown
- Protect the amplifier and drivers from thermal stress
- Recover quickly after high-load operation
