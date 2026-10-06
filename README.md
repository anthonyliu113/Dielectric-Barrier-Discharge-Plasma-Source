# Dielectric Barrier Discharge (DBD) Plasma Source

An atmospheric-pressure, non-thermal plasma source built around a **ZVS Mazzilli driver** and a **hand-wound high-voltage transformer**, with glass-barrier electrodes. The goal is not just to make plasma glow — it's to build an **instrumented** source: measure how much power actually goes into the discharge (Lissajous Q-V method), then use it for surface-activation and ozone experiments.

This is Phase 1–3 of a larger plasma roadmap that ends in a low-pressure DC glow discharge chamber (see [`docs/roadmap.md`](docs/roadmap.md)).

> ⚠️ **High voltage (kV range) and ozone.** Read [`docs/safety.md`](docs/safety.md) before reproducing anything here.

---

## Status

| Subsystem | Status |
|---|---|
| ZVS Mazzilli driver | ✅ Built, running |
| HV transformer (2+2 turn CT primary / 660 turn secondary) | ✅ Rewound — strike voltage dropped from ~30 V to **~14 V** ZVS input |
| Glass-barrier electrode assembly (asher) | ✅ Built, discharge strikes reliably |
| Lissajous Q-V measurement front-end | ✅ Simulated in LTspice, circuit built — **bench validation in progress** |
| Analysis pipeline (Python) | ✅ Validated against SPICE ideal-DBD model (recovers C_d, C_g, U_b, power within ~1%) |
| Power-vs-input-voltage characterization | 🔜 Next |
| Surface activation (contact angle) DOE | 🔜 Planned |
| Ozone / PAW characterization | 🔜 Planned |
| Optical emission spectroscopy (OES) | 🔜 Planned |

## System overview

```
 DC bench supply ──► ZVS Mazzilli ──► HV transformer ──► DBD electrodes ──┐
 (current limited)   (resonant tank)   (660:2+2 CT)       (glass barrier)  │
                                                                          │
         ┌───────────── Capacitive HV divider (100 pF : 100 nF) ◄──────────┤  CH1 = V(t)
 Scope ──┤                                                                │
 THS720A └───────────── Series measurement cap (10 nF) ◄──────────────────┘  CH2 = Q(t)
                         (Zener clamp + 1 MΩ bleeder)

 Plot CH2 vs CH1 (XY mode)  ──►  parallelogram  ──►  area × f = discharge power
```

![Simulated Q-V Lissajous](images/qv_lissajous_simulated.png)
*Simulated ideal-DBD Lissajous from the SPICE model, run through the same script used on real captures. Real captures will replace this.*

## Repository layout

| Path | Contents |
|---|---|
| [`docs/theory.md`](docs/theory.md) | DBD physics, Paschen's law, why the Q-V loop is a parallelogram |
| [`docs/hardware.md`](docs/hardware.md) | Driver, transformer rewind, electrode design, build notes |
| [`docs/qv-measurement.md`](docs/qv-measurement.md) | Lissajous power measurement — circuit, calibration, procedure |
| [`docs/safety.md`](docs/safety.md) | HV and ozone safety rules used for every session |
| [`docs/roadmap.md`](docs/roadmap.md) | Characterization DOE plan and the path to vacuum plasma |
| [`hardware/BOM.md`](hardware/BOM.md) | Bill of materials |
| [`analysis/`](analysis/) | Python: Paschen curves and Lissajous power from scope CSVs |
| [`simulation/`](simulation/) | SPICE model of the Q-V measurement front-end |
| `images/` | Build photos and scope captures |

## Quick start (analysis)

```bash
cd analysis
pip install -r requirements.txt

# Paschen curves for air, N2, Ar, He
python paschen.py

# Discharge power from a scope capture (CSV: time, V_divider, V_meas)
python lissajous_power.py capture.csv --divider-ratio 1000 --cm 10e-9

# Self-test on a synthetic ideal-DBD waveform (no hardware needed)
python lissajous_power.py --demo
```

## Skills demonstrated

- Resonant power electronics (ZVS / Royer-class oscillator) and HV magnetics design
- High-voltage measurement without an HV probe (capacitive divider design + calibration)
- Plasma diagnostics: Lissajous Q-V power, Manley's equation, burning-voltage extraction
- Simulation-before-hardware workflow (LTspice) and Python data analysis
- HV / ozone safety practice

## Author

**Anthony Connolly** ([@anthonyliu113](https://github.com/anthonyliu113)) — Electrical Engineering, Wentworth Institute of Technology

## License

MIT — see [LICENSE](LICENSE).
