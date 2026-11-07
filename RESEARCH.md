# Research notes

Background science, design decisions and open questions for the Sickle Cell Detector. This is a living document. Items marked **(to verify)** are my own estimates or things I still need a source for.

## 1. The biology

- **HbS** (sickle hemoglobin) comes from a single substitution in the beta-globin chain at position 6: glutamic acid is replaced by valine. **HbC** has lysine at the same position.
- Glutamic acid carries a negative charge. Valine and lysine do not, so HbS and HbC have a different net charge from normal adult hemoglobin (HbA). That charge difference is what electrophoresis uses.
- A person with one HbA gene and one HbS gene has sickle cell trait (HbAS). A person with two HbS genes has sickle cell disease (HbSS).

## 2. How hemoglobin electrophoresis works

- Hemoglobin molecules move in an electric field at a speed set by their net charge at a fixed buffer pH.
- In the standard clinical method, a hemolysate (red cells broken open to release hemoglobin) is run on cellulose acetate in an **alkaline buffer, pH about 8.4 to 8.6**. At that pH hemoglobin is negatively charged and moves from the cathode (-) toward the anode (+).
- On cellulose acetate at alkaline pH, the bands appear in this order from the starting point (slowest) to fastest: **C, S, F, A**. HbA runs fastest.
- Result patterns:
  - Normal (HbAA): one main HbA band.
  - Sickle trait (HbAS): an HbA band and an HbS band.
  - Sickle disease (HbSS): an HbS band, with no HbA.

## 3. Limits of the alkaline method (important for my design)

- Several variants **co-migrate** on alkaline cellulose acetate. HbS, HbD and HbG run to the same place, and HbA2, HbC, HbE and HbO run together.
- Labs therefore confirm a suspected HbS band with a second test, such as **citrate agar electrophoresis at acid pH (about 6.2)** or a solubility/sickling test.
- **Design consequence:** my device can only produce a *presumptive* screening result. It cannot rule out HbD or HbG, and a positive would still need lab confirmation.

## 4. My approach

- **Separation:** a strip of cellulose acetate wetted with buffer, its ends dipping into two buffer wells with electrodes, run at a controlled voltage.
- **Detection:** green LEDs (540 nm) shine through the strip onto a 3-point photodiode array. A hemoglobin band absorbs green light, so the light on a photodiode dips when a band passes over it. (Oxyhemoglobin absorbs in the green region, around 540 and 577 nm **(to verify)**.)
- **Decision:** a Raspberry Pi Pico reads the three channels and matches the pattern against a logic table. A display shows the result.

### Prototype plan (fair, December 9)

1. Prove separation with a red food dye on the strip.
2. Prove detection with a dye band passing the photodiodes.
3. Combine them and run the logic automatically.
4. Move to real hemoglobin control samples with help from a lab.

## 5. Migration math

Using v = mu x E and t = d / v, with the gap between the buffer wells as d.

| Quantity | Value |
|---|---|
| Electrode gap (d) | 8 cm |
| Mobility (mu), estimated | about 2.5 x 10^-4 cm^2/(V s) **(to verify)** |
| Voltage | 90 V |
| Field E = V / d | 11.25 V/cm |
| Velocity | about 0.0028 cm/s |
| Time to cross the gap | about 2,840 s, roughly 47 min |

With a 30 V supply, E is about 3.75 V/cm, and the velocity drops to about 0.00094 cm/s, so crossing the whole gap would take around 2.4 hours. To keep the demo short:

- Place the three photodiodes close to the start line (first 3 to 4 cm), and/or
- Measure the real dye speed on a bench test and rebuild the numbers from that.

The mobility value is an estimate. I will replace it with a measured value.

## 6. Materials decisions

| Item | Choice | Notes |
|---|---|---|
| Strip | Cellulose acetate electrophoresis membrane, 8 x 12 cm sheets | Cut into strips about 1.5 to 2 cm wide and 10 cm long |
| Backup strip | Chromatography paper | For the dye demo only |
| Buffer (hemoglobin) | Tris-borate-EDTA (TBE), pH about 8.4 | Matches clinical practice |
| Buffer (dye demo) | TBE, or dilute sodium bicarbonate as a fallback | |
| Wells | About 3 cm long, 4 cm wide, 2 cm deep, filled to about 1 cm | Equal levels in both wells |
| Power | Bench supply, 0 to 30 V with current limit | Electrodes fully enclosed |
| Enclosure | 14.8 x 5.6 cm, hinged lid, black, light-sealed | With a lid interlock |

Sodium citrate buffer (pH about 6) belongs with citrate agar as a *confirmation* method, not as the main buffer for cellulose acetate.

## 7. Open problems

1. **Sample preparation.** The clinical method uses a hemolysate, so whole blood must be lysed before it is loaded. I do not yet have a lysis step.
2. **Sensitivity.** Hemoglobin bands from a tiny sample may be faint. I need to measure the photodiode signal against noise.
3. **Co-migration.** HbS and HbD/G run together, so the result is presumptive only (see section 3).
4. **Controls.** I need approved control samples (HbAA, HbAS, HbSS) to validate the device. I will not use my own blood.
5. **Temperature.** Current heats the strip and changes migration speed. I need to check heating at my voltage.

## 8. Questions for mentors

- Which buffer, strip and strength would you use for a miniaturized hemoglobin run?
- How would you lyse whole blood inside a point-of-care device?
- What control samples should I use, and where can I get them?
- What would convince you the readout is valid?
- Is there anything in the design that will not work?

## 9. Experiment log

| Date | Test | Strip | Buffer | Voltage | Result |
|---|---|---|---|---|---|
| | Dye migration test | | | | |
| | Photodiode baseline | | | | |
| | Combined run | | | | |

