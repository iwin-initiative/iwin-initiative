# IWIN STANDARD — BLOCKS 4 AND 7 TECHNICAL GOVERNANCE
*Architecture, Nomenclature, and Governance Rules for Collective Appellations (Block 4) and Producer Labels & Micro-Parcels (Block 7)*

---

## 1. Core Philosophy & Separation of Powers

The IWIN standard relies on a strict boundary between **collective public legality** (shared by multiple operators under a public specifications registry) and **private or plot-specific initiative** (individual estate choices, single-vineyard holdings, or commercial cuvées).

* **Block 4 (Collective Appellations & Sub-Appellations):** Manages the macro- and meso-geographical regulatory infrastructure (Appellations, collective sub-zones, and regional tiers). It is a national dictionary, immutable per vintage.
* **Block 7 (Producer Labels & Parcels):** Manages micro-localizations (closed single-vineyard *Vignas*, *Climats*), commercial cuvées, and specific aging methods or estate tributes.

---

## 2. Block 4: Collective Appellations & Terroirs

### 2.1 File Location, Formatting, and Keys
* **File Structure:** `/registries/countries/[COUNTRY_CODE]/[COUNTRY_CODE]_appellations.json`
* **Format:** Fixed 4-digit sequential numeric keys (`0001` to `9999`).
* **Sequence Law:** Once an appellation code is registered (e.g., `0424` for Ajaccio), it is **immutable**. Codes are never re-ordered, modified, or deleted.
* **The "0000" Dynamic Reserved Keyword:** The sequence `0000` is an application-level reserved keyword for Wines Without Geographical Indication (*VSIG* / *Wines Without Geographical Indication*). It is resolved dynamically by software parsers and **must never be manually entered into physical JSON registry files**.

### 2.2 Inter-Regional Appellations & Cross-Border Designations
* **Multi-Region Array Support:** For broad geographical indications, multi-state, or regional designations that span across multiple administrative or viticultural macro-regions (e.g., cross-border or multi-regional IGPs/AVAs), the `macro_region` attribute accepts an array of strings containing two or more valid trigrams instead of a single string.
* **Ordering Convention:** As a best practice, list the primary or historical home region first in the array.
* **Example Syntax:**
```json
"0841": {"macro_region": ["BOR", "SUD"], "name": "Atlantique (IGP)", "type": "IGP", "allowed_colors": ["R", "W", "P"]},
"0156": {"macro_region": ["MID", "SET"], "name": "Ozark Mountain (AVA)", "type": "AVA", "allowed_colors": ["R", "W"]}
```

### 2.3 Global Classification & Institutional Terminology Matrix
National registries must prioritize **the official legal and historical designations traditionally printed on physical wine labels** rather than enforcing a uniform supranational institutional acronym (such as the European AOP). In federal or multilingual states (e.g., Belgium), local designations must reflect the official language and legal terminology of the specific production zone.

| Country | Base Appellation Tiers (Institutional & Local) | Collective Sub-Zone Level (UGA / Sub-appellation / DGC) |
| :--- | :--- | :--- |
| **EU** | AOP / IGP (Supranational framework, superseded locally by tradition) | — |
| **FR** | AOC Grand Cru, AOC Regional, AOC Sparkling, IGP Regional | DGC (Dénomination Géographique Complémentaire), Lieu-dit, Sub-zone |
| **BE** | AOP (Wallonia), IGP (Wallonia), BOB (Flanders), BGA (Flanders) | Communal / Regional sub-designations |
| **IT** | DOCG, DOC, IGT | UGA, Sottozona, MGA (collective), Rive, Contrada |
| **US** | AVA, IGP / Wine | Sub-AVA |

### 2.4 Syntax Requirement for Block 4 Names
All local registry names (`name`) follow a clean syntax structuring the relationship between the base appellation and its collective sub-zones:
* **Base Appellation:** `"Appellation Name (BASE_TYPE)"` (e.g., *Champagne (AOC)*, *Rioja (DOCa)*)
* **Sub-Zone / UGA / DGC:** `"Parent Appellation (BASE_TYPE) - Sub-Zone Name (SUB_TYPE)"` 
  *(Example: `"Montepulciano d'Abruzzo (DOC) - Terre dei Vestini (UGA)"`)*
  *(Note: The inclusion of type indicators in parentheses serves as a structured aid for human inspection and machine parsing).*

### 2.5 Chronological Resolution & Historical Status Mergers
To prevent data inflation and avoid maintaining ghost codes for historically absorbed appellations (e.g., *Néac* into *Lalande-de-Pomerol*, or *Terre Tollesi* into *Tullum DOCG*), IWIN applies a **Chronological Resolution Rule** driven by application-level logic based on Block 2 (Vintage):
* **Database Encoding:** All bottles from a historic terroir use a single, unified permanent key.
* **UI Resolution Rule:** Applications parse the vintage against the official date of regulatory mutation:
  * If **Vintage < Mutation Date**, the interface displays the historical designation (e.g., *Néac AOC* or *Terre Tollesi DOC*).
  * If **Vintage >= Mutation Date**, the interface displays the active current designation (e.g., *Lalande-de-Pomerol AOC* or *Tullum DOCG*).

---

## 3. Block 7: Cuvée, Estate Labels, and Parcels

Block 7 provides 2 uppercase alphabetical characters (`AA` to `ZZ`), granting **676 unique combinations** per producer and vintage to identify distinct commercial labels and micro-parcels.

### 3.1 Strict Structural Rules (Mandatory)
1. **Format Constraint:** Exactly 2 uppercase letters (`A-Z`). Numerals, lowercase, and special characters are **strictly forbidden**.
2. **Inter-Vintage Stability:** The 2-letter code assigned to a specific wine label must remain identical across vintages. Year distinctions are handled entirely by Block 2.

### 3.2 Producer Freedom & Professional Recommendations (Advisory)
Winemakers and estate managers retain **total freedom** in choosing their 2-letter codes. However, IWIN provides advisory guidelines and professional best practices to structure the 676 combinations rationally:

* **Mnemonic Best Practices (English & French Examples):**
  * *Generic / Flagship & Estate Labels:* `MW` (*Main Wine*), `OV` (*Old Vines*), `AM` (*Aunt Margaret*), `GV` (*Grand Vin*), `VV` (*Vieilles Vignes*), `GM` (*Grand-Mère / Family Tribute*).
  * *Parcels & Single Vineyards:* `HI` (*Parcel on the Hill*), `CR` (*Parcel by the Creek*), `OB` (*Oyster Bay Hamlet*), or distinct consonantal pairs (e.g., `UN` for *Lieu-dit Untel*, `CN` for *Climat / Cuvée*).
* **Advanced Structural Strategy (Optional Professional Framework):**
  To use the 676 combinations in an ultra-rational manner, cellars may optionally structure the two-letter pair as follows:
  * **1st Letter (Origin / Plot Nature):** `A` for zone blends / generic cuvées; `B`, `C`, `D`... for exclusive *Vignes*, single vineyards, or cadastral plots.
  * **2nd Letter (Aging Style / Vinification):** `A` for *Annata* (classic/young), `R` for *Riserva* (extended aging), `S` for *Superiore* (selected selection).

