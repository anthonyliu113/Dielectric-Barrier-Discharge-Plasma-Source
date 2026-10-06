# Safety

This project produces **kilovolt-level AC** and **ozone**. These rules apply to every session, no exceptions.

## Electrical

- **Current-limited bench supply** on the ZVS input. Set the limit before connecting.
- **One-hand rule** near anything energized. Other hand in a pocket or behind your back.
- **Grounded discharge stick** — touch every HV node (including the divider and C_m) before handling. Capacitors hold charge after power-off.
- Power off and **unplug** before any rewiring. Don't trust a switch.
- Keep a **clear zone** around the secondary and electrodes; no conductive tools or jewelry nearby.
- Scope chassis is grounded — only measure through the divider and C_m, never directly on the HV node.
- Zener clamp across C_m protects the scope if the cell flashes over.
- Never operate alone with nobody aware of what you're doing.

## Ozone and NOx

- An air DBD makes **ozone** and nitrogen oxides. Ozone is a lung irritant at low ppm.
- Run with **ventilation / extraction** (fan blowing away from you, ideally out a window or into a fume extractor).
- If you can smell it strongly, it's too much — stop and ventilate.
- Keep run times short until extraction is in place.

## Thermal / mechanical

- MOSFETs, choke, and tank capacitor run hot — check temps during long runs.
- Glass electrodes can crack from thermal stress; inspect before each run.

## UV

- Discharges in air emit some UV. Don't stare at the discharge at close range for long periods.
