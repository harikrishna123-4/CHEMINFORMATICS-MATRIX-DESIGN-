# MOLECULE. — Molecule Explorer + Cheminformatics Virtual Lab

A single-file web app (`Molecule_Explorer.html`) that turns a SMILES string into a 2D drawing, an interactive 3D model, a full property table and a downloadable report. It is a browser re-creation of a Streamlit/RDKit "Cheminformatics Virtual Laboratory" with a Nothing OS-inspired look. No server, no install, no sign-in: open the file and it runs.

## 1. Design

**Visual language (Nothing OS style)**
- Palette: pure black / off-white with exactly one accent, red `#D71921`. Dark and light themes follow the device setting automatically.
- Type: dot-matrix display font (Doto) for titles and big numbers; Space Mono for all body text.
- Surface: dot-grid background, 1px hairline borders, large rounded panels (28px), pill-shaped buttons and inputs, red LED-style highlights.
- 3D colours: atoms are red and blue only (C, H, N blue; O and all other elements red). Bonds are always white. The 3D stage is always black so white bonds stay visible in both themes.

**Layout**
- Phone: single column, scrollable pill tab bar at the top, stats in an auto-fit grid, tables scroll sideways inside their own box.
- Laptop: Explorer uses two columns (left rail of controls, right stage with viewer and readouts), content centred up to 1180px.

## 2. Pages

| Tab | What it does |
|---|---|
| **Home** | What students can do, learning objectives, and a tappable list of all example molecules (tap opens it in the Explorer). |
| **Explorer** | Main workspace: SMILES input, category/search, 2D, 3D, REPORT tabs, stats, Lipinski panel. |
| **Theory** | Short cards: SMILES, formula and molar mass, σ/π bonds, LogP, TPSA, HBD/HBA, Lipinski and Veber, 3D models. |
| **Compare** | Comparison table of molecules with CSV download. |
| **Analysis** | Structure–property scatter plot with automatic observations. |
| **Quiz** | 10-question assessment with score and correct answers. |
| **Final Report** | Personalised student report (name + registration number) with download. |

## 3. Explorer in detail

- **Your own SMILES**: type any SMILES; a live line says "Valid molecular structure detected" or "Invalid SMILES".
- **Library**: 55+ molecules in 6 categories (Medicines, Vitamins, Amino acids, Brain & hormones, Food & flavour, Solvents & basics), searchable by name, plus **Surprise me**.
- **2D tab**: skeletal structure drawn with SmilesDrawer, recoloured for the active theme.
- **3D tab**: geometry built in the browser by a force-model embedder (realistic bond lengths and angles; stereochemistry is not enforced).
  - Styles: **Sticks**, **Ball & Stick**, **Space Fill**, **Wire**.
  - **VDW Surface** toggle adds a translucent van der Waals shell.
  - **Spin** toggle, drag to rotate, scroll/pinch to zoom, **Copy .MOL** exports the coordinates.
- **Report tab**: on-screen report plus downloads (below).
- **Readout cards**: molar mass, LogP, TPSA, H-bond donors/acceptors, rotatable bonds, rings, heavy atoms, total atoms (with H), heavy-atom bonds, σ bonds, π bonds, total bonds (σ+π).
- **Lipinski panel**: four rule lights, score out of 4 and an "oral drug-like" verdict.

## 4. Calculation engine (all in JavaScript)

- Own SMILES parser: branches, ring digits and `%nn`, bracket atoms, charges, aromatic lowercase atoms, Kekulé or aromatic input, implicit hydrogens by valence, aromaticity perception by the 4n+2 rule. Stereo marks are accepted but ignored.
- Formula in Hill order, molar mass, rings, rotatable bonds (amide and ring bonds excluded), HBD/HBA.
- LogP: Wildman–Crippen-style atom and fragment contributions. TPSA: Ertl-style polar-atom contributions. Both are estimates, usually within about 0.5 of RDKit.
- σ/π: every bond is one σ; extra bond orders add π; aromatic systems add one π per two ring atoms that donate a p electron.
- Spot checks against the original Streamlit app: ethanol 46.069 / −0.001 / 20.23, benzene 78.114 / 1.687, phenol 94.113 / 1.392, acetic acid 60.052 / 0.091 / 37.30, aspirin 180.159 / 1.31 / 63.6 (MW / LogP / TPSA).

## 5. Compare, Analysis, Quiz, Final Report

- **Compare**: starts with Ethanol, Benzene, Phenol, Acetic acid, Aspirin. Columns: MW, LogP, TPSA, HBD, HBA, RotB, Rings. Add the molecule currently open in the Explorer, remove rows with ✕, clear all, download or copy the CSV.
- **Analysis**: choose any two properties (molar mass, LogP, TPSA, HBD, HBA, rotatable bonds, rings, heavy atoms). Draws a scatter plot of the comparison table and reports Pearson correlation (strength and sign), highest/lowest values and the mean LogP with vs without H-bond donors.
- **Quiz**: SMILES basics, ethanol/benzene strings, parentheses, π bonds in CO₂, LogP, TPSA, Lipinski limit, σ bonds in ethanol. Submit shows score, marks right/wrong and reveals correct answers; Reset clears.
- **Final Report**: enter name and registration number, then Generate. Contains student block, molecule identity, 2D structure, 3D snapshot, atom and bond counts, properties, Lipinski and Veber results and an automatic interpretation.

## 6. Downloads

- Explorer REPORT tab: HTML report, Markdown, CSV, JSON, structure SVG, 3D MOL (`.mol.txt`), and the whole library as CSV.
- Compare: comparison CSV. Final Report: HTML, Markdown, CSV.
- When the host allows file saving, each download asks for confirmation. When it does not, the buttons switch to **COPY** and copy the file text to the clipboard.

## 7. How to use (30 seconds)

1. Open **Explorer**, pick a molecule or type a SMILES (e.g. `CC(=O)Oc1ccccc1C(=O)O`).
2. Check 2D, then 3D; try the styles and VDW surface.
3. Read the stats and Lipinski panel.
4. Go to **Compare**, add it next to others, open **Analysis** for the plot.
5. Take the **Quiz**, then fill in **Final Report** and download it.

## 8. Technical notes and limits

- One HTML file; libraries loaded from CDNs (SmilesDrawer, 3Dmol.js, Google Fonts), so an internet connection is needed for drawings, 3D and fonts.
- Supported elements: H, B, C, N, O, F, Si, P, S, Cl, Br, I, Se, Li, Na, K, Mg, Ca, Fe, Zn, Cu.
- Estimated LogP/TPSA and the force-model 3D shapes are for learning and screening, not research or medical use. RDKit is not used.
- Data is not saved between sessions; the Compare table resets on reload.

## Files

- `Molecule_Explorer.html` — final app (all features, red/blue/white 3D).
- `cheminformatics-lab.html` — earlier standalone lab version.
- `molecule-explorer.html` — first 3D molecule viewer version.
