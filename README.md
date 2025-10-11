# Analyse Multicritère pour déterminer les secteurs favorables à l'implantation d'un refuge dans la commune de Val-Cenis

## Objectif
Identifier rapidement les secteurs favorables à l’implantation de nouveaux refuges sur la commune de **Val-Cenis** en croisant relief, accessibilité aux chemins de randonées, à la route, isolement vis-à-vis des bâtiments, points-de-vue et contraintes environnementales/risques.</br>  
L'analyse produit un fichier vecteur des zones favorables à l'implantation d'un nouveau refuge selon les critères précedemment cités avec un taux supérieur à 80% de confiance. L'analyse produit également un raster noté de 0 à 100 représentant les zones plus ou moins favorables.</br>
Données issues de la base **sig_vc** et d'un MNT à 1m de résolution issu de la BD Alti de l'IGN.</br>

## Méthodologie

La démarche combine des critères hétérogènes transformés en surfaces continues comparables. À partir du MNT, la pente et l’altitude sont reclassées en scores 0–100 selon des seuils cohérents pour l’implantation d’un refuge. Les autres couches (sentiers, routes, refuges, points d’intérêt) ainsi que l’isolement vis-à-vis des bâtiments sont convertis en distances au plus proche, puis reclassés en scores 0–100 selon que la proximité ou l’éloignement est recherché.</br>

Les contraintes environnementales et les risques sont fusionnés, une zone tampon de 10m est établit en plus par mesure de sécurité minimale, puis rasterisées en masque binaire. Ce masque est appliqué au score combiné pour supprimer toute valeur sur les zones exclues. Les critères reclassés sont agrégés par une moyenne pondérée, puis le résultat est normalisé. Le top 20% est ensuite polygonisé et filtré sur une surface minimale. Enfin, une dernière zone de tampon de 200m est découpée du dernier résultat pour produire le résultat final à partir des risques les plus importants représentés par la couche geo050k_harm_l_struct.</br> 

## Limites
- Seuils & pondérations : Valeurs choisies arbitrairement selon un contexte cohérent, mais pourrait être amélioré par une expertise métier forte. 
- Qualité de l'analyse dépendante de la dernière mise à jour des données ainsi que de leur précision.  
- Technique d'analyse qui pourrait davantage optimisée, et réalisée intégralement en python. 
