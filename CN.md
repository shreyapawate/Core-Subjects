# 5. Physical Layer

## Definition
The Physical Layer is **Layer 1 of the OSI model**. It transmits raw bits (`0s` and `1s`) as physical signals through a transmission medium.

## Main Functions
- Bit transmission
- Defines data rate and signaling
- Defines physical interfaces/connectors
- Transmission media specification
- Bit synchronization
- Transmission mode

## Transmission Media

### Guided
- **Twisted Pair** – Copper; commonly used in LANs
- **Coaxial Cable** – Copper; better shielding
- **Fiber Optic** – Light; high bandwidth, long distance, EMI resistant

### Unguided
- Radio waves
- Microwaves
- Infrared

## Important Devices
- **Repeater** – Regenerates signals to extend distance
- **Hub** – Repeats incoming signals to all ports

## Important Concepts

| Concept | Meaning |
|---|---|
| Bit Rate | Bits transmitted per second |
| Baud Rate | Symbols transmitted per second |
| Attenuation | Loss of signal strength |
| Distortion | Change in signal shape |
| Noise | Unwanted interference |

## Delay Formulas

**Transmission Delay:**
`T = L / R`

**Propagation Delay:**
`T = d / v`

Where:
- `L` = packet size in bits
- `R` = data rate
- `d` = distance
- `v` = propagation speed

## Key Point
**Physical Layer = Layer 1 = Bits + Signals + Transmission Medium**
