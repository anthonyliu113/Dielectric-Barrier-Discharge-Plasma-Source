# Hardware

## 1. ZVS Mazzilli driver

A Mazzilli zero-voltage-switching oscillator: two N-channel MOSFETs cross-coupled through diodes, driving a center-tapped primary that resonates with a tank capacitor. The MOSFETs switch only near zero drain voltage, so switching loss is low and no gate-driver IC is needed.

```
   +Vin ──[ L choke ]──┬── CT of primary
                       │
        ┌── Q1 drain ──┤ primary half A ├──┐
        │              │   ‖ C_tank ‖     │
        └── Q2 drain ──┤ primary half B ├──┘
   Gates: pulled up through resistors, cross-clamped by fast diodes to the opposite drain
   Zeners on each gate (≈12 V) to protect gate oxide
```

Resonant frequency is set by the primary inductance and tank capacitance:

$$f_r = \frac{1}{2\pi\sqrt{L_p C_{tank}}}$$

The secondary is loaded by the DBD (mostly capacitive), which pulls the frequency down slightly once the discharge strikes — this shift is something to record during characterization.

**Why ZVS for v1:** simple, robust, self-oscillating, cheap. **Limitation:** frequency isn't independently adjustable, which constrains frequency sweeps in the DOE. An adjustable-frequency driver (e.g., a half-bridge or flyback with a controllable oscillator) is planned for the DOE phase — see [roadmap](roadmap.md).

## 2. HV transformer

| Parameter | Original | After rewind |
|---|---|---|
| Primary | — | **2 + 2 turns, center-tapped** |
| Secondary | — | **660 turns** |
| Strike voltage (ZVS input) | ~30 V | **~14 V** |

Rewinding to fewer primary turns raises the effective turns ratio (660 : 2 per half), so the same input voltage produces a higher secondary voltage and the gap breaks down at roughly half the input.

**Idealized secondary estimate.** In a Mazzilli the peak voltage across each primary half is ≈ π·V_in, so

$$V_{sec,pk} \approx \pi V_{in} \cdot \frac{N_s}{N_{p,half}} = \pi \cdot 14 \cdot \frac{660}{2} \approx 14.5\ \text{kV}$$

This is an **upper bound** — leakage inductance, core losses, and discharge loading all pull it down. The real value will be measured with the capacitive divider (see [qv-measurement.md](qv-measurement.md)).

Winding notes:
- Secondary layered with insulation between layers to prevent inter-layer arcing
- Primary wound with heavy-gauge wire (high circulating current in the tank)
- Keep the secondary start/finish leads physically separated

## 3. DBD electrode assembly (plasma asher)

- Glass tube electrodes act as the dielectric barrier — the glass blocks DC and forces the self-limiting DBD behavior
- Configuration: volume DBD across a small air gap between barrier-covered electrodes
- Used as a plasma asher: sample placed in/near the discharge region to oxidize organic residue

> 📷 TODO: add photos to `images/` — driver board, transformer, electrode assembly, discharge (lights off).

## 4. Measurement front-end

See [qv-measurement.md](qv-measurement.md) for full detail. In short: 100 pF HV ceramic discs over a 100 nF film cap form a ~1000:1 capacitive divider (no HV probe needed), and a 10 nF film cap in the ground return integrates discharge current into charge.

## Lessons learned

- **Turns ratio matters more than input voltage.** Rewinding the primary halved the strike voltage — less stress on the MOSFETs and supply for the same discharge.
- **Simulate the measurement before trusting it.** The Q-V front-end was modeled in LTspice (sine drive, XY plot) before going on the bench.
- **Component values drift from their labels.** The "100 pF" HV discs measure ~0.18 nF on the meter, so the divider ratio has to be calibrated, not assumed.
