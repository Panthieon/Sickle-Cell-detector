# Sickle Cell Detector

A low-cost, point-of-care (POC) device that screens for sickle cell disease by separating hemoglobin variants with electrophoresis and reading the result optically. A user places a drop of blood in the device, and it reports a result on a colour display.

**Status:** early prototype. Components are gathered, the PCB schematic is in progress, and the mechanism has **not yet been validated** against known samples. Everything below is completely design focused and is not fact.

## Why I made it

I'd realised that poorer countries are the most exploited and most used for resources and things along that vain and still aren't treated as the people that they are. This device isn't necessarily going to change all the pain and inhumane treatment that they get but I hope that just one more life can be saved because of it. Just one person being helped because of something I made in my room will hopefully show other people that helping other humans is actually worthwhile.

## How it works

1. **Separation.** A blood sample is loaded onto a strip wetted with buffer, between two electrodes. A current pulls the hemoglobin along the strip. Different variants (HbA, HbS, HbC) move at different speeds, so they form separate bands.
2. **Detection.** A light-tight enclosure holds green LEDs (540 nm) above a 3-point photodiode array. Hemoglobin absorbs green light, so a band passing under a photodiode lowers the light it sees.
3. **Decision.** A Raspberry Pi Pico reads the three channels, turns each into "clear" (high light) or "drop" (low light), and looks up the 3-bit pattern in a logic table.
4. **Output.** A colour display shows whether the test detected sickle cell, did not, or was inconclusive.

### Migration estimate

V.0.2
| Quantity | Formula | Value |
|---|---|---|
| Electric field | E = V / d | 90 V / 7 cm ≈ 12.8 V/cm |
| Mobility (Hb, alkaline buffer, estimated) | μ | ≈ 2.5 × 10⁻⁴ cm²/(V·s) |
| Velocity | v = μ × E | ≈ 0.0032 cm/s |
| Run time | t = d / v | ≈ 2,190 s ≈ 36 min |

The tray is 7cm long and I will increase the voltage so the time decreases.

V.0.3
| Quantity | Formula | Value |
|---|---|---|
| Electric field | E = V / d | 90 V / 7 cm ≈ 12.8 V/cm |
| Mobility (Hb, Acid buffer, estimated) | μ | ≈ 2.5 × 10⁻⁴ cm²/(V·s) |
| Velocity | v = μ × E | ≈ 0.0028 cm/s |
| Run time | t = d / v | ≈ 2,840 s ≈ 47 min |

 I increased the length of the track to 8cm

 V.0.4 (current)
 | Quantity | Formula | Value |
|---|---|---|
| Electric field | E = V / d | 120V / 7 cm ≈ 17,14 V/cm |
| Mobility (Hb, Acid buffer, estimated) | μ | ≈ 2.5 × 10⁻⁴ cm²/(V·s) |
| Velocity | v = μ × E | ≈ 0.004285 cm/s |
| Run time | t = d / v | ≈ 1633.605 s ≈ 27.2 min |

These are estimates from literature ranges for μ. Real run times will be measured during testing.

### Logic table (draft)

Gate 1 to 3 are the three photodiodes. **Clear** = high light, **Drop** = low light.

| Gate 1| Gate 2|Gate 3| Meaning                     |Code |

| Clear | Clear | Drop | Normal (HbAA)               | 110 |
| Drop  | Clear | Clear| Sickle cell (HbSS)          | 011 |
| Drop  | Clear | Drop | Sickle trait (HbAS)         | 010 |
| Drop  | Drop  | Clear| HbSC / trait                | 001 |
| Clear | Drop  | Clear| Incomplete test             | 101 |
| Clear | Drop  | Drop | Slow separation             | 100 |
| Clear | Clear | Clear| No sample / blank           | 111 |
| Drop  | Drop  | Drop | Smear / overload            | 000 |

## Hardware

- Raspberry Pi Pico (MicroPython)
- 540 nm green LED array
- 3-channel photodiode array
- Custom PCB (EasyEDA) for amplification and signal conditioning
- Electrophoresis chamber with electrodes and a sample slot
- Colour display and power switch
- Light-tight enclosure (about 17 × 14 × 10 cm) designed in OnShape
- Power: external bench supply for testing. Battery power comes after the core mechanism is validated.


## 3D models

- [V.0.1 (OnShape)](https://cad.onshape.com/documents/ac5d915b7dd56fe7887c1290/w/e9d363a77695b765c791623f/e/d9d70f5a69e87de0878bb7b5?renderMode=0&uiState=6ae7c165631afd0008e3419c): encloses the main parts of the device
  
- [V.0.2 (OnShape)](https://cad.onshape.com/documents/049959b67cfd6dc5f4ab098d/w/375048fa51df30f6dd2c9980/e/a6782179d54698f48a207f64?renderMode=0&uiState=6aea72344f4c2c1c2baf14a5): 

- [V.0.3 (OnShape)](https://cad.onshape.com/documents/633eec5867b5b4bc40dc1a9c/w/6806ada02e6d3b0567c96b86/e/820ddc78e995182a8570b57b?renderMode=0&uiState=6aefac22679dc84116f17ebd): 
  
- [V.0.4 (OnShape)](https://cad.onshape.com/documents/47ab40eb762feee9ffd6612a/w/5b9899adb3dfb9fb330092ec/e/f19f73eaa2c35cfabcb71556?renderMode=0&uiState=6af0e8059aba66657643dbe8): newest prototype

## Roadmap

- [x] Concept, layout and logic table
- [x] Enclosure models (V0.1, V0.2)
- [ ] PCB schematic and layout
- [x] Assemble the electrophoresis chamber
- [x] Firmware for reading the photodiodes and the logic lookup
- [ ] Validate against known HbAA, HbAS and HbSS samples (with a mentor or lab)
- [x] Battery power and a final enclosure

## Limitations

- This is a screening concept, **not a medical device**, and it must not be used to diagnose anyone.
- Results need validation against a reference method (for example, lab electrophoresis or HPLC) before they mean anything.
- Blood handling needs proper safety practice. Prototypes should be tested with approved control samples (not real blood).


