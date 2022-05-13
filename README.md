# TIGA

## THINK & DO TANK Sciences, Sociétés et Industrie

**Fiche-action RECHERCHE du projet TIGA de la Métropole de Lyon Saint-Etienne.**

Cette action a pour objectif de déceler, prototyper, expérimenter et valoriser sous un horizon de 3 ans des méthodes et des outils innovants pour la médiation industrielle. Elle vise à identifier des verrous scientifiques et proposer des solutions innovantes (supports matériels, numériques et méthodologiques) pour une interaction fluide et continue entre les acteurs du territoire.
L'action 14 engage une dynamique qui vise à s'inscrire dans la pérennité, au-delà du projet TIGA Lyon Saint-Etienne, pour devenir un laboratoire permanant de cocréation, de recherche-action et d'interactions entre les acteurs du territoire.
L'action 14 repose sur 4 démarches qui s'enrichissent mutuellement et se combinent autour d'un processus commun de production.

---

## Développement

### 3D Tiles

[3D Tiles](https://github.com/CesiumGS/3d-tiles) est un community standard open source, décrit par Cesium et l'OGC. Il a été pensé pour aider à la visualisation massive de contenu géospatial 3D, tout en prenant en compte les aspects de streaming et rendu. Ce standard permet de décrire un _tileset_ : un arbre de tuiles 3D . Chaque tuile contient des modèles 3D auxquels sont associés des données sémantiques. Un tileset permet une organisation spatiale des tuiles, optimisée pour le rendering d'objets 3D urbains, notamment en supportant différentes méthodes de tuilage (K-d tree, octree, etc) mais aussi le concept de _Hierarchical Level Of Detail_. Les niveaux de détail permettent d'alterner entre des géométries plus ou moins détaillées en fonction des besoins, par exemple en affichant des modèles très simplifiés de loin et détaillés lorsqu'on est proche.

Les tuiles peuvent avoir différents formats :

- Batched 3D Model (B3DM) : Modèles 3D hétérogènes.
- Instanced 3D Model (I3DM) : Instances de modèles 3D.
- Point Cloud (PNTS) : Nombre massif de points colorés.
- Composite : Mélange de différents formats.

Dans les outils développés dans le cadre de TIGA, nous utilisons principalement le format B3DM. Ce format décrit les géométries à l'aide du format glTF, libre et facilitant le streaming et rendu de modèles 3D sur le web. Chaque tuile contient un ensemble de _features_ : des modèles 3D représentants par exemple des batiments ou des arbres. Il est possible d'associer des informations specifiques aux features ou à la tuile dans les _Feature Table_ et _Batch Table_.

La méthode utilisée pour créer les 3D Tiles peut avoir un impact direct sur la visualisation des objets. Il est donc nécéssaire de disposer d'outils permettant de tester différentes méthodes de tuilage afin d'optimiser le rendu et la visualisation des modèles 3D depuis différentes sources. Il y a aussi le besoin d'offrir à l'utilisateur des outils lui permettant de créer des 3D Tiles depuis différentes données, en y ajoutant de la couleur, des textures et des niveaux de détail. C'est dans ces objectifs qu'on été développé [Py3DTiles](#py3dtiles) et [Py3DTilers](#py3dtilers).

### Py3DTiles

[Py3DTiles](https://github.com/VCityTeam/py3dtiles/tree/Tiler) est une librairie Python permettant de manipuler les [3D Tiles](#3d-tiles). Originellement développé par [Oslandia](https://gitlab.com/Oslandia/py3dtiles), cette bibliothèque a été enrichie et robustifiée au cours du projet TIGA.

L'objectif des modifications et ajouts est de permettre de produire des 3D Tiles de sorte à ce qu'ils puissent être facilement personnalisés selon les modalités de l'utilisateur puis facilement visualisés avec différents outils. Dans ce but, des développements ont notamment été effectués afin de s'assurer que les 3D Tiles produits soient fidèles à la spécification de l'OGC. Cela permet d'être sûr que les 3D Tiles créés avec Py3DIiles puissent être manipulés par tous les outils suivant la spécification. De plus, l'ajout de la possibilité de créer des extensions avec Py3DTiles permet d'enrichir les 3D Tiles selon les choix de l'utilisateur.

Ajouts:

- Classes permettant de représenter les 3D Tiles en mémoire: Batch Table, Bounding Volume, Tile, Tileset.
- Mécanisme pour ajouter des extensions, ainsi que 2 extensions:
  - Batch Table Hierarchy: introduit la notion de hiérarchie et de typage dans la Batch Table.
  - Extension Temporelle: permet d'ajouter la temporalité dans un tileset.
- Validation de validation des 3D Tiles via schémas JSON.
- Matériaux glTF: permet de créer des matériaux texturés ou colorés.

Modifications:

- Modification de la classe Feature Table pour qu'elle soit utilisable par les 3D Tiles au format B3DM.
- Corrections de l'encodage/décodage des parties binaires des 3D Tiles au format B3DM.
- Faire en sorte que les 3D Tiles produits soient fidèles à la spécification.
- Amélioration de la lecture/écriture des 3D Tiles au format B3DM.

### Py3DTilers

[Py3DTilers](https://github.com/VCityTeam/py3dtilers) est un outil Python open source permettant de créer des [3D Tiles](https://github.com/CesiumGS/3d-tiles) depuis différents formats: [OBJ](https://en.wikipedia.org/wiki/Wavefront_.obj_file), [GeoJSON](https://en.wikipedia.org/wiki/GeoJSON), [IFC](https://en.wikipedia.org/wiki/Industry_Foundation_Classes) et [CityGML](https://en.wikipedia.org/wiki/CityGML) (via une base de donnée [3dCityDB](https://3dcitydb-docs.readthedocs.io/en/release-v4.2.3/).

Py3DTilers utilise la librairie [Py3DTiles](#py3dtiles) pour sa représentation en mémoire des 3D Tiles.

Py3DTilers inclut des _Tilers_, permettant chacun de transformer un format précis de données en 3D Tiles. Ils sont au nombre de 5:

- [ObjTiler](https://github.com/VCityTeam/py3dtilers/tree/master/py3dtilers/ObjTiler#readme): converti des fichiers OBJ en 3D Tiles
- [GeojsonTiler](https://github.com/VCityTeam/py3dtilers/tree/master/py3dtilers/GeojsonTiler#readme): converti des fichiers GeoJSON en 3D Tiles
- [IfcTiler](https://github.com/VCityTeam/py3dtilers/tree/master/py3dtilers/IfcTiler#readme): converti des fichiers IFC en 3D Tiles
- [CityTiler](https://github.com/VCityTeam/py3dtilers/tree/master/py3dtilers/CityTiler#readme): converti des features CityGML (bâtiments, ponts, courts d'eau et relief) extraites d'un base 3DCityDB en 3D Tiles
- [TilesetReader](https://github.com/VCityTeam/py3dtilers/tree/master/py3dtilers/TilesetReader#readme): lit, fusionne et transforme des tilesets 3D Tiles déjà existant

Les Tilers partagent des [options communes](https://github.com/VCityTeam/py3dtilers/tree/master/py3dtilers/Common#common-tiler-features) pour personnaliser la création:

- La création de niveaux de détails
- La reprojection des données
- La translation et mise à l'échelle des modèles 3D
- L'export en modèle OBJ
- La création de 3D Tiles texturés ou colorés

### Docker Py3DTilers

Permet d'utiliser [Py3DTilers](#py3dtilers) via un [docker](https://github.com/VCityTeam/py3dtilers-docker).

Ce programme permet d'utiliser toutes les fonctionnalités de Py3DTilers sans effectuer toutes les installations préalables.

Le docker contient aussi une documentation pour utiliser [3DCityDB avec docker](https://github.com/VCityTeam/py3dtilers-docker#1-start-a-3dcity-database).

### UD-Viz

[UD-Viz](https://github.com/VCityTeam/UD-Viz) est une librairie JavaScript basé sur [iTowns](https://github.com/itowns/itowns). UD-Viz permet de visualiser de la donnée urbaine et d'intéragir avec cette donnée. UD-Viz utilise iTowns pour pouvoir charger et afficher des [3D Tiles](#3d-tiles), des modèles 3D hiérarchisés spatialement.

Dans le cadre du projet TIGA, UD-Viz a été enrichi afin de permettre de nouvelles intéractions avec la donnée urbaine et d'offrir de nouvelles possibilités à l'utilisateur. Ces ajouts concernent en autre une meilleur gestion des couleurs et textures des modèles 3D, ... (**à compléter**).

#### **Couleurs et textures**

UD-Viz intègre une gestion des styles permettant d'appliquer des couleurs aux modèles des 3D Tiles. Ces styles peuvent être appliqués à un modèle unique ou à un ensemble de modèles. Un style arbitraire était appliqué par défaut à tous les 3D Tiles lors du chargement. Le problème avec ce style par défaut est qu'il détruit les matériaux déjà présents dans les modèles, pour en appliquer un avec le style arbitraire. En conséquence, il n'était pas possible de charger des 3D Tiles texturés ou avec des couleurs différentes.

Des modifications ont été apportées à cette gestion des styles afin de laisser à l'utilisateur le choix entre:

- appliquer un style par défaut aux 3D Tiles
- conserver le style déjà présent dans les 3D Tiles

La deuxième option permet un plus grande diversité des styles, puisque ces derniers ne sont plus limités à une couleur unique. De plus, les différentes options de [Py3DTilers](#py3dtilers), l'outil permettant de créer les 3D Tiles, offre beaucoup d'options pour ajouter des couleurs et des textures aux 3D Tiles. Ces couleurs et textures peuvent maintenant être chargés dans UD-Viz.

En plus de ce travail sur le style par défaut des 3D Tiles, des modifications ont été apportées à la gestion du style dans UD-Viz afin de corriger un problème sur les niveaux de détail des modèles. Ces niveaux de détail étaient chargés sans style par défaut, et leur style ne pouvait pas être modifié.

![texture_refine](https://user-images.githubusercontent.com/32875283/168042130-65e3b7a5-14d1-4783-a759-72606a9a1c33.gif)

### Démos UD-Viz

[UD-Viz-Template](https://github.com/VCityTeam/UD-Viz-Template) est un template d'application permettant des créer rapidement des démonstrations basées sur la librairie [UD-Viz](#ud-viz). Ces démonstrations permettent d'illustrer des fonctionnalités et/ou des données particulières.

#### **Démo Py3DTilers**

- [Code](https://github.com/VCityTeam/UD-Demo-vcity-py3dtilers-lyon)
- [Docker](https://github.com/VCityTeam/UD-Demo-vcity-py3dtilers-lyon-docker)
- [Démo en ligne](https://py3dtilers-demo.vcityliris.data.alpha.grandlyon.com/)

Cette démo propose un ensemble de tilesets 3D Tiles créés avec les outils de Py3DTilers. On y retrouve des 3D Tiles de bâtiments, ponts, relief et cours d'eau, certains texturés et d'autres colorés.

![image](https://user-images.githubusercontent.com/32875283/168044197-59741221-5033-4829-b081-b6fcb36261f6.png)

La démo introduit aussi de nouvelles modalités de visualisation des 3D Tiles. Elle implémente notamment un nouvau fonctionnement des niveaux de détails des modèles 3D. Par défaut, le niveau de détails se raffine en fonction du zoom de la camera: plus la camera est proche d'un modèle, plus ce dernier est détaillé. Ici, on offre la possibilité d'utiliser la souri comme une "loupe": les modèles proches de la souri de l'utilisateur sont raffinés afin d'obtenir des modèles plus détaillés sans avoir besoin de bouger la caméra.

![mouse_refine](https://user-images.githubusercontent.com/32875283/168044116-a1af5952-2da6-4420-be6e-3b28909d9bd9.gif)

#### **Démo UI-driven**

- [Code](https://github.com/VCityTeam/UD-Demo-VCity-UI-driven-data-computation-Lyon)
- [Docker](https://github.com/VCityTeam/UD-Demo-VCity-UI-driven-data-computation-Lyon-docker)
- [Démo en ligne](https://ui-driven-data-lyon.vcityliris.data.alpha.grandlyon.com/)

Cette démo permet de calculer la hauteur de routes en les plaçant sur le relief. Pour cela, les routes doivent être contenues dans des fichiers GeoJSON. Ces fichiers peuvent ensuite être glissés/déposés. Les routes seront affichées au fur et à mesure du calcul. Une fois toutes les routes placées sur le relief, de nouveaux fichiers GeoJSON contenant les routes aux bonnes altitudes sont téléchargés.

![ezgif-5-42ca35522d](https://user-images.githubusercontent.com/32875283/165520908-eeda0798-4ca1-4e76-ad3c-6779d593cff3.gif)

#### **Démo [Vallée de la chimie](https://fr.wikipedia.org/wiki/Vall%C3%A9e_de_la_chimie)**

- [Code](https://github.com/VCityTeam/UD-Demo-TIGA-Webdoc-ChemistryValley)
- [Démo en ligne](https://www.derrierelesfumees.com/_Contenusdlf/Carte/index.html)

![chemistryvalley](https://user-images.githubusercontent.com/32339907/168281845-a47fbad9-f3cf-41db-8eb2-0b630ebea659.jpg)


La démo  [Vallée de la chimie](https://fr.wikipedia.org/wiki/Vall%C3%A9e_de_la_chimie) a été développer dans le cadre d'un web-documentaire en collaboration avec [Interfora](), un pôle de formation au métiers de la chimie. L'objectif de cette démo est de représenter le territoire de la [vallée de la chimie](https://fr.wikipedia.org/wiki/Vall%C3%A9e_de_la_chimie) et de le contextualiser à l'aide de document, d'interviews d'acteurs de ce territoire et données urbaines. Ces différents éléments sont disposés à des points d'intêret en lien avec le thème de l'industrie dans cette représentation numérique 3D.

### Autre

---

## Documentations

- [Veille](https://docs.google.com/spreadsheets/d/1WMBi1XcP12ggSY--qWQrSYCXhRfvsycCfQJ3W6OuaC4/edit?usp=sharing) : Veille sur les outils de médiations lié au thème de l'industrie.
- [Mind map](https://docs.google.com/spreadsheets/d/1WMBi1XcP12ggSY--qWQrSYCXhRfvsycCfQJ3W6OuaC4/edit?usp=sharing) : "Min map" donnant une autre approche de visualisation de la veille.
- [Livrable 2020](./Livrables/Action14_UDL_LIRIS_2020.pdf) :
- [Article Py3DTilers](./Livrables/article_py3dtilers.pdf) : Article de recherche sur Py3DTilers et ses fonctionnalités
- [Article intégration de multimedia dans une représentation 3D numérique](./Livrable/Integrating_multimedia_documents_for_augmented_models_to_a_better_understanding_of_its_territory.pdf) : Article de recherche sur l'intégration de multimédia dans une représentation 3D d'un territoire afin d'apporter plus d'information sur celui-ci et mieux le comprendre.

### Documentation 3D Tiles

- [Visualisation des 3D Tiles](https://github.com/VCityTeam/UD-SV/blob/master/ImplementationKnowHow/Visualize3DTiles.md) : comment visualiser des 3D Tiles dans UD-Viz, iTowns et Cesium.
- [Créer des 3D Tiles depuis de l'open data](https://github.com/VCityTeam/UD-Reproducibility/blob/master/Computations/3DTiles/Lyon_Relief_Roads_Buildings_Water/Compute_Lyon_3DTiles.md) : créer des 3D Tiles de bâtiments, relief, cours d'eau, routes et ponts depuis l'open data du Grand Lyon et de l'IGN.

### Documentation PostGIS/3DCityDB

- [Créer et remplir des bases 3DCityDB](https://github.com/VCityTeam/UD-SV/blob/master/ImplementationKnowHow/PostgreSQL_for_cityGML.md) : comment créer et remplir des bases de données 3DCityDB avec des fichiers CityGML.
- [Exporter une base PostGIS](https://github.com/VCityTeam/UD-SV/blob/master/ImplementationKnowHow/Dump_psql_postgis_database.md) : comment exporter une base de données PostGIS en un fichier SQL.

### Documentation calcul de données

- [Calcul d'altitude des routes](https://github.com/VCityTeam/UD-Reproducibility/blob/master/Computations/3DTiles/Lyon_Relief_Roads_Buildings_Water/Roads_from_relief.md) : Calculer les altitudes de routes (au format GeoJSON) à l'aide d'une [démo UD-Viz](#démo-ui-driven).

### Documentation intégration de contenu multi-media

Méthode d'integration de multi-media tel que des fichiers texts, des images, des vidéos ou des vidéos 360. Cette méthode à pour but de contextualiser les modèles 3D de bâtiments dans la librairie UD-Viz.
- [Configuration du fichier JSON]() :
- [Episode visualizer object](https://github.com/VCityTeam/UD-Demo-TIGA-Webdoc-ChemistryValley/blob/main/doc/PinsDoc.md) :

### Documentation couches de données WFS/WMS

* TO-DO : beosin d'une doc dans UD-Viz pour l'integration de couche de données 

## Données

- [Photos de la catastrophe Feyzin](<https://numelyo.bm-lyon.fr/BML:BML_01ICO001015c33b77d0036c?&collection_pid=BML:BML_01ICO00101&luckyStrike=1&query[]=isubjectgeographic:%22Feyzin%20(Rh%C3%B4ne)%22&hitPageSize=1&hitTotal=62&hitStart=24>)
- [Modèle 3D de la métropole de Lyon](https://partage.liris.cnrs.fr/index.php/apps/files/?dir=/VCity/Data/Obj/Metropole%20de%20Lyon&fileid=251305282) : Données générées lors de projet étudiant de master en informatique.
- [Tilesets 3D Tiles de Lyon](https://dataset-dl.liris.cnrs.fr/three-d-tiles-lyon-metropolis/Demo/UD-Demo-vcity-py3dtilers-lyon/) : 3D Tiles générés avec Py3DTilers (bâtiments, relief, routes, ponts, fleuves)

### TO-DO

- Py3dtilers decouper avec la liste des socles de développement.
- Trouver un nom pour les développments techniques.
