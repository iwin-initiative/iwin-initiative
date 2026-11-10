# IWIN Reference Registries & Dictionaries

This directory structures the decentralized data dictionaries for the **International Wine Identification Number (IWIN)** protocol. These files serve as the static ground truth for local validation, client-side parsing, and offline inventory resolution.

## 📂 Directory Structure

```text
registries/
├── README.md                          # This architectural guidance file
├── volumes_registry.json               # Global volume capacities & packaging suffixes (7 char.)
├── appellation_types_registry.json     # Global regulatory tier definitions (AOC, IGP, AVA...)
└── countries/                          # Federated territorial country registries
    └── FR/
        ├── FR_regions.json             # 3-Letter regional macro-mappings (Block 3)
        └── FR_appellations.json        # 4-Digit incremental sequential codes (Block 4)
```

## 🛠️ Data Standards & Continuous Integrity

All registry maintenance, dictionary updates, or pull requests must comply with the structural boundaries defined in `README.md` and `REGISTRY_CONVENTIONS.md` at the root of the repository:

1. **Fixed-Width Mapping:** Ensure that any additions to `FR_appellations.json` feature strict 4-digit zero-padded keys (`0001` to `9999`).
2. **Dynamic Keyword Protection:** Do not attempt to log the code `0000` inside country databases. This is an application-only keyword reserved for Wines Without Geographical Indication (VSIG / Vin de France).
3. **Composite Volume Compliance:** The `volumes_registry.json` operates on a 7-character composite formatting model (`6 digits volume in ml` + `1 letter packaging material suffix`), natively mapping combinations like `000750B` (Standard Glass Bottle) or `003000X` (3-Liter Bag-in-Box).
4. **Naming Consistency:** All added appellations must follow the explicit syntax: `"Appellation Name - Mandatory Suffix (TYPE IN CAPITAL LETTERS)"` (e.g., `Champagne (AOC)`).
