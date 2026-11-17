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
| Voltage | 120 V |
| Field E = V / d | 17.14 V/cm |
| Velocity | about  0.004285 cm/s |
| Time to cross the gap | about 1633,605 s, roughly 27.4 min |

With a 120 V supply, E is about 17.14 V/cm, and the velocity drops to about  0.004285  cm/s, so crossing the whole gap would take around 27.4 Min To keep the.

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
### How this ties in

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

# Sickle Cell Research Notes

*Compiled from handwritten notes. The green-light/heme section was written November 16, 2026. Pages were reordered by topic.*

---

## 1. Hemoglobin Variants

There are four main variants of hemoglobin. All four involve a change to the glutamic acid in the **beta chain**.

| Variant | Name | Change | Most common in | Effect |
|---|---|---|---|---|
| **HbS** | Sickle cell hemoglobin | Glutamic acid → **valine** at position 6 | (malaria regions) | The change allows polymerization inside the red blood cell |
| **HbC** | Hemoglobin C | Glutamic acid → **lysine** at position 6 (same spot as HbS) | West Africa | Reduced solubility, so hemoglobin forms rigid hexagonal crystals inside red blood cells. Two copies cause mild chronic hemolytic anemia |
| **HbD** | Hemoglobin D (Punjab / Los Angeles) | Glutamic acid → **glutamine** at position 121 | Northwest India and Pakistan | Harmless, no symptoms. But inheriting HbD from one parent and HbS from the other gives **HbSD**, which can cause severe sickling symptoms similar to HbS |
| **HbE** | Hemoglobin E | Glutamic acid → **lysine** at position 26 | Southeast Asia | Hemoglobin is slightly less stable, and the body produces it more slowly. One or two copies cause no symptoms, usually just mild anemia |

> **Hemolytic anemia:** a type of anemia where the red blood cells are destroyed and removed from the blood stream faster than the bone marrow can replace them.

### Why do they all swap out glutamic acid?

**Location (survivability)**
- Glutamic acid is a negatively charged, hydrophilic amino acid, so evolution put it on the *outside* of the protein to keep it smoothly dissolved in blood.
- Mutations on the outside tend to leave the oxygen-carrying inside intact.
- A mutation on the *inside* could damage the oxygen-carrying part, which would be fatal for the cell, so the trait wouldn't get passed on.

**Malaria protection (natural selection)**
- Having a copy of HbS, HbC, or HbE gives people strong natural protection against malaria.
- Malaria is a major killer in tropical regions like Sub-Saharan Africa and South Asia, so people carrying these traits lived long enough to pass them on.

**Easy detection (why they were found first)**
- Glutamic acid carries a strong negative charge (−1). When it is swapped for a neutral or positively charged variant, the overall charge changes drastically.
- This is what allows electrophoresis to actually work.

---

## 2. Background: Polymerization

**Polymerization** is the process in which thousands of small, simple molecules link together end-to-end to form a giant chain-like molecule (Power Rangers style).

**Building blocks**
- **Monomers:** the small molecules you start with. They tend to be gases or thin liquids.
- **Polymers:** a giant chain of monomers locked together. Because the chains get so large and tangled with one another, they form solid, flexible, or stretchy materials (plastic, rubber, or deoxyribonucleic acid).

### Two main polymer types

**Addition polymerization**
- Monomers have double chemical bonds that "open up" (like holding hands), which allows them to bond without losing any atoms.
- Common with plastics like polyethylene (plastic bags and bottles) and PVC (plumbing pipes).
- No by-products, meaning no atoms were lost in the process.
- Example drawn: C=C with four H's (polyethylene).

**Condensation polymerization**
- Two different monomers join up, and every time they link up they kick out a tiny leftover molecule, usually water.
- This is how nylon, polyester, and even proteins in your body are built.
- Example drawn: a dicarboxylic acid monomer (HO–C(=O)–[R]–C(=O)–OH) bonding with a diol (HO–[R]–OH). An H and an OH are released and form **H₂O**.

**Summary**
- Polymerization occurs when monomers start binding to one another and form polymers.
- *Addition:* two like monomers bond with one another, leaving no by-product.
- *Condensation:* two unlike monomers bond, one leaving H and the other leaving OH. They get rid of the H and OH to bond with each other, and the H and OH form H₂O.

---

## 3. How HbS Behaves

### Deoxygenation and oxygenation (HbS)
- In sickle cell hemoglobin (HbS), **oxygenation keeps proteins flexibly apart**, while **deoxygenation** causes a slight shape change that makes HbS molecules stick to one another and polymerize into rigid fibers.

