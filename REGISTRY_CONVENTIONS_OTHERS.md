# IWIN Registry Conventions & Architecture Guidelines

This document outlines the operational and architectural rules governing the central dictionaries of the **International Wine Identification Number (IWIN)** initiative.

## 1. Structure of Central Dictionaries

All IWIN dictionary data is maintained in structured JSON files within the `/registries/` directory, organized by country code (ISO 3166-1 alpha-2).

### 1.1 Viticultural Regions (Block 3)

* File: `/registries/countries/[COUNTRY_CODE]/[COUNTRY_CODE]_regions.json`

* **Rules:** Regions represent recognised legal viticultural areas (e.g., `BOR` for Bordeaux, `BRG` for Burgundy, `ALS` for Alsace), not administrative political borders.

### 1.2 Appellations & Terroirs (Block 4)

See details in dedicated md file.

## 2. Vinifier / Producer Registry (Block 6)

Block 6 encodes the vinifying entity using a unique 6-digit numeric ID (`000001` to `999999`), yielding an available namespace of nearly 1,000,000 global entities.

### 2.1 Scope & Definition of a "Producer"

To ensure structural consistency across diverse production models:

* **The Vinifier Principle:** A Block 6 entity is strictly defined as the *vinifier* (independent estate, merchant-vinifier, or cooperative winery transforming grapes into finished wine). Grape growers who solely sell raw fruit to third parties do not receive a Block 6 code.

* **The Bottle-Return Exception (*Retour de Bouteilles*):** Viticulturists who deliver grapes to a cooperative but retain legal rights to receive and market finished bottles under their own commercial label are assigned a unique Block 6 ID, as they operate as distinct commercial wine entities.

### 2.2 Global Single-Namespace & Hybrid Pivot System

* **No Regional Sub-segmentation:** Block 6 IDs are allocated from a single global numeric sequence without regional prefixes, avoiding redundancy with Blocks 1 (Country) and 3 (Region).

* **Allocation Modes:**

  1. *Sequential Assignment (Default):* New applicants receive the next available incremental 6-digit number.

  2. *Custom Preference Assignment (Optional):* Applicants may request a specific, available 6-digit number.

* **Government Meta-Registry Pivot:** To guarantee identity verification, each IWIN producer entry maps its immutable `iwin_producer_id` to official national registers (e.g., SIRENE/CVI in France, TTB permits in the US, P.IVA in Italy).

```
{
  "iwin_producer_id": "104852",
  "name": "Château Margaux",
  "entity_type": "independent_winery",
  "country": "FR",
  "legal_identity": {
    "official_register": "SIRENE (INSEE)",
    "official_id": "38012845200018"
  },
  "address": {
    "locality": "Margaux-Cantenac",
    "macro_region": "BOR",
    "coordinates": [45.0441, -0.6698]
  }
}

```

## 3. Cuvée / Label Recommendations (Block 7)

See details in dedicated md file.

## 4. Packaging & Volume Registry (Block 8)

Block 8 is a 7-character composite segment: **6 digits for exact volume in milliliters** (zero-padded) + **1 letter for container material**.

| Material Suffix | Category | Examples | 
| ----- | ----- | ----- | 
| **B** | Bottle | Standard glass bottles (`000750B`), Magnums (`001500B`), Clavelins (`000620B`). | 
| **C** | Can | Aluminum wine cans (`000250C`). | 
| **X** | Bag-in-Box | Cubitainers & boxed wines (`003000X`, `005000X`). | 
| **K** | Keg | Stainless steel or KeyKeg barrels (`020000K`). | 
| **T** | Tube | Sample glass/plastic tubes (`000100T`). | 
| **P** | Pouch | Flexible modern liquid pouches (`001500P`). | 



