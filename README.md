# IWIN: International Wine Identification Number
## Technical Specification & Architecture Whitepaper

**Author:** IWIN Initiative  
**Status:** Canonical Standard Specification  
**Version:** 1.0.0 (November 2026)  
**Licensing:** Dual-Licensing (Open-Source GNU GPLv3 / Commercial)

---

### 1. Abstract
The digital wine ecosystem currently lacks a decentralized, human-readable, and semantic data standard. Existing identification methods rely on arbitrary sequential database keys that require constant API queries to resolve fundamental product attributes like country, appellation, vintage, or color. This architecture introduces high friction for cellar management software, smart appliances, logistics platforms, and decentralized inventories.

The **International Wine Identification Number (IWIN)** solves this problem by providing a fixed-width, positional, and semantic identifier. By alternating alphabetical and numerical blocks, an IWIN string embeds a wine’s strict biological, regulatory, and physical DNA directly into the code. IWIN enables instant local data parsing, algorithmic sorting, and full offline portability without requiring a central API query, establishing a new open infrastructure for global wine-tech applications.

---

### 2. Strategic Positioning: The ISBN of the Wine Industry
To protect interoperability and establish clear boundaries, IWIN defines its scope strictly as an **infrastructure protocol**:

* **IWIN is NOT a cellar manager:** It does not track personal bottle locations, maturity windows, or tasting notes. It provides the universal key (and QR standard) that software platforms can leverage to automate processing.
* **IWIN is NOT a rating or review system:** It records no subjective scores or commercial reviews. The standard is strictly factual, regulatory, and technical.
* **IWIN is NOT an e-commerce platform:** It does not sell wine, monitor retail pricing, or charge transactions.
* **IWIN is NOT a narrative encyclopedia:** It does not store estate stories or soil geology write-ups. The central registry validates only the legal existence of viticultural regions, appellations, vinifiers, and physical packaging.

By acting as an open, invisible, and universal infrastructure layer, IWIN empowers existing software tools rather than competing with them.

---

### 3. Representation Formats: Core vs. Extended
To accommodate both low-level database efficiency and human interaction (text entry, optical character recognition, mobile scanning), IWIN defines two logical levels and two structural layouts:

1. **IWIN-Core (22 characters in storage):** Represents the immutable identity of the liquid itself (Country, Vintage, Region, Appellation, Color, Producer, Cuvée).
2. **IWIN-Extended (29 characters in storage):** Extends the Core identity by appending a 7-character physical packaging segment (Volume in ml + Container Material).

#### Structural Encoding Formats
* **IWIN-Storage (Continuous):** Compact format stripped of punctuation, optimized for database primary keys, RFID/NFC chips, and barcode payloads.
  * *IWIN-Core Storage (22 char.):* `FR2019BOR0042R104852AA`
  * *IWIN-Extended Storage (29 char.):* `FR2019BOR0042R104852AA000750B`

* **IWIN-Display (Delimited):** Human-friendly format using a hyphen (`-`) anchor between logical blocks. Structural alternation between letters and digits reduces visual fatigue, improves scanning readability, and prevents OCR character shifting.
  * *IWIN-Core Display (28 char.):* `FR-2019-BOR-0042-R-104852-AA`
  * *IWIN-Extended Display (36 char.):* `FR-2019-BOR-0042-R-104852-AA-000750B`

---

### 4. Semantic Anatomy (Block-by-Block Specification)

An `IWIN-Extended-Storage` string consists of **8 distinct positional blocks** spanning exactly 29 characters.

| Block | Width & Type | Component | Description / Allowed Values |
| :---: | :---: | :--- | :--- |
| **1** | 2 Alpha | Country Code | ISO 3166-1 alpha-2 (e.g., `FR` for France, `IT` for Italy). |
| **2** | 4 Numeric | Vintage | 4-digit harvest year (e.g., `2019`). `0000` is reserved for Non-Vintage (NV). |
| **3** | 3 Alpha | Macro-Region | 3-letter uppercase viticultural region code (e.g., `BOR` for Bordeaux, `ALS` for Alsace), no duplicates allowed within the same country. |
| **4** | 4 Numeric | Appellation / Terroir | 4-digit sequential numeric code (`0001` to `9999`) assigned to legal AOC, DGC, Grand Cru, or IGP sub-zones. `0000` is a reserved software keyword for Wines Without Geographical Indication (*Vin de France*), no duplicates allowed within the same country. |
| **5** | 1 Alpha | Wine Color | `R` (Red), `W` (White), `P` (Rosé / Pink), `A` (Amber / Orange), `G` (Grey / Gris), `Y` (Yellow / Jaune). **The letter `O` is strictly forbidden.** |
| **6** | 6 Numeric | Vinifier / Producer ID | Unique 6-digit worldwide identifier (`000001` to `999999`) assigned to the vinifying entity (independent estate, merchant-vinifier, cooperative, or bottle-return grower), no duplicates allowed. |
| **7** | 2 Alpha | Cuvée / Label | 2-letter code (`AA` to `ZZ`) identifying a specific commercial label for a given vinifier and vintage (676 combinations available per vintage), no duplicates allowed within the same producer. |
| **8** | 7 Composite | Volume & Packaging | Composite block of **6 digits for volume in ml** (`000001` to `999999`) + **1 letter for container material**. |

#### Safety & Disambiguation Rules
* **The "O" Exclusion Rule:** The letter `O` (Orange) is banned in Block 5 to avoid visual confusion with the numeral `0`. Orange / Skin-Contact wines must use the code **`A`** (Amber).
* **Block 8 Packaging Material Codes (Position 29):**
  * `B` : Bottle (Glass bottle)
  * `C` : Can (Aluminum container)
  * `X` : Bag-in-Box (Cubitainer / Flexible boxed liquid)
  * `K` : Keg (Service keg / KeyKeg / Bulk barrel)
  * `T` : Tube (Glass or plastic sample tube / Vinotte)
  * `P` : Pouch (Flexible pouch format)

---

### 5. Deployment: Physical QR Code Payload

For physical integration on wine back-labels, cellar tags, and logistics cartons, the human-readable string is bound to a standard 2D QR Code.

#### 5.1 Standard URI Format
Official IWIN QR codes embed a structured cryptographic URL pointing to the universal resolver:
`https://iwincode.org/[IWIN-Extended-Storage]`

* *Example Payload:* `https://iwincode.org/FR2019BOR0042R104852AA000750B`

#### 5.2 Resolution Behavior
* **Consumer Interface:** Scanning the QR code with a standard smartphone camera navigates to `iwincode.org`, rendering an instant, localized technical passport of the wine.
* **Professional & System Interface:** Cellar management applications, warehouse scanners, and ERP inventory engines intercept the URL locally, extract the trailing 29 characters, and process the product identity entirely offline.

---

### 6. Intellectual Property & Central Authority
* **Open Standard Specification:** The IWIN format and parsing specifications are published under the **GNU GPLv3** license for non-commercial software implementations.
* **Registry Governance:** To prevent namespace collisions and data corruption, global allocation of Appellation Codes (Block 4) and Producer IDs (Block 6) is managed exclusively by the central IWIN Registry Authority.
* **Commercial Integration:** Commercial platforms, closed-source enterprise software, and hardware compliance integrations are governed via commercial licensing agreements with the IWIN Authority.
