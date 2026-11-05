# IWIN Registry Coding Conventions
## Global Standards, Regional Frameworks & Historical Rules
**Version:** 1.3.0 (November 2026)

---

## 1. Core Architectural Shift: The 4-Digit Block 4 Revolution
To eliminate data fragmentation and guarantee long-term system stability, the IWIN standard has expanded **Block 4 (Appellation)** from 3 to **4 numeric characters (0001 to 9999)**.

### 1.1 Decoupling Terroir from Commercial Cuvées
Historically, complex viticultural frameworks (such as Alsace Grands Crus, Burgundy DGCs, or Languedoc/Loire IGP sub-zones) were "virtualized" inside Block 7 (Cuvée / Blend) to avoid saturating Block 4.
* **The New Standard:** With 9,999 structural codes now available per macro-region, **every legally protected geographical sub-zone, DGC, MGA/UGA, or individual Grand Cru must receive its own unique, permanent 4-digit code in Block 4.**
* **Liberation of Block 7:** The 2-character alpha block (**Block 7**) is strictly restored to its original purpose: identifying estate-level liquid configurations (*Tradition, Prestige, Single Barrel* from `AA` to `ZZ`). It is now completely open without any structural character prohibitions.

### 1.2 The "0000" Dynamic Exception (VSIG / Vin de France)
To prevent the central dictionaries from bloating with generic entries, the sequence **`0000`** in Block 4 is reserved as a global runtime keyword.
* **Software Interception:** The `0000` identifier designates a *Vin Sans Indication Géographique* (Wine Without Geographical Indication / *Vin de France*). It is processed dynamically by client-side parsers and **must never be registered** inside physical country files like `FR_appellations.json`.
* **Territorial Anchor Preservation:** The liquid preserves its true geographical regional footprint inside Block 3 (e.g., `FR-2022-BOR-0000-...` for a *Vin de France* crushed and fermented within the Bordeaux territorial perimeter).

---

## 2. France: Regional Adaptations (Block 4 Structural Mapping)

### 2.1 Alsace Viticultural Framework
* **Alsace AOC & Varietals:** Generic Alsace wines receive individual codes in Block 4 depending on regulatory specifications. To handle multi-varietal vs. single-varietal labeling without core system inflation, simple app-level metadata tables handle cépage extraction.
* **The 51 Grands Crus:** The 51 legally protected Grand Cru lieux-dits **no longer share a generic parent code**. Each individual Grand Cru vineyard is assigned its own permanent 4-digit code in Block 4 (e.g., `0002` for *AOC Alsace Grand Cru Rangen*, `0003` for *AOC Alsace Grand Cru Schlossberg*).

### 2.2 Burgundy Viticultural Framework & DGC Resolution
The extreme fragmentation of Burgundy is natively absorbed by the 4-digit layout:
* **Bourgogne DGCs:** Regional designations that append a specific village name receive distinct, consecutive sequential codes inside Block 4.
  * *Example:* `BRG-0001` (Bourgogne Blanc), `BRG-0002` (Bourgogne Chitry), `BRG-0003` (Bourgogne Épineuil).
* **Mâcon Villages:** The 26 specific village designations authorized to append their name to the Mâcon template receive their own 4-digit codes (e.g., `BRG-0112` for Mâcon-Lugny, `BRG-0113` for Mâcon-Verzé).

### 2.3 Languedoc & Loire IGP Sub-Zone De-duplication
* **Departmental & Cultural IGPs:** Localized sub-descriptors and community zones are systematically granted an independent 4-digit structural code in Block 4.
  * *Example (Pays d'Hérault):* `LGD-0208` (Pays d'Hérault - Coteaux du Salagou IGP), `LGD-0209` (Pays d'Hérault - Mont Baudile IGP).
  * *Example (Val de Loire):* `LRE-0281` (Val de Loire - Nièvre IGP), `Val de Loire - Loire-Atlantique IGP` (`LRE-0282`).

## 3. France: Chronological Resolution Rules (Historical Mergers)

To prevent data inflation and avoid maintaining ghost codes for historically absorbed appellations, the IWIN standard uses a **Chronological Resolution Rule** driven by application-level interface logic based on Block 2 (Vintage).

### 3.1 Bordeaux: The Néac / Lalande-de-Pomerol Case (Code `0040`)
The appellation *Néac AOC* was legally absorbed into *Lalande-de-Pomerol AOC* in 1954. No commercial wines have been produced under the Néac name since that date.
* **Database Encoding:** All bottles from this terroir—regardless of age—must strictly use the active 4-digit Appellation code `0040` in Block 4.
* **UI Resolution Rule:** Applications reading an IWIN string containing `BOR-0040` must execute a temporal condition check:
  * If **Block 2 (Vintage) < 1954**, the interface MUST display the historical designation: **"Néac AOC"**.
  * If **Block 2 (Vintage) >= 1954**, the interface MUST display the active designation: **"Lalande-de-Pomerol AOC"**.

### 3.2 Languedoc: Coteaux du Languedoc vs. Languedoc AOC (Code `0197`)
The parent regional appellation *Coteaux du Languedoc AOC* was officially updated and modified by regulatory decree into *Languedoc AOC* in 2007.
* **Database Encoding:** All vintages from this overarching regional footprint strictly map to the persistent numeric 4-digit code `0197`.
* **UI Render Rule:** Applications reading an IWIN string containing `LGD-0197` must apply a temporal branch test based on Block 2 (Vintage):
  * If **Block 2 (Vintage) < 2007**, the interface MUST display: **"Coteaux du Languedoc AOC"**.
  * If **Block 2 (Vintage) >= 2007**, the interface MUST display: **"Languedoc AOC"**.

---

## 4. Logistics and Commercial Variant Coding (The Bloc 8 Composite Rule)
To safeguard the 22-character `IWIN-Core` as a pure liquid-identity pointer, all dimensional and structural material data is localized within the 7-character composite **Block 8**.

### 4.1 Fixed-Width Volume & Material Partitioning
Block 8 enforces an unyielding 7-character continuous width, split into two sub-components:
1. **Positions 1 to 6 (Numeric Volume):** Represents the precise capacity in milliliters (ml) padded with leading zeros up to `999999` (nearly 1,000 Liters).
2. **Position 7 (Alpha Packaging Material Suffix):** Flags the structural physics of the container.
   * `B` = Bottle (Traditional glass flaconnage)
   * `C` = Can (Aluminum cans / Canettes)
   * `X` = Bag-in-Box (Cubitainers / Flexible multi-layer box packaging)
   * `K` = Keg (Service kegs / KeyKegs / Industrial bulk distribution barrels)
   * `T` = Tube (Glass or plastic sample tubes / Vinottes)
   * `P` = Pouch (Flexible modern single-use pouches)

---

## 5. Integrity and Character Alternation Enforcement
To prevent character-shift bugs during OCR scans, no application, dictionary update, or database schema shall modify the width of numeric or alphabetical blocks. Delimiters (`-`) remain mandatory for all user-facing layouts (IWIN-Display) and must be stripped programmatically before cryptographic hashing, local DB caching, or QR Code URL compilation (IWIN-Storage).

