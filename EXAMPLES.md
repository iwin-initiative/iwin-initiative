# IWIN Syntax Examples

This document provides concrete reference examples of the **International Wine Identification Number (IWIN)** syntax, showcasing both the **IWIN-Display** (delimited) and **IWIN-Storage** (continuous) formats under the updated 32-character technical specification.

---

### Example 1: Old World Estate (Standard Configuration)
* **Wine:** Château Margaux 2019 (Grand Vin)
* **Attributes:** Red, Still, Standard Bottle (750ml)
* **Anatomy:** France (`FR`) - 2019 - Bordeaux (`BOR`) - Margaux AOC (`0042`) - Red (`R`) - Producer ID (`104852`) - First Label (`AA`) - 750ml (`00750`)

* **IWIN-Display:** `FR-2019-BOR-0042-R-104852-AA-00750`
* **IWIN-Storage:** `FR2019BOR0042R104852AA00750`

---

### Example 2: Non-Vintage Sparkling in Large Format
* **Wine:** Champagne Bollinger Special Cuvée (Non-Vintage)
* **Attributes:** White, Sparkling, Magnum (1.5L)
* **Anatomy:** France (`FR`) - Non-Vintage (`0000`) - Champagne (`CHA`) - Champagne AOC (`0001`) - White (`W`) - Producer ID (`122450`) - Flagship Blend (`AA`) - 1500ml (`01500`)

* **IWIN-Display:** `FR-0000-CHA-0001-W-122450-AA-01500`
* **IWIN-Storage:** `FR0000CHA0001W122450AA01500`

---

### Example 3: New World AVA Product
* **Wine:** Screaming Eagle "The Flight" 2021
* **Attributes:** White, Still, Standard Bottle (750ml)
* **Anatomy:** United States (`US`) - 2021 - Napa Valley (`NAP`) - Oakville AVA (`0105`) - White (`W`) - Producer ID (`300124`) - "The Flight" Cuvée (`BB`) - 750ml (`00750`)

* **IWIN-Display:** `US-2021-NAP-0105-W-300124-BB-00750`
* **IWIN-Storage:** `US2021NAP0105W300124BB00750`

---

### Example 4: Modern Skin-Contact / Amber Wine
* **Wine:** Gravner Ribolla Gialla 2015
* **Attributes:** Amber/Orange, Matured in Amphora, Standard Bottle (750ml)
* **Anatomy:** Italy (`IT`) - 2015 - Friuli (`FRI`) - Venezia Giulia IGT / IGP (`0089`) - Amber (`A`) - Producer ID (`200561`) - Estate Cuvée (`AA`) - 750ml (`00750`)

* **IWIN-Display:** `IT-2015-FRI-0089-A-200561-AA-00750`
* **IWIN-Storage:** `IT2015FRI0089A200561AA00750`

---

### Example 5: Generic "Vin de Table" (Fallback Resolution)
* **Wine:** Le Petit Rimauresq 2023 
* **Attributes:** Rosé, Table Wine (No geographic indication beyond country), Half-Bottle (375ml)
* **Anatomy:** France (`FR`) - 2023 - Provence (`PRO`) - Generic Non-Appellation (`0000`) - Pink/Rosé (`P`) - Producer ID (`109432`) - Entry Level Cuvée (`AA`) - 375ml (`00375`)

* **IWIN-Display:** `FR-2023-PRO-0000-P-109432-AA-00375`
* **IWIN-Storage:** `FR2023PRO0000P109432AA00375`
