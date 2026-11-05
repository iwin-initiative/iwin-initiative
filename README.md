# IWIN: International Wine Identification Number
## Technical Specification & Architecture Whitepaper
**Author:** IWIN Initiative  
**Status:** Proposal / Request for Comments (RFC)  
**Version:** 1.2.0 (November 2026)  
**Licensing:** Dual-Licensing (Open-Source GNU GPLv3 / Commercial)

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

### 3. Scope of the Project: What IWIN is NOT
To protect our roadmap and establish clear, non-competitive partnerships across the wine-tech ecosystem, IWIN defines its boundaries strictly. **IWIN is an infrastructure protocol, not an end-user application.**

* **IWIN is NOT a cellar manager:** We do not build software to log individual bottles, track drinking windows, or trigger inventory alerts. We provide the universal identification key (and QR code standard) that existing cellar managers can implement to automate their processing.
* **IWIN is NOT a rating or review system:** We do not collect user-generated stars, expert points, or tasting notes. The standard is strictly factual, regulatory, and technical.
* **IWIN is NOT a marketplace or e-commerce platform:** We do not sell wine, we do not compare retailer pricing, and we do not capture sales commissions.
* **IWIN is NOT a wine encyclopedia:** We do not store narrative histories of estates, detailed soil profiles, or food pairing suggestions. Our registry validates only the legal existence of an appellation, a producer, and a packaging format.

By positioning itself as the **ISBN of the wine world**, IWIN acts as an invisible, free, and open infrastructure designed to empower existing platforms rather than replace them.

### 4. Code Architecture: Positional Alternation & Width Standards
To optimize for both database efficiency and everyday consumer interactions (such as text input, user interface navigation, and smartphone camera OCR scanning), the IWIN standard introduces a dual-layered representation framework:

1. **IWIN-Core:** The immutable identity representation of the wine liquid itself. It ends strictly at the Cuvée / Label level (22 characters in storage format).
2. **IWIN-Extended:** The full commercial and physical product variant string, adding a 7-character logical segment to isolate specific packaging formats and high-capacity volumes (29 characters in storage format).

#### 4.1 Structural Comparison: Continuous vs. Delimited
The IWIN standard defines two official encodings for these representations:

* **IWIN-Storage (Continuous):** An ultra-compact format stripped of all punctuation, optimized for database indexes, NFC/RFID tags, and barcode compilation.
  * *IWIN-Core (22 char.):* `FR2019BOR0042R104852AA`
  * *IWIN-Extended (29 char.):* `FR2019BOR0042R104852AA000750B`

* **IWIN-Display (Delimited):** A human-centric format utilizing a hyphen (`-`) anchor between logical segments. The structural alternation between alphabetical and numerical characters lowers OCR interpretation error rates and prevents character-shifting data corruption.
  * *IWIN-Core (28 char.):* `FR-2019-BOR-0042-R-104852-AA`
  * *IWIN-Extended (36 char.):* `FR-2019-BOR-0042-R-104852-AA-000750B`

---

### 5. Semantic Anatomy (Block-by-Block)

The complete IWIN string (`IWIN-Extended-Storage`) is divided into 8 distinct block attributes spanning a fixed 29-character structure. The first 7 blocks (positions 1 to 22) isolate the liquid identity (`IWIN-Core`), while Block 8 completes the physical commercial variant.

| Block | Character Position | Component | Description / Allowed Values |
| :---: | :---: | :--- | :--- |
| **1** | 1 - 2 | Country Code | ISO 3166-1 alpha-2 (e.g., `FR` for France). |
| **2** | 3 - 6 | Vintage | 4-digit numeric year (e.g., `2019`). `0000` for Non-Vintage (NV). |
| **3** | 7 - 9 | Region | 3-letter uppercase regional code (e.g., `BOR` for Bordeaux). |
| **4** | 10 - 13 | Appellation | 4-digit sequential numeric registry code (`0001` to `9999`). |
| **5** | 14 | Wine Color | 1-letter uppercase code (`R`=Red, `W`=White, `P`=Rosé, `A`=Amber/Orange). **Letter `O` is strictly forbidden**. |
| **6** | 15 - 20 | Producer ID | 6-digit unique identifier for the winery or cooperative (e.g., `104852`). |
| **7** | 21 - 22 | Cuvée / Label | 2-letter uppercase code representing the specific cuvée. Total flexibility from `AA` to `ZZ`. |
| **8** | 23 - 29 | Volume & Type | 7-character composite block: **6 digits for volume** in milliliters (up to 999,999 ml) + **1 letter for packaging material**. |

#### ⚠️ Critical Ambiguity and Safety Rules
* **The "O" Exclusion Rule:** In Block 5 (Color), the letter `O` (Orange) is completely banned to avoid visual confusion with the numeral `0`. It is replaced by **`A`** (Amber/Orange).
* **Block 8 Packaging Material Suffixes (Position 29):** 
  * `B` : Bottle (Traditional glass bottles)
  * `C` : Can (Aluminum cans / Canettes)
  * `X` : Bag-in-Box (Cubitainers / Flexible box packaging)
  * `K` : Keg (Service kegs / KeyKegs / Industrial bulk barrels)
  * `T` : Tube (Glass or plastic sample tubes / Vinottes)
  * `P` : Pouch (Flexible modern pouches)

---

### 6. Physical Field Deployment: The QR Code Standard
To ensure seamless operations in cellar environments, inventory scanning, and retail distribution, the human-readable alphanumeric code is bound to a high-density 2D **QR Code matrix**.

#### 6.1 Unified URL Routing (The Payload)
Rather than raw data strings, the official IWIN QR code embeds a structured, resolvable cryptographic URL pointing directly to the open registry gateway:
`https://iwincode.org[IWIN-Extended-Storage]`

* *Example Payload:* `https://iwincode.orgFR2019BOR0042R104852AA000750B`

#### 6.2 Operational Interception Layer
* **Consumer Mode:** Scanning via a stock smartphone camera automatically opens the browser to `iwincode.org`, translating the code into a clean, localized technical identity sheet.
* **Professional Mode:** Inventory applications, smart cellars, and ERP scanners intercept the incoming URL string locally, isolate the final 29 characters, and perform zero-latency database operations entirely offline.

---

### 7. Intellectual Property, Governance & Registry Authority
* **Open Standard Specification:** The structural mechanics of the IWIN format (positional 22-character Core and 29-character Extended layouts) are published openly to maximize global adoption in the wine-tech industry under the **GNU GPLv3** license for non-commercial software.
* **Registry & Authority Control:** To prevent data corruption, namespace collisions, and algorithmic parsing errors, the distribution of Producer IDs (Block 6) and official Appellation Sequences (Block 4) is strictly restricted. These sequences are exclusively managed by the official IWIN Central Registry Authority.
* **Trademark & Commercial Licensing:** While the technical nomenclature is open for public routing, commercial implementation, compliance badges, and automated API resolution services are strictly governed. Commercial operations, closed-source integrations, and certified software labels require an official agreement with the designated registry management company.
