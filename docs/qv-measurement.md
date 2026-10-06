# Lissajous (Q-V) Power Measurement

Measuring DBD power by multiplying a voltage probe by a current probe doesn't work well: discharge current is a train of nanosecond filaments riding on a large displacement current. The **Q-V Lissajous** method integrates the current in hardware instead, using a series capacitor.

## Circuit

```
   HV out ──┬──────────────► DBD cell ──────────┬──────► CH2 (V_m)
            │                                   │
          C_top  100 pF / 15 kV ceramic        C_m  10 nF CBB film
            │                                   │   ‖ Zener clamp (both polarities)
            ├──────────────► CH1 (V_div)        │   ‖ 1 MΩ bleeder
            │                                   │
          C_bot  100 nF CBB film               GND
            │
           GND
```

| Part | Role | Notes |
|---|---|---|
| C_top (100 pF, 15 kV ceramic disc) | Divider HV arm | Measures ~0.18 nF on meter — **calibrate** |
| C_bot (100 nF CBB film) | Divider low arm | Low-loss film, stable |
| C_m (10 nF CBB film) | Charge integrator | Must be ≫ C_cell so it barely affects the discharge |
| Zener clamp | Protects scope input if the cell flashes over | |
| 1 MΩ bleeder | Drains DC build-up on C_m | |
| Tektronix THS720A | 2-ch scope, XY mode | |

### Divider ratio

$$\frac{V_{HV}}{V_{div}} = \frac{C_{top} + C_{bot}}{C_{top}} \approx \frac{100\text{ nF}}{100\text{ pF}} \approx 1000$$

With the measured 0.18 nF the ratio is closer to ~556. The true ratio must be found by calibration (below). Plan: **oil-immerse C_top** to suppress corona at its leads, which otherwise causes a leakage path and reading error.

### Charge

$$Q(t) = C_m \cdot V_m(t)$$

## Calibration procedure

1. With HV **off**, verify C_top, C_bot, C_m values on an LCR meter; record them.
2. Drive the divider from a known low-voltage sine (function generator at a few volts, near the ZVS frequency) and compare input amplitude vs. CH1 → actual ratio. Capacitive dividers are linear, so a low-voltage calibration holds at kV as long as C_top isn't leaking or coronating.
3. Check frequency flatness of the ratio near the ZVS operating frequency.
4. Record the ratio in the capture metadata; pass it to `lissajous_power.py --divider-ratio`.

## Measurement procedure

1. Follow [safety.md](safety.md). Current-limited supply, hands clear.
2. Set scope: CH1 = divider, CH2 = C_m, both DC-coupled; capture several full cycles.
3. Ramp ZVS input from below strike (~14 V) upward in steps; save a CSV at each step.
4. Run `python analysis/lissajous_power.py capture.csv --divider-ratio <cal> --cm 10e-9`.
5. Plot power vs. input voltage and vs. peak HV.

## What each capture gives

| Quantity | From |
|---|---|
| Discharge power P | Loop area × frequency |
| C_cell | Slope of discharge-off edges |
| C_d (dielectric) | Slope of discharge-on edges |
| C_g (gas gap) | C_d·C_cell / (C_d − C_cell) |
| Burning voltage U_b | From loop geometry / Manley |
| Electrical efficiency | P_discharge / P_input (supply V × I) |

## Status

- ✅ LTspice simulation of the front-end (sine drive, XY plot) — see [`simulation/`](../simulation/)
- ✅ Circuit built
- 🔜 Calibration and first live captures
- 🔜 DBD breakdown element added to the SPICE model
