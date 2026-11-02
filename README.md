# IWIN: International Wine Identification Number
## Technical Specification & Architecture Whitepaper
**Author:** IWIN Initiative  
**Status:** Proposal / Request for Comments (RFC)  
**Version:** 1.1.0 (November 2026)

---

### 1. Abstract
The current digital wine ecosystem lacks a decentralized, human-readable, and semantic data standard. Existing methodologies, such as the Liv-ex Wine Identification Number (LWIN), rely on arbitrary sequential database keys that require constant API lookups to resolve fundamental product attributes like country, appellation, or color. This architecture creates high friction for consumer-facing cellar management software, smart appliances, and decentralized inventories. 

This paper introduces the **International Wine Identification Number (IWIN)**, a fixed-width, positional, and semantic identifier. By alternating alpha and numeric blocks, IWIN embeds a wine’s strict biological and regulatory DNA (Country, Vintage, Wine Region, Appellation/DGC, Color, Producer, and Cuvée) directly into the code string. IWIN enables instant local data parsing, algorithmic sorting, and offline portability without the need for a centralized dictionary, setting a new standard for the consumer wine-tech industry.

### 2. The Problem Statement: Why LWIN and GTIN Fail Consumers
While global standards exist for trade and retail checkout, they are fundamentally unsuited for consumer application development and personal inventory tracking:

* **GTIN (Global Trade Item Number / Barcodes):** Barcodes are designed for point-of-sale retail logistics. They lack structural metadata, cannot be retroactively applied to rare or back-vintage cellared wines, and do not inherently link identical liquids packaged for different regional distributors.
* **LWIN (Liv-ex Wine Identification Number):** Designed for financial trading floors, LWIN treats wine like a stock ticker. An LWIN string contains no geographic intelligence. To filter a digital cellar by "French wines" or "Napa Valley AVAs," an application must perform heavy, costly metadata queries against a centralized server for every single bottle. If the server goes offline, the data becomes unreadable.
* **The Manual Entry Fatigue:** Because consumer applications lack a semantic standard, users are forced to endure tedious manual data entry or suffer from high error rates in Optical Chemical Recognition (OCR) label scanning, leading to rapid user abandonment.

---

### 3. Code Architecture: Positional Alternation & Human-Readable Layouts
To optimize for both database efficiency and everyday consumer interactions (such as text input, user interface navigation, and smartphone camera OCR scanning), the IWIN standard introduces a dual-layered representation framework:

1. **IWIN-Core (27 Characters):** The immutable cryptographic and biological representation of the wine liquid itself. It ends strictly at the Cuvée identifier.
2. **IWIN-Extended (32 Characters):** The full logistics and inventory string, adding a 5-digit fixed-width volume indicator in milliliters (ml) to isolate specific commercial physical variants.

#### 3.1 Structural Comparison: Continuous vs. Delimited
The IWIN standard defines two official encodings for these representations:
* **IWIN-Storage (Continuous):** An ultra-compact format stripped of all punctuation, optimized for database indexes, NFC/RFID tags, and barcode compilation. 
  * *Example (Core):* `FR2019BOR0262R104852AA`
* **IWIN-Display (Delimited):** A human-centric format utilizing a hyphen (`-`) anchor between logical segments. The structural alternation between alphabetical and numerical characters, combined with visual delimiters, dramatically lowers OCR interpretation error rates and prevents character-shifting data corruption.
  * *Example (Extended):* `FR-2019-BOR-0262-R-104852-AA-01500`

---

### 4. Semantic Anatomy (Block-by-Block)

The IWIN string is divided into 8 distinct block attributes, standardizing all key features of a wine product into a fixed 27-character structure (`IWIN-Storage`).

| Block | Character Position | Component | Description / Allowed Values |
| :---: | :---: | :--- | :--- |
| **1** | 1 - 2 | Country Code | ISO 3166-1 alpha-2 (e.g., `FR` for France). |
| **2** | 3 - 6 | Vintage | 4-digit numeric year (e.g., `2019`). `0000` for Non-Vintage (NV). |
| **3** | 7 - 9 | Region | 3-letter uppercase regional code (e.g., `BOR` for Bordeaux, `CHA` for Champagne). |
| **4** | 10 - 13 | Appellation | 4-digit sequential numeric registry code (e.g., `0262` for Margaux, `0417` for Champagne AOC). |
| **5** | 14 | Wine Color | 1-letter uppercase code (`R` = Red, `W` = White, `P` = Pink/Rosé, `O` = Orange). |
| **6** | 15 - 20 | Producer ID | 6-digit unique identifier for the winery or cooperative. |
| **7** | 21 - 22 | Label / Packaging | 2-letter uppercase code representing the specific cuvée, label level, or special packaging type. |
| **8** | 23 - 27 | Volume | 5-digit numeric volume in milliliters, matching the global `volumes_registry.json`. |

#### ⚠️ Critical Rule for Block 7 (Label / Packaging)
To ensure universal identification of alternative packaging methods without changing the 27-character database constraint, **the letter `Z` is strictly reserved and restricted**:
* **Standard Glass Bottles (`AA` to `YY`):** Producers can use any 2-letter combination starting from `AA` up to `YY` to identify their standard wine labels or cuvées. **The letter `Z` is completely forbidden** anywhere in these codes (e.g., codes like `AZ`, `CZ`,`ZA`, or `ZZ` cannot be assigned to standard bottles).
* **Alternative Packaging (`Z[A-Z]`):** Any code where the first character is **`Z`** signals to parsers and users that the wine is not packaged in a traditional glass bottle.
  * `ZA`: Aluminum Can (Canette)
  * `ZP`: Flexible Pouch (Poche souple)
  * `ZK`: Service Keg / KeyKeg (Fût)
  * `ZX`: Bag-in-Box / Cubitainer

---

### 5. Data Governance & Extensibility
The IWIN architecture operates on a decentralized resolution model but requires a lightweight, federated Central Registry to prevent collisions in the Producer (Block 6) and Appellation (Block 4) sequences. 

#### 5.1 Non-Destructive Schema Updates
To maintain absolute backward compatibility, the IWIN Central Registry enforces a strict "append-only" policy for dictionary updates. If a geopolitical event creates a new wine region or a regulatory body introduces a new AOC/AVA/DGC, the registry issues the next incremental numeric code without re-indexing historical records. 

#### 5.2 Decoupling Marketing Content from Identity
The IWIN standard strictly separates physical and geographic identity from commercial classifications and marketing designations (e.g., *Grand Cru Classé*, *Riserva*, *Organic Certified*). These volatile or region-specific attributes are classified as downstream metadata tags. They must be resolved via application-level dictionaries mapped to the persistent IWIN-Core string, ensuring the identifier remains permanent and unaffected by regulatory disputes or branding updates.

---

### 6. Intellectual Property, Governance & Registry Authority
* **Open Standard Specification:** The structural mechanics of the IWIN format (positional 27-character Core and 32-character Extended layouts) are published openly to maximize global adoption in the wine-tech industry under the **GNU GPLv3** license for non-commercial software.
* **Registry & Authority Control:** To prevent data corruption, namespace collisions, and algorithmic parsing errors, the distribution of Producer IDs (Block 6) and official Appellation Sequences (Block 4) is strictly restricted. These sequences are exclusively managed by the official IWIN Central Registry Authority.
* **Trademark & Commercial Licensing:** While the technical nomenclature is open for public routing, commercial implementation, compliance badges, and automated API resolution services are strictly governed. Commercial operations, closed-source integrations, and certified software labels require an official agreement with the designated registry management company (currently being incorporated in Luxembourg).

