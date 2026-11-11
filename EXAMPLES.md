# IWIN Reference Examples

This document provides non-official reference encodings for the **International Wine Identification Number (IWIN)** across both **IWIN-Storage** (compact) and **IWIN-Display** (delimited) formats. These examples were compiled at the very start of the project therefore the Region, Appellation and Producer blocks (blocks 2, 4 and 6) do not display actual ids. These examples will be updated when those ids are defined for good.

---

### Example 1: Classic Estate (Grand Vin)
* **Wine:** Château Margaux 2019
* **Attributes:** Red, Still, Standard Glass Bottle (750ml)
* **Anatomy:** France (`FR`) | 2019 (`2019`) | Bordeaux (`BOR`) | Margaux AOC (`0042`) | Red (`R`) | Producer (`104852`) | Grand Vin (`GV`) | 750ml Bottle (`000750B`)

* **IWIN-Core (Storage - 22 char.):** `FR2019BOR0042R104852GV`
* **IWIN-Extended (Storage - 29 char.):** `FR2019BOR0042R104852GV000750B`
* **IWIN-Extended (Display - 36 char.):** `FR-2019-BOR-0042-R-104852-GV-000750B`

---

### Example 2: Non-Vintage Champagne (Magnum Format)
* **Wine:** Champagne Bollinger Special Cuvée (Non-Vintage)
* **Attributes:** White, Sparkling, Glass Magnum Bottle (1500ml)
* **Anatomy:** France (`FR`) | Non-Vintage (`0000`) | Champagne (`CHA`) | Champagne AOC (`0417`) | White (`W`) | Producer (`122450`) | Flagship Cuvée (`FL`) | 1500ml Bottle (`001500B`)

* **IWIN-Core (Storage - 22 char.):** `FR0000CHA0417W122450FL`
* **IWIN-Extended (Storage - 29 char.):** `FR0000CHA0417W122450FL001500B`
* **IWIN-Extended (Display - 36 char.):** `FR-0000-CHA-0417-W-122450-FL-001500B`

---

### Example 3: Amber / Skin-Contact Wine
* **Wine:** Gravner Ribolla Gialla 2015
* **Attributes:** Amber / Orange (Amphora Matured), Standard Glass Bottle (750ml)
* **Anatomy:** Italy (`IT`) | 2015 (`2015`) | Friuli (`FRI`) | Venezia Giulia IGT (`0089`) | Amber (`A`) | Producer (`200561`) | Estate Cuvée (`ES`) | 750ml Bottle (`000750B`)

* **IWIN-Core (Storage - 22 char.):** `IT2015FRI0089A200561ES`
* **IWIN-Extended (Storage - 29 char.):** `IT2015FRI0089A200561ES000750B`
* **IWIN-Extended (Display - 36 char.):** `IT-2015-FRI-0089-A-200561-ES-000750B`

---

### Example 4: Wine Without Geographical Indication (Vin de France)
* **Wine:** Le Petit Rimauresq 2023
* **Attributes:** Rosé, Vin de France (No Appellation Entry), Glass Half-Bottle (375ml)
* **Anatomy:** France (`FR`) | 2023 (`2023`) | Provence (`PRV`) | Non-Appellation Reserved Keyword (`0000`) | Rosé (`P`) | Producer (`109432`) | Entry Level Cuvée (`AA`) | 375ml Bottle (`000375B`)

* **IWIN-Core (Storage - 22 char.):** `FR2023PRV0000P109432AA`
* **IWIN-Extended (Storage - 29 char.):** `FR2023PRV0000P109432AA000375B`
* **IWIN-Extended (Display - 36 char.):** `FR-2023-PRV-0000-P-109432-AA-000375B`

---

### Example 5: Alternative Packaging (Bag-in-Box)
* **Wine:** Domaine du Pont 2024 (Syrah / Grenache)
* **Attributes:** Red, Still, 3-Liter Bag-in-Box
* **Anatomy:** France (`FR`) | 2024 (`2024`) | Languedoc (`LGD`) | Pays d'Oc IGP (`0528`) | Red (`R`) | Producer (`105612`) | Cuvée Tradition (`TR`) | 3000ml Bag-in-Box (`003000X`)

* **IWIN-Core (Storage - 22 char.):** `FR2024LGD0528R105612TR`
* **IWIN-Extended (Storage - 29 char.):** `FR2024LGD0528R105612TR003000X`
* **IWIN-Extended (Display - 36 char.):** `FR-2024-LGD-0528-R-105612-TR-003000X`
