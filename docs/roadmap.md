# Roadmap

This repo covers the **atmospheric DBD platform**. It's the first half of a two-part plasma portfolio:

1. **Instrumented atmospheric non-thermal plasma platform** (this repo) — Phases 1–3
2. **Low-pressure plasma processing tool** — DC glow discharge in a vacuum chamber (separate project)

## Phase 1 — Surface activation

- **Goal:** show the plasma measurably changes a surface.
- **Diagnostic:** contact-angle goniometry — phone camera + ImageJ (drop shape analysis).
- **Method:** establish untreated baselines *before* any plasma (metrology first), then treat samples (glass slides, polymer film) for set times and measure water contact angle.
- **Deliverable:** contact angle vs. treatment time plot, with repeat measurements.

## Phase 2 — Ozone / plasma-activated water (PAW)

- **Goal:** quantify reactive species output.
- **Diagnostics:** KI titration (ozone), Griess reagent (nitrite/nitrate in PAW), pH.
- **Deliverable:** ozone yield (g/kWh) using the Q-V discharge power as the denominator.

## Phase 3 — Characterization DOE

- **Factors:** input voltage (→ discharge power), gap, frequency*, treatment time, gas
- **Responses:** discharge power (Lissajous), contact angle, ozone yield, OES line intensities
- **Diagnostics:** Lissajous Q-V, optical emission spectroscopy (N₂ second positive system, O 777 nm, OH 309 nm)
- *Frequency control needs an adjustable-frequency driver (ZVS frequency is set by the tank) — planned upgrade.

## Next: vacuum

The same measurement mindset moves to a low-pressure system:

1. Rough vacuum with a dry diaphragm pump (~1 Torr floor) and a Pirani gauge with a custom ESP32 reader
2. DC glow discharge in a borosilicate tube (Ar, N₂, He, CO₂)
3. Deeper vacuum (second-stage pump) → sputtering → possibly CCP

Atmospheric DBDs are relevant to surface activation; real IC etch happens in low-pressure plasmas (CCP/ICP-RIE). Going from one to the other is the point of the roadmap.

## Checklist

- [x] ZVS driver built
- [x] Transformer rewound (strike ~30 V → ~14 V input)
- [x] Electrode assembly / asher working
- [x] Q-V front-end simulated (LTspice) and built
- [ ] Divider calibration
- [ ] First Lissajous captures + power vs. input voltage
- [ ] Add DBD breakdown model to SPICE sim, compare to measurement
- [ ] Contact-angle baseline set
- [ ] Surface activation runs
- [ ] Ozone / PAW measurements
- [ ] OES setup
- [ ] Adjustable-frequency driver
