# Research notes

These notes are just directly from my iPad and converted to make more sense.
(Claude was used to structure the conversion, but they are all my personal notes)


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

## Biology Notes: pH, Buffers and Biomolecules (Nov 15, 2026)

### 1. What pH Measures

pH measures the concentration of hydrogen ions (H⁺) in water, which is directly controlled by how water's internal covalent bonds break and reform.

> pH is the amount of hydrogen ions (H⁺, meaning an electron is missing) and pH is controlled by the water's covalent bonds breaking and coming back.

### 2. The Water Molecule

**Intramolecular covalent bonds:** Each water molecule is held together by polar covalent bonds between the hydrogen and oxygen atoms. The oxygen pulls strongly on the shared electrons, causing the bond to be polar.

**Intermolecular hydrogen bonds:** Different water molecules attract each other via the differences in charge of the water molecule.

- The hydrogen side is more positively charged (δ+) because the electrons are pulled closer by the oxygen.
- The oxygen side is more negatively charged (δ−).
- This unequal sharing of electrons makes the molecule polar. The property is called **electronegativity**.

### 3. How Bond Breaking Creates pH (Autoionization)

Water molecules are constantly bumping into each other because of the hydrogen bonds. Eventually a pull from another water molecule is able to break the bond of another.

```
2 H₂O ⇌ H₃O⁺ + OH⁻
```

**Hydronium (H₃O⁺, often written as H⁺):** Forms when a lone proton (H⁺) bonds with a water molecule, covalently bonding with the H₂O by sharing a pair of electrons from the oxygen. Hydrogen has one proton and one electron, so it needs to gain or lose an electron to become stable. Oxygen has 6 valence electrons, so two more electrons are required to achieve stability.

**Hydroxide (OH⁻):** Forms when a water molecule splits and a hydrogen is separated from the H₂O, giving OH⁻. The oxygen keeps the electron from the lost hydrogen, which makes the OH⁻ charge uneven because there are more electrons than protons.

**In pure water** this happens at a very slow and even rate, leaving an equal amount of OH⁻ and H₃O⁺, making the pH 7, so neutral.

### 4. Why Can You Have a Neutrally Charged Polar Molecule?

> The charge is the totality of the molecule's charge; it says nothing about the areas in which the electrons and protons live.
>
> Polarity cares more about the section the protons and electrons live.
>
> In short, the amount of protons and electrons determine the charge and the area they are in is the polarity.

### 5. How Acids and Bases Shift the Balance

- **Acids (e.g. HCl):** Weak bonds break easily in water, releasing extra H⁺ ions. The H⁺ start to outnumber the OH⁻, which drops the pH below 7.
- **Bases (e.g. NaOH):** They release OH⁻ ions or form strong bonds to soak up free H⁺ ions. OH⁻ outnumber H⁺, causing the pH to rise above 7.

### 6. Strong vs. Weak Acids (Bond Strength)

The strength of an acid or base is all driven from the strength of the covalent bond.

| Type | Example | Bond holding the hydrogen | Effect |
|---|---|---|---|
| Strong acid | HCl | Weak | Bonds break fully in water, leaving free H⁺ protons, so pH decreases a lot |
| Weak acid | Vinegar | Much stronger | Only releases a small amount of H⁺, producing a slightly lower pH |

### 7. Buffers

A buffer is a "chemical absorber" in the sense that in water it is able to resist sudden pH changes when an acid or base is added.

A buffer keeps the number of free H⁺ ions steady by using a reversible chemical bond: soaking up extra H⁺ when it's getting too acidic and releasing H⁺ when it is getting too basic.

Every buffer has two closely related partners:

- **A weak acid (the proton donor):** A molecule holding an H⁺ ion with a moderately strong covalent bond, ready to release it when necessary.
- **Its conjugate base (the proton sponge):** The exact same molecule without the H⁺ ion. It carries a negative charge to absorb a free roaming H⁺.

**How they work together**

- When a strong acid is poured in (a flood of H⁺ ions), the amount of free H⁺ skyrockets. The buffer locks some of these H⁺ into a weak covalent bond, keeping them out of the water.
- When a base is added, it starts taking as many H⁺ ions as possible, which makes the pH increase. The buffer starts releasing more H⁺ to replace the ones that were lost.

### 8. Buffers in the Human Body

Human blood has a pH of **7.4**. A bump to 7.8 or a drop to 6.8 can be fatal. The body's solution is a **bicarbonate buffer**, a partnership between carbonic acid (H₂CO₃) and bicarbonate (HCO₃⁻).

When you exercise, your body releases lactic acid (H⁺) into your bloodstream. The bicarbonate buffer absorbs that instantly so that your blood's pH stays the same / barely moves.

### 9. Buffer Capacity

A buffer isn't infinite. If you pour enough acid to use up every single one of the proton sponges in the water, the buffer hits its **buffer capacity** and the pH will start to decrease.

### 10. The Blood Buffer

```
CO₂ + H₂O ⇌ H₂CO₃ ⇌ H⁺ + HCO₃⁻
```

Carbon dioxide + water ⇌ carbonic acid ⇌ free proton + bicarbonate (the sponge)

The double arrow (⇌) means it can go either direction depending on what your body needs.

**Too acidic?**
The body uses HCO₃⁻ to bind to the extra H⁺, pushing a reaction to make H₂CO₃. Carbonic acid then turns to water and CO₂. Your body detects the rise in CO₂, then makes you breathe deeper and faster to get rid of the excess.

