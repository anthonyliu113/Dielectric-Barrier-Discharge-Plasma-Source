# Theory

## What a DBD is

A dielectric barrier discharge is a gas discharge between two electrodes where **at least one electrode is covered by an insulator** (here: glass). Because the barrier blocks DC current, the source must be AC. Each half-cycle:

1. The applied voltage rises until the gas gap breaks down.
2. Charge flows across the gap and **accumulates on the dielectric surface**.
3. That surface charge builds an opposing field, which **self-extinguishes** the discharge within nanoseconds — before it can thermalize into an arc.
4. When the polarity reverses, the stored surface charge *adds* to the applied field, so the next breakdown happens earlier.

The result is a cold, non-equilibrium plasma: hot electrons (several eV), near-room-temperature gas. That's why DBDs are used for ozone generation, surface activation, and plasma ashing without melting the workpiece.

## Paschen's law

Breakdown voltage of a uniform gap depends on the product of pressure *p* and gap *d*:

$$V_b = \frac{B\,pd}{\ln(A\,pd) - \ln\!\left[\ln\!\left(1 + \frac{1}{\gamma_{se}}\right)\right]}$$

- *A*, *B* — gas constants (ionization), *γ* — secondary electron emission coefficient of the cathode
- At atmospheric pressure and mm-scale gaps we sit on the **right side** of the Paschen minimum → breakdown in the kV range.
- In the later vacuum chamber (~1 Torr, ~20 cm gap) the same equation tells us where the glow discharge will strike.

`analysis/paschen.py` plots this for air, N₂, Ar, and He.

## Equivalent circuit

```
          ┌──── C_d ────┬──── C_g ────┐
  V(t) ───┤  dielectric │   gas gap   ├─── GND
          │             └─ plasma ────┘
```

- **Discharge off:** the cell is two capacitors in series → C_cell = C_d·C_g / (C_d + C_g).
- **Discharge on:** the gas gap is held at roughly the **burning voltage** U_b, so the external circuit sees only C_d.

## Why the Q-V Lissajous is a parallelogram

Put a large capacitor C_m in series with the cell. Its voltage is proportional to the charge transferred: **Q = C_m · V_m**. Plot Q against applied voltage V:

- Two edges have slope **C_cell** (discharge off)
- Two edges have slope **C_d** (discharge on)

The enclosed area is the energy dissipated per cycle:

$$E = \oint V\,dQ \qquad P = f \cdot E$$

For an ideal parallelogram this reduces to **Manley's equation**:

$$P = 4 f\, C_d\, U_b \left( V_{pk} - U_b\,\frac{C_d + C_g}{C_d} \right)$$

Measuring the slopes gives C_d and C_cell, from which C_g and U_b follow — so one scope capture yields power, both capacitances, and the burning voltage.

## Why it matters for semiconductor work

Atmospheric DBDs are used for surface activation and cleaning. Actual IC etch uses **low-pressure** plasmas (CCP-RIE, ICP-RIE, ECR). The physics vocabulary (breakdown, sheath, reactive species, power coupling) and the diagnostics (electrical power measurement, OES) carry over — the hardware does not. That's the reason this project continues into a vacuum chamber.

## References

- U. Kogelschatz, "Dielectric-barrier discharges: their history, discharge physics, and industrial applications," *Plasma Chem. Plasma Process.* 23 (2003)
- T. C. Manley, "The electric characteristics of the ozonator discharge," *Trans. Electrochem. Soc.* 84 (1943)
- A. Fridman, *Plasma Chemistry*, Cambridge University Press (2008)
- M. A. Lieberman & A. J. Lichtenberg, *Principles of Plasma Discharges and Materials Processing*, 2nd ed., Wiley (2005)
