# IWIN Registry Coding Conventions

To maintain a strict 31-character fixed-width structural integrity across diverse global wine laws, the IWIN standard applies specific contextual rules to **Block 7 (Cuvée / Blend)** for certain wine regions. 

This document tracks active regional matrix assignments managed by the IWIN Central Registry.

---

## 1. France: Alsace Viticultural Framework

Because the Alsace regulatory framework heavily relies on single-varietal labeling and specific single-vineyard terroirs (Lieux-dits) rather than heavily splintered geographic appellations, the IWIN standard utilizes standardized **Letter Ranges** inside **Block 7** to preserve schema uniformity.

### 1.1 Alsace AOC (Code `001`) Varietal Allocation Mapping
Wineries and developers must map single-varietal products and traditional regional blends within the `AA` to `ZZ` sequence using the following standardized alpha anchors for the first letter of Block 7:

* **`AA` to `AM`:** Reserved for **Riesling** cuvées (e.g., `AA` for Tradition, `AB` for Vieilles Vignes).
* **`GA` to `GM`:** Reserved for **Gewurztraminer** cuvées.
* **`PA` to `PM`:** Reserved for **Pinot Gris** cuvées.
* **`BA` to `BM`:** Reserved for **Pinot Blanc / Auxerrois** cuvées.
* **`NA` to `NM`:** Reserved for **Pinot Noir** cuvées (Red and Rosé vinifications).
* **`SA` to `SM`:** Reserved for **Sylvaner** cuvées.
* **`MA` to `MM`:** Reserved for **Muscat** cuvées.
* **`EA` to `EM`:** Reserved for Traditional Blends (**Edelzwicker / Gentil**).

*Applications reading an IWIN string containing `ALS-001` will seamlessly resolve the primary grape variety locally by referencing these strict boundary envelopes.*

### 1.2 Alsace Grand Cru AOC (Code `002`) Terroir Resolution
The 51 legally protected Grand Cru lieux-dits do not receive individual 3-digit numeric codes in Block 4. They all share the global parent Appellation code `002`. 

The specific micro-geography of the Grand Cru is mapped entirely via Block 7, combining the vineyard identity and the grape variety. The IWIN Central Registry maintains the sequential allocation from `AA` to `ZZ` for each certified estate (e.g., a producer making a Riesling on the *Rangen* grand cru will be allocated `RA`, while their Gewurztraminer on the same plot will be allocated `RB`).


---

## 2. France: Bordeaux Historical Mergers & Chronological Resolution

To prevent data inflation and avoid maintaining ghost codes for historically absorbed appellations, the IWIN standard uses a **Chronological Resolution Rule** driven by application-level logic based on Block 2 (Vintage).

### 2.1 The Néac / Lalande-de-Pomerol Case (Code `040`)
The appellation *Néac AOC* was legally absorbed into *Lalande-de-Pomerol AOC* in 1954. No commercial wines have been produced under the Néac name since that date.
* **Database Encoding:** All bottles from this terroir—regardless of age—must strictly use the active Appellation code `040`.
* **UI Resolution Rule:** Applications reading an IWIN string containing `BOR-040` must execute a temporal condition check:
  * If **Block 2 (Vintage) < 1954**, the interface MUST display the historical designation: **"Néac AOC"**.
  * If **Block 2 (Vintage) >= 1954**, the interface MUST display the active designation: **"Lalande-de-Pomerol AOC"**.

*This approach preserves structural data consistency for cross-referencing while ensuring absolute authenticity for historical cellar assets.*


---

## 3. France: Burgundy Viticultural Framework & DGC Virtualization

Burgundy is the most fragmented wine region globally. To avoid schema inflation within Block 4 (Appellation), the IWIN standard virtualizes **Dénominations Géographiques Complémentaires (DGC)** (geographic village sub-mentions) by anchoring them to their legal parent generic appellation and partitioning the specific municipality via **Block 7 (Cuvée / Blend)**.

### 3.1 Regional Bourgogne AOC (Code `069`) Village Mapping
All regional designations that append a specific village name (e.g., *Bourgogne Chitry*, *Bourgogne Épineuil*, *Bourgogne Côte d'Or*, *Bourgogne Sainte-Marie-la-Blanche*) strictly use the parent Appellation code `069`. 
The application-level parser resolves the specific village sémantically using a 2-letter alpha code in Block 7 managed by the IWIN Central Registry:
* **`CH`** = Chitry (e.g., `FR-2022-BRG-069-W-100245-CH-00750` resolves locally to *Bourgogne Chitry AOC*).
* **`EP`** = Épineuil.
* **`TO`** = Tonnerre.
* **`CO`** = Côte d'Or.
* **`SM`** = Sainte-Marie-la-Blanche.

### 3.2 Mâcon AOC (Code `112`) Geographic Village Mapping
Similarly, the 26 specific village designations authorized to append their name to the Mâcon template (e.g., *Mâcon-Azé*, *Mâcon-Lugny*, *Mâcon-Verzé*) do not receive individual 3-digit numeric codes in Block 4. They all share the parent Appellation code `112`.
Block 7 isolates the geographical village identity:
* **`AZ`** = Azé (e.g., `FR-2024-BRG-112-W-100452-AZ-00750` resolves locally to *Mâcon-Azé AOC*).
* **`LU`** = Lugny.
* **`VE`** = Verzé.

*This sub-mapping configuration compresses over 60 structural entities into just 2 core Appellation keys, maximizing local database execution speed while preserving micro-terroir precision for the consumer.*
