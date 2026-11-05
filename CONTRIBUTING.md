# Contributing to the IWIN Standard Registry

Thank you for your interest in the **International Wine Identification Number (IWIN)** initiative! To achieve a truly global, decentralized, and semantic standard for wine data, we rely on the open-source community to seed, verify, and maintain our central dictionaries.

Whether you are a developer, a winery owner, or a wine tech platform, here is how you can contribute to the IWIN database registries.

---

## 1. How to Request New Registrations
We are actively populating the dockets for three main entities:
* **Wine Regions (Block 3 - 3 Alpha characters)**
* **Appellations / Designations (Block 4 - 4 Numeric characters)**
* **Producers (Block 6 - 6 Numeric characters)**

### Option A: Open an Issue (Non-technical contributors)
If you notice a missing appellation, region, or producer required for your cellar or software:
1. Navigate to the **Issues** tab on this repository.
2. Click **New Issue** and select the "Registry Addition Request" template.
3. Provide the official regulatory name, country of origin, and an official government link (e.g., EU eAmbrosia register, US TTB database) proving the legal status of the viticultural zone or entity.

### Option B: Submit a Pull Request (Developers)
If you wish to add raw data directly to our dictionary files:
1. Fork this repository.
2. Navigate to the appropriate country directory under `/registries/countries/[COUNTRY_CODE]/` (e.g., `/registries/countries/FR/` for France).
3. Append your data to the relevant JSON file (`[COUNTRY_CODE]_appellations.json`) using the **next available incremental sequential 4-digit code**.
4. Open a **Pull Request (PR)** tracking your changes.

---

## 2. Strict Registry Rules & Architectural Laws
To maintain the structural integrity of the IWIN standard, all data contributions must adhere to the following strict laws:

1. **Terroir-First Geography:** Wine regions and appellations must reflect legal viticultural zones, never generic political or administrative borders.
2. **Append-Only Sequences:** Once a 4-digit appellation code (e.g., `0424`) or a 6-digit producer ID (e.g., `104852`) is assigned, it can **never** be changed, re-ordered, or deleted. This ensures absolute backward compatibility.
3. **The "0000" Forbidden Submission Rule:** The code `0000` is a reserved application-level keyword for Wines Without Geographical Indication (VSIG / Vin de France). It is processed dynamically by software parsers and **must never be manually appended** to the physical JSON registry files.
4. **No Marketing Traps:** Do not submit entries for volatile wine classifications or commercial certifications (e.g., *Grand Cru Classé*, *Reserve*, *Organic Certified*). These are treated as downstream metadata tags and must not be encoded into the core identifier.

---

## 3. Code of Conduct
We aim to foster a collaborative, inclusive, and professional environment. Please ensure all discussions in Issues and Pull Requests remain respectful, professional, and focused on improving data quality for the global wine-tech ecosystem.
