# IWIN Registry Coding Conventions

To maintain a strict 31-character fixed-width structural integrity across diverse global wine laws, the IWIN standard applies specific contextual rules to **Block 7 (Cuvée / Blend)** for certain wine regions. 

This document tracks active regional matrix assignments managed by the IWIN Central Registry.

---

## 1. France: Alsace Viticultural Framework

Because the Alsace regulatory framework heavily relies on single-varietal labeling and specific single-vineyard terroirs (Lieux-dits) rather than heavily splintered geographic appellations, the IWIN standard utilizes standardized **Letter Ranges** inside **Block 7** to preserve schema uniformity.

### 1.1 Alsace AOC (Code `001`) Varietal Allocation Mapping
Wineries and developers must map single-varietal products and traditional regional blends within the `AA` to `ZZ` sequence using the following standardized alpha anchors for the first letter of Block 7:

* **`AA` to `AM`:** Reserved for **Riesling** cuvées (e.g., `AA` for Tradition, `AB` for Vieilles Vignes).
* **`GA` to `GM`:** Reserved for **Gewurztraminer** cuvées.
* **`PA` to `PM`:** Reserved for **Pinot Gris** cuvées.
* **`BA` to `BM`:** Reserved for **Pinot Blanc / Auxerrois** cuvées.
* **`NA` to `NM`:** Reserved for **Pinot Noir** cuvées (Red and Rosé vinifications).
* **`SA` to `SM`:** Reserved for **Sylvaner** cuvées.
* **`MA` to `MM`:** Reserved for **Muscat** cuvées.
* **`EA` to `EM`:** Reserved for Traditional Blends (**Edelzwicker / Gentil**).

*Applications reading an IWIN string containing `ALS-001` will seamlessly resolve the primary grape variety locally by referencing these strict boundary envelopes.*

### 1.2 Alsace Grand Cru AOC (Code `002`) Terroir Resolution
The 51 legally protected Grand Cru lieux-dits do not receive individual 3-digit numeric codes in Block 4. They all share the global parent Appellation code `002`. 

The specific micro-geography of the Grand Cru is mapped entirely via Block 7, combining the vineyard identity and the grape variety. The IWIN Central Registry maintains the sequential allocation from `AA` to `ZZ` for each certified estate (e.g., a producer making a Riesling on the *Rangen* grand cru will be allocated `RA`, while their Gewurztraminer on the same plot will be allocated `RB`).


---

## 2. France: Bordeaux Historical Mergers & Chronological Resolution

To prevent data inflation and avoid maintaining ghost codes for historically absorbed appellations, the IWIN standard uses a **Chronological Resolution Rule** driven by application-level logic based on Block 2 (Vintage).

### 2.1 The Néac / Lalande-de-Pomerol Case (Code `040`)
The appellation *Néac AOC* was legally absorbed into *Lalande-de-Pomerol AOC* in 1954. No commercial wines have been produced under the Néac name since that date.
* **Database Encoding:** All bottles from this terroir—regardless of age—must strictly use the active Appellation code `040`.
* **UI Resolution Rule:** Applications reading an IWIN string containing `BOR-040` must execute a temporal condition check:
  * If **Block 2 (Vintage) < 1954**, the interface MUST display the historical designation: **"Néac AOC"**.
  * If **Block 2 (Vintage) >= 1954**, the interface MUST display the active designation: **"Lalande-de-Pomerol AOC"**.

*This approach preserves structural data consistency for cross-referencing while ensuring absolute authenticity for historical cellar assets.*


---

## 3. France: Burgundy Viticultural Framework & DGC Virtualization

Burgundy is the most fragmented wine region globally. To avoid schema inflation within Block 4 (Appellation), the IWIN standard virtualizes **Dénominations Géographiques Complémentaires (DGC)** (geographic village sub-mentions) by anchoring them to their legal parent generic appellation and partitioning the specific municipality via **Block 7 (Cuvée / Blend)**.

