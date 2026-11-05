# IWIN Syntax Examples

This document provides concrete reference examples of the **International Wine Identification Number (IWIN)** syntax, showcasing both the **IWIN-Display** (delimited) and **IWIN-Storage** (continuous) formats under the updated 29/36-character technical specification.

---

### Example 1: Old World Estate (Standard Configuration)
* **Wine:** Château Margaux 2019 (Grand Vin)
* **Attributes:** Red, Still, Standard Glass Bottle (750ml)
* **Anatomy:** France (`FR`) - 2019 - Bordeaux (`BOR`) - Margaux AOC (`0042`) - Red (`R`) - Producer ID (`104852`) - First Label (`AA`) - 750ml with Glass Suffix (`000750B`)

* **IWIN-Core (Storage - 22 char.):** `FR2019BOR0042R104852AA`
* **IWIN-Extended (Storage - 29 char.):** `FR2019BOR0042R104852AA000750B`
* **IWIN-Extended (Display - 36 char.):** `FR-2019-BOR-0042-R-104852-AA-000750B`

---

### Example 2: Non-Vintage Sparkling in Large Format
* **Wine:** Champagne Bollinger Special Cuvée (Non-Vintage)
* **Attributes:** White, Sparkling, Glass Magnum (1.5L)
* **Anatomy:** France (`FR`) - Non-Vintage (`0000`) - Champagne (`CHA`) - Champagne AOC (`0417`) - White (`W`) - Producer ID (`122450`) - Flagship Blend (`AA`) - 1500ml with Glass Suffix (`001500B`)

* **IWIN-Core (Storage - 22 char.):** `FR0000CHA0417W122450AA`
* **IWIN-Extended (Storage - 29 char.):** `FR0000CHA0417W122450AA001500B`
* **IWIN-Extended (Display - 36 char.):** `FR-0000-CHA-0417-W-122450-AA-001500B`

---

### Example 3: New World AVA Product
* **Wine:** Screaming Eagle "The Flight" 2021
* **Attributes:** White, Still, Standard Glass Bottle (750ml)
* **Anatomy:** United States (`US`) - 2021 - Napa Valley (`NAP`) - Oakville AVA (`0105`) - White (`W`) - Producer ID (`300124`) - "The Flight" Cuvée (`BB`) - 750ml with Glass Suffix (`000750B`)

* **IWIN-Core (Storage - 22 char.):** `US2021NAP0105W300124BB`
* **IWIN-Extended (Storage - 29 char.):** `US2021NAP0105W300124BB000750B`
* **IWIN-Extended (Display - 36 char.):** `US-2021-NAP-0105-W-300124-BB-000750B`

---

### Example 4: Modern Skin-Contact / Amber Wine
* **Wine:** Gravner Ribolla Gialla 2015
* **Attributes:** Amber/Orange, Matured in Amphora, Standard Glass Bottle (750ml)
* **Anatomy:** Italy (`IT`) - 2015 - Friuli (`FRI`) - Venezia Giulia IGT / IGP (`0089`) - Amber (`A`) - Producer ID (`200561`) - Estate Cuvée (`AA`) - 750ml with Glass Suffix (`000750B`)

* **IWIN-Core (Storage - 22 char.):** `IT2015FRI0089A200561AA`
* **IWIN-Extended (Storage - 29 char.):** `IT2015FRI0089A200561AA000750B`
* **IWIN-Extended (Display - 36 char.):** `IT-2015-FRI-0089-A-200561-AA-000750B`

---

### Example 5: Generic "Vin de France" (Dynamic Fallback Resolution)
* **Wine:** Le Petit Rimauresq 2023 
* **Attributes:** Rosé, Vin de France (No geographical indication entry), Glass Half-Bottle (375ml)
* **Anatomy:** France (`FR`) - 2023 - Provence (`PRV`) - Dynamic Non-Appellation Keyword (`0000`) - Pink/Rosé (`P`) - Producer ID (`109432`) - Entry Level Cuvée (`AA`) - 375ml with Glass Suffix (`000375B`)

* **IWIN-Core (Storage - 22 char.):** `FR2023PRV0000P109432AA`
* **IWIN-Extended (Storage - 29 char.):** `FR2023PRV0000P109432AA000375B`
* **IWIN-Extended (Display - 36 char.):** `FR-2023-PRV-0000-P-109432-AA-000375B`
