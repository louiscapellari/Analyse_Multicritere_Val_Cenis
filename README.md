# Analyse_Multicritere_Val_Cenis

## Objectif
Identifier rapidement les secteurs favorables à l’implantation de nouveaux refuges sur **Val-Cenis** en croisant relief, accessibilité rando/route, isolement vis-à-vis des bâtiments, points-de-vue et contraintes environnementales/risques.  
L'analyse produit une fichier vecteur des zones favorables pour l'implantation d'un nouveau refuge selon ces critères, avec un taux supérieur à 80% de confiance. L'analyse produit aussi un raster noté de 0 à 100 représentant les zones favorables.</br>
Données issues de la base **sig_vc** et du **MNT ign_mnt_2020**.

## Méthode

### Critères reclassés en 0–100
- **Terrain** : pente (°) et altitude (m).  
- **Accessibilité & attractivité** : distances au plus proche **sentier**, **route**, **refuge**, **POI** (points de vue + centres de plans d’eau).  
- **Isolement** : distance aux **bâtiments** (loin = mieux).  
- Chaque distance est convertie en **score 0–100** par seuils simples/progressifs.

### Masques & contraintes
- Vecteurs sensibles (hydro, glaciers, zones humides, PPR, etc.) **buffer 10 m**, fusion, rasterisation **binaire 0/1** (*All touched*).  
- Application au score : zones exclues → **NoData** (ou 0).  
- *Ceinture & bretelles* : en fin de chaîne, **Difference** vectorielle avec l’exclusion 10 m pour garantir le « strictement hors ».

### Combinaison pondérée
- Moyenne pondérée (somme = 1) : **pente 0,225 · altitude 0,135 · sentiers 0,225 · refuges 0,135 · routes 0,090 · POI 0,090 · bâtiments 0,100** → `score_intermediaire`.  
- Application du masque → `score_ajuste`. **Normalisation 0–100** par étirement **Min/Max** (ou **P5/P95**).

### Extraction des zones prioritaires
- **Top 10 %** : `score_final ≥ 90` avec **NoData ailleurs** (évite de polygoniser la classe 0).  
- **Sieve** (≥ 100 px à 10 m = 1 ha), **Polygonize (DN=1)**, **Dissolve**, filtre **≥ 1 ha**.  
- **Assainissement** : **Difference** avec l’exclusion 10 m ; *(option)* retrait **≥ 200 m** des `geo050k_harm_l_struct` via **Difference** avec **buffer 200 m**.  
- **Zonal statistics** (mean/max sur le raster final) et **Point on surface** pour les candidats.

### Sorties
- `score_final_0_100.tif`, `zones_prioritaires_scored.gpkg` (≥ 1 ha, hors contraintes/200 m), `points_candidats.gpkg`.

## Notes & limites
- **Seuils & pondérations** par défaut : à ajuster selon l’expertise locale et la saisonnalité (enneigement, ouverture des itinéraires/POI).  
- La **normalisation 0–100** n’altère pas les rangs relatifs mais dépend des extrêmes observés ; préférer **P5/P95** en cas d’outliers.  
- Le masque **10 m** avec *All touched* est **conservatif** (peut sur-exclure des lisières étroites).  
- Qualité dépendante de la **fraîcheur des données** (bâti/risques/cheminements) et du **calage** (tous rasters en 10 m, emprise identique).  
- Après retraits (exclusion, 200 m structures), **recalculez la surface** et **refiltrez ≥ 1 ha** ; refaites les **zonal stats** pour classer correctement les morceaux restants.  
- Le **Modeler** encapsule tout pour la **reproductibilité** ; documenter les versions **QGIS/GDAL** et les chemins d’entrée.
