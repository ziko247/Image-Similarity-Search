# Recherche d’images par similarité visuelle

Projet académique de recherche d’images par contenu, utilisant le jeu de données Caltech-101. L’objectif est de retrouver les images les plus proches d’une image de requête à partir de leurs caractéristiques visuelles.

## État du projet

- **Disponible :** téléchargement local de Caltech-101 et notebook d’exploration des catégories, des dimensions et de quelques images. Le notebook vérifie également si les images peuvent être décodées.
- **À développer :** prétraitement, extraction des signatures de couleur et de texture, indexation, recherche des images les plus proches et évaluation.

Les choix de paramètres et de distances seront précisés à mesure que les expériences avancent. L’indexation des signatures constitue la phase d’apprentissage prévue ; aucun modèle entraîné n’est encore implémenté.

## Jeu de données

Télécharger [Caltech-101 depuis CaltechDATA](https://data.caltech.edu/records/mzrjq-6wc02), puis extraire `101_ObjectCategories.tar.gz` depuis l’archive téléchargée et placer le dossier `101_ObjectCategories` ainsi :

```text
Project_image_similarity_search/
├── 01_exploration_caltech101.ipynb
├── 02_pretraitement.ipynb
└── data/
    └── 101_ObjectCategories/
        ├── airplanes/
        ├── BACKGROUND_Google/
        └── ...
```

Le dossier `data/` est ignoré par Git : chaque personne doit télécharger les données séparément. La copie locale contient 9 144 images JPEG dans 102 dossiers, dont `BACKGROUND_Google` ; le traitement de ce dernier reste à décider.

## Démarrer l’exploration

Python et un environnement Jupyter sont nécessaires. Depuis un terminal ouvert à la racine du projet :

```bash
python -m pip install notebook
jupyter notebook
```

Ouvrir `01_exploration_caltech101.ipynb`, choisir l’environnement Python où les dépendances ont été installées, puis exécuter les cellules dans l’ordre. Dans VS Code, le notebook peut également être ouvert avec la prise en charge Jupyter et un noyau Python approprié.

Le notebook a été vérifié sur les comptages complets et sur un échantillon d’images ; son exécution interactive complète reste à confirmer.

La premiere cellule du notebook installe Pillow dans le noyau Python selectionne.

Le notebook `02_pretraitement.ipynb` présente le prétraitement d’une image. Ouvrir le fichier depuis la racine du projet et sélectionner le noyau `.venv`.

## Approche envisagée

1. **Prétraitement :** redimensionnement et conversion en HSV.
2. **Extraction :** histogramme de couleur HSV et descripteur global de texture, tel que LBP.
3. **Indexation :** stockage des signatures associées aux images.
4. **Recherche :** comparaison des signatures et affichage des N images les plus proches.

Une évaluation par catégorie et des comparaisons entre couleur seule, texture seule et combinaison sont envisagées. Ces étapes ne sont pas encore implémentées.
## Suivi du projet

Les cases cochées correspondent à un travail réalisé et vérifié. Les options techniques ci-dessous sont des points de départ à confirmer par les expériences.

### 1. Préparer et explorer Caltech-101

- [x] Télécharger la base officielle et extraire les catégories dans `data/`.
- [x] Compter les images et examiner la répartition par catégorie.
- [x] Préparer un notebook d’exploration et tester ses cellules sur les comptages complets et un échantillon d’images.
- [ ] Exécuter le notebook complet dans Jupyter ou VS Code : afficher les exemples, examiner les dimensions et vérifier la lecture de toutes les images.
- [ ] Décider et documenter le traitement de `BACKGROUND_Google` (exclusion proposée pour l’évaluation par catégorie).

### 2. Prétraiter une image

Notebook préparé : `02_pretraitement.ipynb`. Son exécution complète reste à vérifier.

- [ ] Charger une image et vérifier sa lecture.
- [ ] La redimensionner de manière uniforme ; 256 × 256 pixels est une valeur de départ à tester.
- [ ] Produire une version HSV pour la couleur et une version en niveaux de gris pour la texture.
- [ ] Afficher les différentes versions et examiner l’effet du redimensionnement sur les proportions et les textures.

### 3. Extraire les caractéristiques

- [ ] Calculer et afficher un histogramme HSV normalisé pour la couleur.
- [ ] Calculer et afficher un histogramme de texture normalisé ; LBP est le descripteur proposé à évaluer.
- [ ] Vérifier que les signatures ont des dimensions constantes pour toutes les images et que leur normalisation permet une comparaison cohérente.

### 4. Comparer quelques images

- [ ] Comparer séparément les signatures de couleur et de texture sur un petit échantillon.
- [ ] Tester une distance adaptée aux histogrammes normalisés, notamment Bhattacharyya ; comparer éventuellement avec la distance euclidienne.
- [ ] Combiner les distances avec des poids et trier les exemples du plus proche au plus éloigné.
- [ ] Examiner les résultats et choisir expérimentalement la normalisation et les poids.

### 5. Construire l’index de référence

- [ ] Appliquer le même prétraitement et la même extraction à toutes les images retenues.
- [ ] Conserver, pour chaque image, son chemin, sa catégorie et ses signatures de couleur et de texture.
- [ ] Enregistrer l’index pour éviter de recalculer les signatures à chaque recherche.
- [ ] Signaler les fichiers qui ne peuvent pas être lus.

### 6. Rechercher des images similaires

- [ ] Traiter l’image de requête exactement comme les images de la base.
- [ ] Comparer sa signature à celles de l’index et classer les résultats par distance.
- [ ] Afficher les N images les plus proches avec leurs scores.
- [ ] Exclure l’image de requête des résultats lorsqu’elle appartient à la base.

### 7. Évaluer et préparer la démonstration

- [ ] Comparer couleur seule, texture seule et combinaison des deux.
- [ ] Mesurer la précision à 5 et à 10 sur plusieurs requêtes, par catégorie et en moyenne.
- [ ] Séparer les requêtes utilisées pour choisir les poids de celles utilisées pour l’évaluation finale.
- [ ] Analyser des résultats réussis et des erreurs, en rappelant que l’appartenance à une même catégorie ne garantit pas une forte ressemblance visuelle.
- [ ] Décider si une petite interface est utile une fois le pipeline validé.