**Too basic?**
The body slows down breathing to trap CO₂ in your blood. That increase in CO₂ combines with water to form carbonic acid, then bicarbonate and a free proton, to neutralize the pH back to 7.4.

### Summary: pH

The pH of a substance is determined by the amount of OH⁻ and H⁺ in the system.

- More H⁺ than OH⁻: **acidic**
- More OH⁻ than H⁺: **basic**
- Even: **neutral** (pH 7)

---

## Biology Notes: The Four Major Biomolecules

Carbohydrates, lipids, proteins and nucleic acids. Together these molecules make up the majority of the cell's mass and perform all structural and functional tasks needed to sustain life.

| Biomolecule | Building block | Main uses | Elements | Examples |
|---|---|---|---|---|
| **Carbohydrates** | Monosaccharides (simple sugars that can't be broken down further) | Quick energy source, short-term energy storage, structural support | Hydrogen, carbon, oxygen | Glucose, starch, glycogen |
| **Lipids** | Fatty acids and glycerol | Long-term energy storage, cell membrane structure, insulation, signaling | Carbon, hydrogen, oxygen, sometimes phosphorus | Fats, steroids, cholesterol |
| **Proteins** | Amino acids | Enzymes, structure, transport, immune defense, movement | Carbon, hydrogen, oxygen, nitrogen, sometimes sulfur | Hemoglobin, antibodies, insulin, collagen |
| **Nucleic acids** | Nucleotides | Storing, transmitting and expressing genetic information; energy transfer | Carbon, hydrogen, oxygen, nitrogen, phosphorus | DNA, RNA, ATP |

### About Them

- **Carbohydrates** are the body's main energy source and provide "immediate" energy to the body using glucose, while complex chains like glycogen store energy for later.
- **Lipids** are hydrophobic. Unlike the other three classes they do not form long chains of repeating monomers. They form the **lipid bilayer** of the cell membrane and store energy.

### The Lipid Bilayer

> The lipid bilayer is a self-assembling, 5-nanometer-thick double layer of amphipathic lipid molecules that form the universal barrier of all cells and organelle membranes.
>
> Key: selectively permeable, lipids diffuse rapidly in the monolayer, it is self healing.

**Amphipathic molecules:** Each lipid molecule (mostly phospholipids) features a hydrophilic head and two hydrophobic, non-polar fatty tails.

They arrange themselves as two leaflets where the hydrophilic heads face outward toward the external liquids and the hydrophobic ends face inward.

```
 O O O O O O O O     <- hydrophilic heads (facing the outside liquid)
 | | | | | | | |     <- hydrophobic tails (facing inward)
 | | | | | | | |
 | | | | | | | |
 O O O O O O O O     <- hydrophilic heads (facing the inside liquid)
```
### How this tie in

### 1. Hemoglobin is a protein, and its charge is the basis of the test

Hemoglobin is made of amino acids. Sickle hemoglobin (HbS) differs from normal hemoglobin (HbA) by one amino acid in the beta chain: a negatively charged, hydrophilic glutamic acid is replaced by a nonpolar, hydrophobic valine.

- The swap removes one negative charge per beta chain, so HbS has a slightly different net charge than HbA.
- In electrophoresis, charge separates the bands: the more negative protein travels faster toward the positive electrode.
- In standard clinical hemoglobin electrophoresis (alkaline buffer, around pH 8.4 to 8.6), HbA runs farther than HbS. This is the separation the photodiode array needs to detect.

### 2. Buffer pH decides whether the separation works

Hemoglobin's isoelectric point is roughly pH 7, where its net charge is about zero.

- Above that pH it is net negative and migrates toward the anode. An alkaline buffer keeps hemoglobin clearly negative and widens the charge gap between HbA and HbS.
- Near pH 7.4 (blood pH) the charge difference is small, so bands are harder to separate.
- This ties to the pH notes: the buffer sets the H⁺/OH⁻ balance, and that balance sets the charge on the protein.

### 3. Buffer capacity matters during a run

Electrophoresis splits water at the electrodes, producing H⁺ at the anode and OH⁻ at the cathode. Over a run, the buffer pH drifts, which shifts band charge and migration speed.

- This is the buffer capacity limit from the notes: once the proton sponges are used up, pH starts to move.
- Design considerations: larger buffer reservoirs, a higher buffer concentration (without raising current too much), and short run times.
- For the dye-based proof-of-concept, the same logic applies. A tracking dye (bromophenol blue is a common one) needs to keep the same charge throughout the run.

### 4. Sample prep uses the lipid bilayer notes

Hemoglobin sits inside red blood cells, behind a lipid bilayer membrane. The cells must be lysed to release hemoglobin into solution (a hemolysate).

- Lysis breaks down the amphipathic membrane, for example with water, a mild detergent, or saponin.
- In a living cell the membrane is self-healing, but lysis is meant to overwhelm that.

### 5. Sample pH and oxygen state matter

Lower pH and low oxygen both promote sickling. Hemoglobin releases oxygen more readily as pH drops (the Bohr effect), and deoxygenated HbS is the form that polymerizes. The bicarbonate buffer keeps blood near 7.4 partly to avoid pushing this process.

### 6. Detection: why a green LED fits

Hemoglobin absorbs strongly in the green range (oxyhemoglobin peaks around 540 and 577 nm), so unstained bands show up as dark regions in green light. This fits the green LED and photodiode array transmission setup.