### HbA vs HbS
- Normally, adult hemoglobin (HbA) is smooth with a water-friendly outermost shell. The molecules also bounce off one another in a safe manner.
- With HbS, that outer shell becomes hydrophobic and sticky, sticking onto other cells.
- When **oxygenated**, HbS is exactly the same as HbA. The oxygen pulls on the iron in heme, which changes the molecule into its **R-state (Relaxed State)**. This hides the sticky valine part on the outside of the molecule, so it works as intended.
- When the oxygen is dropped off at its intended location, the hemoglobin changes as the iron moves back into place, changing it into its **T-state (Tense State)**. This wider, open shape shows the once-hidden pocket of valine.
- That exposed valine pocket connects with other pockets of valine on other molecules. This starts a chain causing rapid polymerization.
- The result is thousands of HbS molecules linking together and forming long, stiff, rod-like cables.
- This changes the red blood cell from a soft disk to a hard crescent (sickle) shape that can jam small blood vessels.

### The reversible cycle
- If the sickled blood gets back to the lungs in time to get oxygenated, the fibers start to separate from one another.
- But after doing this too many times, the cell can become **permanently damaged**.

### In summary
- Oxygenated HbS behaves like HbA. When it is not oxygenated, the shape changes and valine molecules inside the cell start bonding to one another and stretch out the cell, causing it to sickle.
- This happening to thousands of cells can cause serious harm to the body.

---

## 4. Measuring Blood Oxygen

When oxygen binds to the iron in heme, the **molar absorptivity** (how greedy it is for light) drops sharply. Blasting red and infrared light and using the Beer-Lambert Law (BLL) allows you to determine the percentage of hemoglobin carrying oxygen.

### Beer-Lambert Law (BLL)

The amount of light absorbed by a liquid depends directly on how concentrated the light-absorbing substance is and how thick the liquid is.

**A = ε · c · L**

| Symbol | Meaning |
|---|---|
| **A** | Absorbance: how much light gets trapped |
| **ε** | Molar absorptivity: how naturally greedy a specific molecule is for a certain colour of light |
| **c** | Concentration: how many of these greedy molecules are packed into an area |
| **L** | Path length (thickness) |

- **Concentration example:** add powdered juice to a glass of water. The water gets darker, which absorbs more light.
- **Path length example:** pour that same juice into a wider glass and the absorption of light increases.

**Summary:** concentration, thickness, and molar absorptivity determine how much light gets trapped in a substance. In hemoglobin, the heme gets a sharp decrease in molar absorptivity when oxygen is introduced. You can shine red and infrared light at blood to determine blood oxygen levels. Alternatively, you could shine **green light to determine BPM**.

### Heart rate with green light
- Green light is absorbed so well because hemoglobin has a massive ε (molar absorptivity) for green light. It is really greedy for green photons.
- This is good for us because you only need a tiny amount of blood to absorb a huge, easy amount of green light.
- Every heartbeat gives fresh oxygenated blood to the body. Looking at a single blood vessel, the concentration (c) and thickness (L) would increase. BLL says absorbance (A) would increase, meaning more green light gets absorbed and less bounces back.
- By counting these dips and peaks, you are able to count heartbeats.

---

## 5. Why Green LEDs / Light?  

I'm using green LEDs because hemoglobin absorbs it really well, because of something in it called **heme**.

### Heme
- Heme is an iron-containing molecule mostly found in hemoglobin. It has the ability to carry oxygen to your cells so they can produce energy.
- It is shaped like a **doughnut or ring with an iron in the middle**.
- The structure acts as a protective shell. The central, positively charged iron ion is great at grabbing gases and electrons moving around.
- The structure allows electrons to move around freely around the molecule. The size of the (planar) ring determines how much energy the electrons inside it can hold.

### Electronic transitions (Soret and Q-bands)
- The electrons on the planar ring are able to jump to higher energy levels. To make one of these jumps, the electron must grab energy from a traveling photon with the exact energy level needed.
- The iron in the middle "fine tunes" the ring so that the energy rings need **blue-violet and green light** to get to the next level.
- Red light doesn't have the right energy to cause a jump, so it reflects and bounces to your eyes. This is why blood looks red.
- **Soret band:** a big jump that traps violet and blue light.
- **Q-bands:** smaller jumps that trap green and yellow light.

### Role of the iron atom
- The iron ion in the centre pulls on the electrons, and reacting to the molecule's small vibrations it "fine tunes" those energy rings so that green light is absorbed while the red light passes through and reaches your eyes.

**Side note:** when oxygen binds to heme it physically pulls the iron to the centre of the ring, which changes the tuning to absorb less red light. More red light bounces back, making it brighter and redder.

**Side side note:** the iron isn't exactly in the centre, so when oxygen is introduced it pulls on the iron's electrons, allowing it to fit perfectly in the middle. This allows almost no red light to get absorbed.

### Green light summary
The reason hemoglobin absorbs green light so well is because of a molecule inside of it called heme. Heme has an iron ion at its centre, surrounded by a ring of nitrogens and carbons, and in this ring electrons are able to move freely.


