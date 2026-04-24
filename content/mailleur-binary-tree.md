---
title: "Mailleur automatique Binary Tree"
description: "Le mailleur propriétaire Conformal Adaptive Binary-tree (CAB) de Simerics — rapide, robuste, tolérant aux géométries sales"
ShowToc: true
TocOpen: false
---

Le **mailleur automatique Simerics** repose sur un algorithme propriétaire — **Conformal Adaptive Binary-tree (CAB)** — intégré à Simerics-MP® et PumpLinx®. Il génère en quelques minutes un maillage volumique à partir de géométries CAO, y compris « sales », et constitue l'un des leviers majeurs de la productivité annoncée par Simerics (typiquement 5 à 10× plus rapide que les codes CFD généralistes).

<a href="/pdf/simerics-france-mailleur-binary-tree.pdf" class="btn-blue-red" target="_blank">📄 Télécharger le white paper (PDF)</a>

---

## Principe

Le mailleur CAB construit une grille **cartésienne hexaédrique** qui recouvre le domaine fluide, puis la **raffine localement par division en deux** (d'où *binary tree*) à l'approche des surfaces. Aux frontières, les cellules sont **découpées pour conformer la grille aux surfaces** du domaine.

![Figure 1 — Maillage binary-tree autour d'un cylindre, avec raffinement à l'approche de la surface](/images/mesh/fig-003.png)

Le résultat est une **grille cartésienne adaptative non structurée conforme au corps**, combinant les avantages du structuré (orthogonalité, faible nombre de cellules) et de l'adaptabilité du non structuré (géométries complexes).

---

## Les 8 forces du binary-tree

1. **Architecture parent-enfant** — structure de données compacte et extensible, empreinte mémoire réduite
2. **Raffinement binaire optimal** — transition fluide entre différentes échelles spatiales
3. **Majorité de cellules cubiques** — orthogonalité et *aspect ratio* optimaux, faible distorsion, erreurs numériques minimales
4. **Entièrement automatisé** — temps de mise en données réduit de façon drastique
5. **Formes complexes** — capacité reconnue à mailler des géométries très détaillées
6. **Transitions continues matériaux** — idéal pour le *conjugate heat transfer* (solide ↔ liquide)
7. **Précision avec moins de cellules** — équivalent à un maillage tétraédrique beaucoup plus dense
8. **Tolérance aux géométries sales** — gère les fichiers CAO non nettoyés (fissures, chevauchements)

---

## Un peu d'histoire : pourquoi le binary-tree ?

L'évolution des approches de maillage CFD :

| Époque | Approche | Limites |
|--------|----------|---------|
| **Années 80** | Maillages structurés / *cell-porosity* | Simplification géométrique forte (marches d'escalier), couches limites mal résolues |
| **Années 90** | *Body-fitted structured* | Mise en données très longue, maillages fortement biaisés pour géométries complexes |
| **Années 2000** | Tétraédrique non structuré | Plus simple mais plus de cellules nécessaires, orthogonalité médiocre, couches limites imparfaites |
| **Années 2010+** | Polyédrique (fusion de tétraèdres) | Nombre de faces par cellule encore élevé, sensibilité à la qualité CAO |
| **Aujourd'hui** | **Binary-tree (CAB) — Simerics** | Combine automatisation, précision, tolérance CAO |

---

## Géométries complexes : capacité reconnue

Le mailleur CAB peut résoudre des géométries très détaillées (dragon de Stanford, pompes multi-étages, sous-capots véhicule à 11 millions de facettes STL non nettoyées) sans intervention utilisateur significative.

![Figure 2 — Forme complexe résolue par binary-tree (dragon de Stanford)](/images/mesh/fig-004.png)

**Application typique** : la Figure 5 du white paper illustre le maillage d'une pompe centrifuge dans un plan de coupe. Dans les zones de forte courbure et de petits détails, la grille est subdivisée et découpée pour conformer à la surface. La couche limite peut être densifiée sur la surface sans explosion du nombre total de cellules.

---

## Cas de validation : pompe centrifuge Wang & Wang (2007)

Un exemple typique de la littérature : **Wang & Wang (2007)** avaient utilisé **750 000 cellules structurées** pour simuler une pompe centrifuge avec un code CFD commercial classique. Simerics a reproduit le même cas avec **environ 390 000 cellules binary-tree** (≈ moitié moins) pour une précision équivalente ou supérieure :

| Débit (m³/h) | 30 | 50 | 60 |
|---|---|---|---|
| **Pression (m) — Expérience** | 23,50 | 20,54 | 18,34 |
| **Pression (m) — Wang & Wang (2007)** | 22,52 | 21,06 | 19,26 |
| **Pression (m) — Simerics (binary-tree)** | 21,82 | 20,44 | 18,05 |
| **Puissance (W) — Expérience** | 2 800 | 3 540 | 3 810 |
| **Puissance (W) — Wang & Wang (2007)** | 2 800 | 3 710 | 4 010 |
| **Puissance (W) — Simerics (binary-tree)** | 2 825 | 3 638 | 3 847 |

**Moitié moins de cellules, précision équivalente.**

---

## Adaptation multi-échelles

Parce que la méthode raffine par facteurs de 2, elle est très efficace pour **transiter des grandes échelles aux petites**. Un circuit de lubrification, par exemple, peut combiner des conduites larges et des passages étroits sans aucun réglage manuel (Figure 8 du white paper).

---

## Conjugate Heat Transfer (CHT)

Le binary-tree produit un **maillage continu à travers les volumes connectés** (solide ↔ liquide), ce qui le rend particulièrement adapté au calcul thermique couplé. Exemple d'application : échangeur de chaleur avec liquides en contre-courant dans des tubes conducteurs.

---

## Couches limites

Une question récurrente : **le binary-tree capture-t-il correctement les effets de couche limite ?** Oui, grâce à :

- Des **fonctions de paroi spéciales**
- Des **termes croisés** adaptés aux cellules à facettes multiples produites par le binary-tree
- Une **densification locale** sur les surfaces critiques, qui se propage naturellement dans la couche limite

Résultats validés par de nombreuses comparaisons expérimentales, notamment sur pompes centrifuges multi-étages et pompes axiales.

---

## Vitesse

Le maillage *overlay + refine* est significativement plus rapide que les techniques traditionnelles. À titre d'exemple, un maillage de **46 millions de cellules** pour un modèle de 11 millions de facettes STL est généré en **2,5 heures** sur un Xeon 5550 double quad-core (configuration 2012), la simulation CFD elle-même ne prenant que 18 heures sur la même machine.

---

## Retours utilisateurs

> *« Simerics a dépassé toutes mes attentes : préparation d'un modèle 3D — 2 heures maximum, 1 heure en moyenne. Pas de problème de création de maillage — toujours généré sans erreur. Calcul du premier point caractéristique en 10 à 30 minutes, soit 10 à 30 fois plus rapide que notre programme principal. Précision sur la pression et l'efficacité : de 1 % à 3 % max dans la plage de fonctionnement. Un excellent programme pour l'ingénieur — rapide et simple. Note globale : 9 sur 10. »*
>
> — *Lead Engineer, HMS Group Russia*

---

## En résumé

Le maillage binary-tree tel qu'implémenté dans Simerics :

- Est **convivial, robuste et rapide**
- Utilise **efficacement** le nombre de cellules et la mémoire
- Gère les **matériaux disparates** (CHT à travers liquides et solides)
- S'adapte à une **large plage d'échelles spatiales**
- **Tolère les géométries « sales »** sans nettoyage CAO préalable
- Est **validé expérimentalement** sur de nombreuses applications industrielles

---

## Références

1. Lowry, S. and Keeton, L.W., *"Space Shuttle Main Engine High Pressure Fuel Pump Aft Platform Seal Cavity Flow Analysis"*, NASA Technical Paper 2685, 1987.
2. Wang and Wang, *"Performance prediction of centrifugal pump based on the method of numerical simulation"*, Fluid Machinery, vol. 35, pp. 9-13, 2007.
3. Ding, H., Visser, F.C., Jiang, Y. and Furmanczyk, M., *"Demonstration and validation of a 3D CFD simulation tool predicting pump performance and cavitation for industrial applications"*, J. Fluids Eng. — Trans ASME, 133(1), 011101, 2011.
4. Ding, H., Visser, F.C., and Jiang, Y., *"A practical approach to speed up NPSHR Predictions of Centrifugal Pumps using CFD Cavitation Models"*, FEDSM2012-72282, 2012.
5. *"Cost cutting with pump performance prediction"*, World Pumps, Juillet/Août 2013 ([worldpumps.com](https://www.worldpumps.com)).

---

→ [Voir la page Produits](/produits) · [Choisir Simerics-MP+](/choisir-simerics) · [Demander un benchmark](/contact)
