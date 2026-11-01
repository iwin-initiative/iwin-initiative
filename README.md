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
  * *Example (Core):* `FR2019BOR0042R104852AA`
* **IWIN-Display (Delimited):** A human-centric format utilizing a hyphen (`-`) anchor between logical segments. The structural alternation between alphabetical and numerical characters, combined with visual delimiters, dramatically lowers OCR interpretation error rates and prevents character-shifting data corruption.
  * *Example (Extended):* `FR-2019-BOR-0042-R-104852-AA-01500`

---

### 4. Semantic Anatomy Block-by-Block
The 32-character architecture of the IWIN-Extended string is strictly partitioned into seven mandatory core blocks and one optional logistics block:

| Block | Attribute | Data Type | Fixed Width | Specification & Allowed Values |
| :--- | :--- | :--- | :---: | :--- |
| **1** | Country | Alpha | 2 | Official ISO 3166-1 alpha-2 country codes (e.g., `FR`, `IT`, `US`). |
| **2** | Vintage | Numeric | 4 | 4-digit harvest year. Non-Vintage (NV) products use `0000` as the mandatory fallback. |
| **3** | Wine Region | Alpha | 3 | Pure viticultural macro-regions mapped by the IWIN Central Registry (e.g., `BOR` for Bordeaux, `ALS` for Alsace). Unique within each country string. |
| **4** | Appellation / DGC | Numeric | 4 | Chronological sequential designation code from `0001` to `9999` managed by the IWIN Registry. Encapsulates full AOC/AOP levels, including Dénominations Géographiques Communales (DGC) and local Crus. Wines without legal designation default to `0000`. |
| **5** | Liquid Color | Alpha | 1 | Strictly bounded taxonomy: `R` (Red), `W` (White), `P` (Pink/Rosé), `A` (Amber/Orange), `G` (Gris/Grey). Excludes `O` to prevent OCR confusion with the number `0`. |
| **6** | Producer ID | Numeric | 6 | Unique structural corporate key from `000001` to `999999` managed by the central ledger. Can be mapped to sequential registrations or regulatory tax keys. |
| **7** | Cuvée / Blend | Alpha | 2 | Micro-product partition code supporting 676 combinations from `AA` to `ZZ` to isolate single vineyards, second labels, or specific estate blends. **Strictly reserved for producer branding; geographic mentions are prohibited here.** |
| **8*** | Bottle Volume | Numeric | 5 | *Optional Logistics Extension.* Fixed-width allocation representing capacity in milliliters (ml), spanning from `00188` (Piccolo) up to `30000` (Melchizedek) and beyond. |

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

---

### 6. Intellectual Property, Trademark & Governance
* **Trademark:** The name **"IWIN"**, the acronym **"International Wine Identification Number"**, and its official branding are protected legal trademarks. Unauthorized commercial use of the brand name to label software, APIs, or database services as "IWIN-Certified" or "IWIN-Compatible" without written consent is strictly prohibited.
* **Dual-Licensing Framework:** This project operates on a dual-license model to balance rapid community adoption with commercial sustainability. Non-commercial and open-source applications may use the standard freely under the **GNU GPLv3** license. Proprietary, commercial, and closed-source enterprise systems must secure a commercial agreement.