### 3.1 Regional Bourgogne AOC (Code `069`) Village Mapping
All regional designations that append a specific village name (e.g., *Bourgogne Chitry*, *Bourgogne Épineuil*, *Bourgogne Côte d'Or*, *Bourgogne Sainte-Marie-la-Blanche*) strictly use the parent Appellation code `069`. 
The application-level parser resolves the specific village sémantically using a 2-letter alpha code in Block 7 managed by the IWIN Central Registry:
* **`CH`** = Chitry (e.g., `FR-2022-BRG-069-W-100245-CH-00750` resolves locally to *Bourgogne Chitry AOC*).
* **`EP`** = Épineuil.
* **`TO`** = Tonnerre.
* **`CO`** = Côte d'Or.
* **`SM`** = Sainte-Marie-la-Blanche.

### 3.2 Mâcon AOC (Code `112`) Geographic Village Mapping
Similarly, the 26 specific village designations authorized to append their name to the Mâcon template (e.g., *Mâcon-Azé*, *Mâcon-Lugny*, *Mâcon-Verzé*) do not receive individual 3-digit numeric codes in Block 4. They all share the parent Appellation code `112`.
Block 7 isolates the geographical village identity:
* **`AZ`** = Azé (e.g., `FR-2024-BRG-112-W-100452-AZ-00750` resolves locally to *Mâcon-Azé AOC*).
* **`LU`** = Lugny.
* **`VE`** = Verzé.

*This sub-mapping configuration compresses over 60 structural entities into just 2 core Appellation keys, maximizing local database execution speed while preserving micro-terroir precision for the consumer.*


---

## 4. France: Languedoc Viticultural Framework & Historical Resolution

The Languedoc region contains massive structural data points due to its historical shift from mass volume production to micro-terroir partitioning and the presence of heavily sub-divided IGP (Indication Géographique Protégée) designations.

### 4.1 Chronological Resolution Rule: Coteaux du Languedoc vs. Languedoc AOC (Code `197`)
The parent regional appellation *Coteaux du Languedoc AOC* was officially updated and modified by regulatory decree into *Languedoc AOC* in 2007.
* **Database Encoding:** All vintages from this overarching regional footprint strictly map to the persistent numeric code `197`.
* **UI Render Rule:** Applications reading an IWIN string containing `LGD-197` must apply a temporal branch test based on Block 2 (Vintage):
  * If **Block 2 (Vintage) < 2007**, the interface MUST display: **"Coteaux du Languedoc AOC"**.
  * If **Block 2 (Vintage) >= 2007**, the interface MUST display: **"Languedoc AOC"**.

### 4.2 IGP Sub-Zone De-duplication (Codes `171`, `192`, `208`)
To prevent dictionary inflation across the major departmental IGPs (Aude, Gard, and Hérault), localized sub-descriptors and community zones are virtualized into **Block 7 (Cuvée / Blend)**. The parent 3-digit numeric code handles core database indexing, while Block 7 isolates the micro-geography sémantically:

* **Aude IGP (Code `171`):** Sub-zones like *Coteaux de la Cabrerisse* or *Val de Cesse* use code `171`. Block 7 assigns two-letter anchors (e.g., `CC`, `VC`).
* **Gard IGP (Code `192`):** Sub-zones like *Coteaux Flaviens* or *Vistrenque* map to code `192`.
* **Pays d'Hérault IGP (Code `207`):** The 16 regional descriptors (e.g., *Mont Baudile*, *Coteaux du Salagou*, *Côtes du Brian*) map strictly to code `208`. An IWIN string matching `LGD-208-X-XXXXXX-SA` resolves locally to *Pays d'Hérault - Coteaux du Salagou IGP*.


---

## 5. France: Loire Valley Viticultural Framework & Multi-Departmental Mapping

The Loire Valley spans a massive geographical footprint. To avoid dictionary bloat within Block 4 (Appellation), the IWIN standard groups regional networks and communal sub-designations into centralized operational parent keys.

### 5.1 Val de Loire IGP (Code `281`) Departmental Resolution
All 14 departmental and sub-regional suffixes attached to the global *Val de Loire IGP* template strictly use the parent Appellation code `281`. Block 7 (Cuvée / Blend) isolates the specific political or cultural sub-zone locally via a 2-letter alpha indicator:
* **`NV`** = Nièvre (e.g., `FR-2024-LRE-281-W-100254-NV-00750` resolves locally to *Val de Loire - Nièvre IGP*).
* **`LA`** = Loire-Atlantique.
* **`VD`** = Vendée.
* **`CH`** = Cher.

### 5.2 Communal AOP Compressors (Codes `242` and `250`)
Communal-level sub-mentions attached to regional volcanic or coastal appellations are similarly virtualized within Block 7:
* **Côtes d'Auvergne AOC (Code `242`):** Sub-zones like *Boudes, Corent, or Châteaugay* use code `242`. Block 7 assigns two-letter anchors (e.g., `BO` for Boudes, `CO` for Corent).
* **Fiefs Vendéens AOC (Code `250`):** Sub-zones like *Brem, Mareuil, Pissotte, or Vix* map to code `250`. An IWIN string matching `LRE-250-X-XXXXXX-MA` resolves locally to *Fiefs Vendéens - Mareuil AOC*.


---

## 6. France: Rhone Valley Viticultural Framework & Communal Sub-mapping

The Rhone Valley relies on a multi-tiered hierarchy. To protect Block 4 from containing dozens of individual keys for communal sub-designations, the IWIN standard groups all 22 named villages under a single parent operational identity.

### 6.1 Côtes du Rhône-Villages with Geographical Names (Code `321`)
Wineries producing village-specific Côtes du Rhône-Villages (e.g., *Plan de Dieu, Sablet, Séguret, Visan, Laudun*) must utilize the unique parent Appellation code `321`. 
The application-level translation engine determines the specific geographic commune locally through a 2-letter alpha indicator mapped inside **Block 7 (Cuvée / Blend)**:
* **`PD`** = Plan de Dieu (e.g., `FR-2024-RHO-321-R-100452-PD-00750` resolves locally to *Côtes du Rhône-Villages Plan de Dieu AOC*).
* **`SA`** = Sablet.
* **`SE`** = Séguret.
* **`VI`** = Visan.
* **`LA`** = Laudun.
* **`CH`** = Chusclan.


---

## 7. France: Roussillon Viticultural Framework & Communal Sub-mapping

The Roussillon region relies on premium dry red crus and historical fortified VDN frameworks. To optimize Block 4, communal sub-designations are centralized into automated operational parent identifiers.

### 7.1 Côtes du Roussillon-Villages with Geographical Names (Code `349`)
Wineries producing village-specific red blends (e.g., *Caramany, Latour de France, Les Aspres, Lesquerde, Tautavel*) must utilize the unique parent Appellation code `349`. 
The localized commune identity is determined through a 2-letter alpha indicator mapped inside **Block 7 (Cuvée / Blend)**:
* **`CA`** = Caramany (e.g., `FR-2024-ROU-349-R-100452-CA-00750` resolves locally to *Côtes du Roussillon-Villages Caramany AOC*).
* **`LT`** = Latour de France.
* **`AS`** = Les Aspres.
* **`LE`** = Lesquerde.
* **`TA`** = Tautavel.


---

## 8. France: South-West Viticultural Framework & IGP Sub-zoning

The South-West region is geographically fragmented and relies on high-volume varietal network maps. To clean database indexes, multi-descriptor regional frameworks are virtualized.

### 8.1 Landes IGP (Code `384`) Territory Resolution
All territorial sub-descriptors attached to the overarching *Landes IGP* framework (e.g., *Coteaux de Chalosse, Côtes de l'Adour, Sables de l'Océan, Sables fauves*) strictly use the core parent Appellation code `384`. 
The application-level translation schema isolates the micro-origin via a 2-letter alpha token mapped inside **Block 7 (Cuvée / Blend)**:
* **`CC`** = Coteaux de Chalosse (e.g., `FR-2025-SUD-384-W-100254-CC-00750` resolves locally to *Landes - Coteaux de Chalosse IGP*).
* **`CA`** = Côtes de l'Adour.
* **`SO`** = Sables de l'Océan.
* **`SF`** = Sables fauves.


---

## 9. France: Savoie and Bugey Viticultural Framework & Cru Virtualization

The Alpine regions feature highly fragmented single-commune crus anchored to regional identities. To prevent database index inflation, all cru extensions use root operational identity schemas.

### 9.1 Vin de Savoie AOC Cru Mapping (Code `409`)
All 16 historically recognized crus (e.g., *Apremont, Abymes, Chignin, Chignin-Bergeron, Arbin*) map strictly to the parent Appellation code `409`. 
The local application parser extracts the specific terroir using a 2-letter alpha identifier in **Block 7 (Cuvée / Blend)**:
* **`AP`** = Apremont (e.g., `FR-2025-SVB-409-W-100254-AP-00750` resolves locally to *Vin de Savoie Apremont AOC*).
* **`AB`** = Abymes.
* **`CH`** = Chignin.
* **`CB`** = Chignin-Bergeron (Exclusively from Chignin Bergeron / Roussanne grape).
* **`AR`** = Arbin (Exclusively red Mondouse).

### 9.2 Roussette de Savoie AOC Cru Mapping (Code `406`)
Similarly, single-vineyard crus for the Altesse grape map to code `406`:
* **`MA`** = Marestel.
* **`FR`** = Frangy.
