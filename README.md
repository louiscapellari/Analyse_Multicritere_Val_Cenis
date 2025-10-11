# Analyse Multicritère pour déterminer les secteurs favorables à l'implantation d'un refuge dans la commune de Val-Cenis

## Objectif
Identifier rapidement les secteurs favorables à l’implantation de nouveaux refuges sur la commune de **Val-Cenis** en croisant relief, accessibilité aux chemins de randonées, à la route, isolement vis-à-vis des bâtiments, points-de-vue et contraintes environnementales/risques.</br>  
L'analyse produit un fichier vecteur des zones favorables à l'implantation d'un nouveau refuge selon les critères précedemment cités avec un taux supérieur à 80% de confiance. L'analyse produit également un raster noté de 0 à 100 représentant les zones plus ou moins favorables.</br>
Données issues de la base **sig_vc** et d'un MNT à 1m de résolution issu de la BD Alti de l'IGN.</br>

## Méthodologie

La démarche combine des critères hétérogènes transformés en surfaces continues comparables. À partir du MNT, la pente et l’altitude sont reclassées en scores 0–100 selon des seuils cohérents pour l’implantation d’un refuge. Les autres couches (sentiers, routes, refuges, points d’intérêt) ainsi que l’isolement vis-à-vis des bâtiments sont convertis en distances au plus proche, puis reclassés en scores 0–100 selon que la proximité ou l’éloignement est recherché.</br>

Les contraintes environnementales et les risques sont fusionnés, une zone tampon de 10m est établit en plus par mesure de sécurité minimale, puis rasterisées en masque binaire. Ce masque est appliqué au score combiné pour supprimer toute valeur sur les zones exclues. Les critères reclassés sont agrégés par une moyenne pondérée, puis le résultat est normalisé. Le top 20% est ensuite polygonisé et filtré sur une surface minimale. Enfin, une dernière zone de tampon de 200m est découpée du dernier résultat pour produire le résultat final à partir des risques les plus importants représentés par la couche geo050k_harm_l_struct.</br> 

## Limites
- Seuils et pondérations : Valeurs choisies arbitrairement selon un contexte cohérent, mais pourrait être amélioré par une expertise métier forte. 
- Qualité de l'analyse dépendante de la dernière mise à jour des données ainsi que de leur précision.
- Un nettoyage des couches temporaires pourrait être fait au fur et à mesure en python.  
- Technique d'analyse qui pourrait davantage optimisée pour réduire les sorties et réalisée intégralement en python éventuellement. 

## Prérequis 
- Disposer d'un MNT sur la commune de Val-Cenis (de préférence le MNT 1m de la BD Alti de l'IGN)
- Disposer de la base de données sig_vc
- Couches requises :
- MNT (de préférence celui de l'IGN)
- val_cenis
- batiment
- refuges
- sentiers_ign
- pt_vue
- routes
- masque_exclu_vc
- geo050k_harm_l_struct

## Instructions 
- Télécharger les fichiers .model3 (format issu du modeler de QGIS).
- Ajoutez les fichiers .model3 à votre QGIS
<img width="558" height="1156" alt="10" src="https://github.com/user-attachments/assets/2365d73c-2c54-475d-9b5a-2df9beed977e" />
- Ajoutez les couches requises à votre projet QGIS (depuis la base de données sig_vc)
<img width="686" height="616" alt="8" src="https://github.com/user-attachments/assets/0e3ce2bb-ced3-4e92-b98e-7eb969ea01e5" />
- Ouvrez le modeler QGIS
<img width="1660" height="314" alt="9" src="https://github.com/user-attachments/assets/8325d7e6-355a-4a11-96c9-a19feb026a8a" />
- Depuis le panneau "Algorithmes", dans la section modèle, ajoutez le premier modèle "1_amc_vc" et paramètrez le comme ci-dessous (les sorties d'algorithmes doivent toutes être remplis): 
<img width="2885" height="1526" alt="11" src="https://github.com/user-attachments/assets/9a923023-5d5e-437f-9647-f5bc064ca5b0" />
<img width="1960" height="1496" alt="1" src="https://github.com/user-attachments/assets/b3e9e223-ea3b-4fb8-9f34-64a57795bc2b" />
- Veillez à bien remplir les chemins des sorties d'algorithmes 
<img width="600" height="696" alt="3" src="https://github.com/user-attachments/assets/c20a5656-cb47-4818-bb79-3cd2c53bbe34" />
- Vous devriez obtenir ceci :
<img width="718" height="972" alt="2" src="https://github.com/user-attachments/assets/ae8bf777-1c9e-4907-8cac-5a90da2a8931" />
- Ajoutez le second modèle "2_amc_vc" et paramètrez le comme ci-dessous (n'oubliez pas de paramétrer la dépendence au premier modèle) :
<img width="1964" height="1502" alt="4" src="https://github.com/user-attachments/assets/1287e448-a05a-4cab-9258-0ac91938e10a" />
- Veillez à bien remplir les chemins des sorties d'algorithmes 
- Ajoutez le troisième et dernier modèle "3_amc_vc" et paramètrez le comme ci-dessous (n'oubliez pas de paramétrer les dépendences aux deux précédents modèles) :
<img width="1958" height="1496" alt="5" src="https://github.com/user-attachments/assets/cba50161-f0a6-43d9-8feb-5196289daa87" />
- **Couches à enregistrer en .gpkg** :
- centroides_grille_vc
- grille100
- grille100_val_cenis
- grille_dist_batiments_2
- grille_dist_pt_vue_2
- grille_100_dist_refuge_2
- grille_100_dist_route_2
- grille_100_dist_sentier_2
- zones_vecteurs
- zones_candidates_finales
- **Couches à enregistrer .tif** :
- pente_vc
- score_alti
- score_pente
- score_batiments
- score_pt_vue
- score_refuge
- score_route
- score_sentier
- score_final
- score_final_exclu
- score_intermediaire
- zones_sup_80
- Vous devriez finalement obtenir un modèle comme ci-dessous :
<img width="1724" height="926" alt="6" src="https://github.com/user-attachments/assets/e0ecdc93-2477-4e36-9f41-7de5fa610a43" />
- Exécutez le modèle

## Résultats 
- Les seuls résultats qui nous intéressent en sortie d'analyse sont les couches "zones_candidates_finales.gpkg" et , le reste peut-être supprimé.
- Vous devriez obtenir le résultat suivant : 
<img width="2312" height="1221" alt="7" src="https://github.com/user-attachments/assets/face1b41-999b-4315-be17-d9632579be0b" />

