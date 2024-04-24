---
title: "Cartographie 2024"
author: "Romain Loup"
date: "2024-04-10"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## Qu'est-ce qu'une carte ?

« *Une **carte** est une image simplifiée et codifiée de l'espace géographique, qui représente ses caractéristiques et/ou son organisation. Elle résulte de l'acte créateur et des choix de son auteur.* » (Lambert & Zanin, 2016)

### Il existe deux types de cartes :

1.  Les cartes **topographiques**. Essentiellement des observations directes : reliefs, cours d'eau, aménagements humains. Il s'agit d'éléments concrets, **fixes** et **durables** observés à un moment donné.
2.  Les cartes **thématiques**. Il s'agit ici de représenter des phénomènes localisables, **quantitatifs** ou **qualitatifs** de toute nature. C'est essentiellement ce type de carte qui nous intéresse en statistiques.

### Pourquoi des cartes ?

Elles permettent de représenter **spatialement** l'information, de la visualiser et de l'analyser.

![Carte Dufour de 1845 © [SwissTopo](https://www.swisstopo.admin.ch/fr/connaissances-faits/histoire-collections/cartes-historiques/carte-dufour/carte-dufour-patrimoine.html)](images/dufour.jpg)

## Construire une carte

Il y a plusieurs moyens de construire une carte par ordinateur. Il faut donc d'abord définir l'**objectif** de la carte. Si celle-ci est **statique**, il suffit de la constuire dans un logiciel SIG (Système d'Information Géographique) comme [QGis](https://www.qgis.org/fr/site/). Sinon, il est possible de concevoir une carte **dynamique** en utilisant JavaScript (avec comme structure des balises html) et la librairie *open-source* [Leaflet](https://leafletjs.com).

Pour des analyses statistiques, il est possible de construire ces deux types de cartes à l'aide de **R** sans notions de programmation avancées ni avoir à prendre en mains un logiciel SIG.

## Formats de données SIG
Le format de données SIG est une représentation informatique de l'information géographique dont deux pricipaux types de modèles de données:
-   **vectoriel**
-   **matriciel**

### Vectoriel
Les données vectorielles représentent des entités géographiques sous forme de **points**, **lignes** ou **polygones** ([*Plus d'infos sur ESRI*](https://resources.arcgis.com/fr/help/getting-started/articles/026n0000000n000000.htm#ESRI_SECTION1_FECAF3B77D5144EA9C43FA0E9F17AB15)). Elles sont caractérisées par leur précision et leur adaptabilité à différentes échelles de carte. Ces primitives doivent être stockées selon la **latitude** (axe X) et la **longitude** (axe Y) selon un système de **projection** (expliqué plus loin). L'**altitude** (axe Z) n'est pas obligatoire, cette information doit parfois être supprimée suivant le type de carte choisi. A cela peuvent être ajoutées des **métadonnées**, qui contiennent les informations sur les fichiers sous forme de table d'attributs, comme par exemple le nombre d'habitants, la langue parlée ou encore la monnaie d'une région. Ces données sont appélées de *couches* et plusieurs de ces couches peuvent être affichées en même temps sur une carte.
Les formats de données vectorielles les plus courants incluent les fichiers Shapefile (.shp), GeoPackage (.gpkg) ou GeoJSON (.geojson) par exemple.

#### Fichier de format *Shapefile*
Un *Shapefile* (*fichier de formes* en français, nom jamais utilisé...) est un format de "couches" de fichiers qui stocke les données des informations géométriques (attributs géométriques) nécessaires à créer une carte, inventé par [ESRI](https://desktop.arcgis.com/fr/arcmap/10.3/manage-data/shapefiles/what-is-a-shapefile.htm). Ce type de fichier est très répendu dans la création de cartes.

Il est encore à noter que ce format contient 5 fichiers, reconnaissables par les extensions `.shp`, `.shx`, `.dbx`, `.cpg` et `.prj`.

![Ville de Toulouse, à partir de shapefiles [© GEOFABRIK](https://www.geofabrik.de/en/data/shapefiles.html)](images/shapefiles_toulouse.png){width="75%"}

### Matriciel (raster)
Les données matricielles, également connues sous le nom de données raster, sont organisées sous forme de grille régulière de cellules, chaque cellule contenant une valeur ou une classe. Elles sont idéales pour représenter des données continues telles que l'altitude, la température ou la couverture terrestre sous forme d'images aériennes. Les formats de données raster populaires incluent GeoTIFF (.tif) et les fichiers de grille ESRI (.grd).

![Image aérienne du campus de l'Unil/EPFL [Données: © CNES, Spot Image, swisstopo, NPOC](https://map.geo.admin.ch/?lang=fr&topic=ech&bgLayer=ch.swisstopo.swissimage&layers=ch.swisstopo.zeitreihen,ch.bfs.gebaeude_wohnungs_register,ch.bav.haltestellen-oev,ch.swisstopo.swisstlm3d-wanderwege,ch.astra.wanderland-sperrungen_umleitungen&layers_opacity=1,1,1,0.8,0.8&layers_visibility=false,false,false,false,false&layers_timestamp=18641231,,,,)](images/swisstopo.png){width="75%"}

### Autres formats de fichier cartographique
En plus des formats mentionnés ci-dessus, d'autres formats de fichier cartographique sont couramment utilisés dans les SIG et les applications de cartographie. Parmi eux, nous trouvons le GeoPackage (GPKG), un format de base de données géospatiale portable et open source; le GeoJSON, un format de données géospatiales basé sur JSON; et le KML (Keyhole Markup Language), un format XML utilisé pour représenter des données géospatiales dans Google Earth et d'autres applications cartographiques.
En utilisant une combinaison de ces formats de données, il est possible créer des visualisations riches et informatives de l'information spatiale, permettant une meilleure compréhension et analyse de notre environnement géographique.

### Fichiers de *format texte* et *bases de données*
En plus des *Shapefiles*, il est possible d'ajouter d'autres données à la carte, en utilisant des sources de données externes comme des fichiers en format texte comme le *CSV* (.csv), *JSON* (.json), ou autres formats texte ou encore des bases de données. Il suffit ensuite de joindre ces données grâce à un identifiant commun dans R.

### Télécharger des données SIG pour créer une carte
Ce type de données est souvent en libre accès. Il est possible d'obtenir toutes sortes de données comme par exemple les frontières cantonales ou communales de la Suisse. Disponible à ce lien, les [couches de différents niveaux géographique de la Suisse](https://www.bfs.admin.ch/bfs/fr/home/statistiques/statistique-regions/fonds-cartes/niveaux-geographiques.html) permettent de créer des cartes. Nous allons ici utiliser les couches des limites communales suisses ainsi que celles des principaux lacs.

Sources de données

-   [SwissTopo](https://shop.swisstopo.admin.ch/fr/products/free_geodata) : Suisse
-   [GEOFABRIK](https://download.geofabrik.de) : Monde
-   [StatSilk](https://www.statsilk.com/maps/download-free-shapefile-maps) : regroupe des sites comportant des *shapefiles*

## Premiers pas et cartes statiques

*Note pour le tutoriel : il est normalement possible d'exécuter les lignes de commandes sans adapter le code, il faut cependant installer les divers packages utilisés si ce n'est pas déjà fait. A noter aussi que chaque package utilisé est chargé ici au fur et à mesure, mais par convention, les packages sont habituellement chargés au tout début du script.*

Initialement, les *shapefiles* devraient être exploités avec un logiciel de SIG, mais il est possible de les ouvrir dans R grâce au package `sf` (*Simple Features*). `sf` permet de lire les données géospatiales dans R. Il existe d'autres packages dévolus à cette fonction comme `rgdal`.

Le processus est le suivant :

Placer ce fichier R markdown dans un dossier depuis lequel travailler et commencer à exécuter les commandes. Il est important de préparer les données avant de commencer.

```{R, eval=F, echo=T}
# Adapter le répertoire de travail là où se trouve le document R markdown
setwd(dirname(rstudioapi::getActiveDocumentContext()$path))

# Créer un dossier nommé "geoData" (à faire une seule fois)
dir.create("geoData")

# Télécharger les données "Swissboundaries 3d" sur le site de swisstopo et décompresser le dossier
url <- "https://data.geo.admin.ch/ch.swisstopo.swissboundaries3d/swissboundaries3d_2024-01/swissboundaries3d_2024-01_2056_5728.shp.zip" # source des données
download.file(url, "geoData/swissboundaries3d.zip") # téléchargement
unzip("geoData/swissboundaries3d.zip", exdir="geoData/swissboundaries3d") # extraire les fichiers
file.remove("geoData/swissboundaries3d.zip") # supprimer le fichier d'archive
```

### Ouverture d'un shapefile dans R

`st_read` permet de lire les données, ne pas oublier l'extension `.shp` à la fin du nom du fichier.

```{R}
library("sf") # installer chaque package si ce n'est pas déjà fait
communesCH <- st_read("geoData/swissboundaries3d/swissBOUNDARIES3D_1_5_TLM_HOHEITSGEBIET.shp")
communesCH <- communesCH[which(communesCH$BFS_NUMMER < 7000),] # ne garder que les communes suisses (n° OFS s'arrêtent à 7000)
```

Voici un exemple de fichier shapefile dans R. La commande `head(communesCH,3)` permet de visualiser les trois premières lignes des données contenues dans le fichier `communesCH`.

```{R}
head(communesCH,3)
```

La colonne `geometry` contient les données de géométrie, soit les coordonnées des points, lignes, polygones de chaque couche, importée par le package `sf`, accessible ici par `communesCH$geometry`. La fonction `plot` est issue du package de base de R.

```{R}
plot(communesCH$geometry)
```

Cette couche de données comporte une troisième dimension "Z" qui ne sera pas utile pour nos analyses et production de cartes. Afin d'éviter des problèmes de projection, il est préférable de supprimer cette dimension avec la fonction `st_zm()`.

```{R}
communesCH <- st_zm(communesCH, drop = T, what = "ZM")
head(communesCH,1)
```
`Dimension:     XY` montre que la dimension `Z` a été supprimée

`summary` résume les informations contenues dans chaque colonne d'un *simple feature* et donnes des statistiques de base

```{R}
summary(communesCH)
```

#### Ajout d'une deuxième couche: les lacs suisses

Télécharger directement depuis GitHub la couche simplifiée des lacs suisses, provenant à la base de swisstopo.

```{R}
url_2 <- "https://github.com/romainloup/R_carto/raw/main/lacs.zip"
download.file(url_2, "geoData/lacs.zip") # téléchargement
unzip("geoData/lacs.zip", exdir="geoData") # extraire les fichiers
file.remove("geoData/lacs.zip") # supprimer le fichier d'archive

lacs <- st_read("geoData/lacs/lacs.shp")
```

#### Système de coordonnées et de projection

*« Les données sont définies dans des systèmes de coordonnées horizontales et verticales. Les systèmes de coordonnées horizontales localisent les données sur la surface de la Terre, et les systèmes de coordonnées verticales les localisent par rapport à la hauteur ou la profondeur des données. »* [Définition ESRI](https://pro.arcgis.com/fr/pro-app/2.8/help/mapping/properties/coordinate-systems-and-projections.htm)

Il est important vérifier quel est le système de projection utilisé. La projection cartographique est un ensemble de techniques géodésiques permettant de représenter une surface non plane (surface de la Terre, d'un autre corps céleste, du ciel, ...) dans son ensemble ou en partie sur la surface plane d'une carte ([Wikipédia](https://fr.wikipedia.org/wiki/Projection_cartographique)).

Il s'agit, de façon générale, de cartographier au mieux la réalité, en adaptant la surface *ellipsoïdale* de la terre à une représentation *plane*, celle d'une carte.

Plusieurs systèmes de projection sont disponibles pour s'adapter de manière optimale à différentes zones du globe terrestre. Grâce au package `sf`, il est possible de déterminer le système de projection utilisé par les données. La commande `st_crs(communesCH)` (ci-dessous) permet de consulter le système de projection utilisé par les données `communesCH`.

```{R}
st_crs(communesCH)
```
```{R}
st_crs(lacs)
```

Il est à noter que le système de projection pour la couche des lacs est différent que celui des communes. Si deux couches n'ont pas le même système de projection, il n'est pas possible de les combiner. Les deux couches peuvent être affichées mais ne seront pas superposées.

Il est alors possible d'uniformiser le système de projection (`crs`) avec la fonction `st_transform()` :

```{R}
communesCH <- st_transform(communesCH, crs = 2056)
lacs <- st_transform(lacs, crs = 2056)

st_crs(communesCH)
```

`4326`correspond au système de projection mondial (EPSG Geodetic Parameter Dataset) [WGS84](https://epsg.io/4326). Il est possible de trouver des informations sur les différents systèmes de projections [ici](https://epsg.io). Le système de projection suisse [CH1903+](https://epsg.io/2056) est l'EPSG 2056. `5728` correpsond au système de projection suisse CH1903+ pour [l'altitude](https://epsg.io/5728).

### Premières cartes : communes de Suisse et ses lacs

Les cartes ci-dessous sont obtenues grâce aux *shapefiles* téléchargés précédemment. Il est possible de superposer des couches et de leur donner une certaine esthétique.

```{R}
par(mar=c(0,0,0,0)) # marges initialisées à 0
plot(communesCH$geometry, col="grey", bg="#f2f2f2", lwd=0.25, border="#FFFFFF") # communes de Suisse, couleur grise, sur fond gris clair (#f2f2f2), marges à 0.
plot(lacs$geometry, col="lightblue", bg="#f2f2f2", lwd=0.25, border=F, add=T) # pas de bordure et ajoutée à la couche précédente (add=T)
```

`col` : couleur du fond des polygones

`bg` : *background*, couleur du fond de la carte

`lwd` : épaisseur des lignes de séparation

`border` : couleur de la bordure

`add = T` : permet d'ajouter une deuxième couche au graphique/à la carte déjà créé

### Cartographier un phénomène

Grâce à ces premiers outils, il est possible de réaliser la cartographie de phénomènes tels que les résultats des votations fédérales par commune.

Charger le fichier `.json` avec les résultats de l'initiative populaire «Mieux vivre à la retraite (initiative pour une 13e rente AVS)».

```{R}
library("jsonlite")
json_data <- fromJSON("https://app-prod-static-voteinfo.s3.eu-central-1.amazonaws.com/v1/ogd/sd-t-17-02-20240303-eidgAbstimmung.json")
# Titre votation
# json_data$schweiz$vorlagen[[2]]$vorlagenTitel[[2]]
json_data$schweiz$vorlagen$vorlagenTitel[[1]]
```

```{R}
votation <- data.frame(
  noCommune = numeric(),
  nomCommune = character(),
  ouiPourcentCommune = numeric(),
  ouiCommune = numeric(),
  nbBulletins = numeric(),

  stringsAsFactors = FALSE
)

# Récupérer les différentes données dans le JSON
for (i in 1:length(json_data$schweiz$vorlagen$kantone[[1]]$gemeinden)) {
  noCommune = as.numeric(json_data$schweiz$vorlagen$kantone[[1]]$gemeinden[[i]]$geoLevelnummer)
  nomCommune = json_data$schweiz$vorlagen$kantone[[1]]$gemeinden[[i]]$geoLevelname
  ouiPourcentCommune = json_data$schweiz$vorlagen$kantone[[1]]$gemeinden[[i]]$resultat$jaStimmenInProzent
  ouiCommune = json_data$schweiz$vorlagen$kantone[[1]]$gemeinden[[i]]$resultat$jaStimmenAbsolut
  nbBulletins = json_data$schweiz$vorlagen$kantone[[1]]$gemeinden[[i]]$resultat$gueltigeStimmen
  
  # Add the values to the data frame
  votation <- rbind(votation, data.frame(
    noCommune = noCommune,
    nomCommune = nomCommune,
    ouiPourcentCommune = ouiPourcentCommune,
    ouiCommune = ouiCommune,
    nbBulletins = nbBulletins
  ))
}

```

Pour la commune n°1

-   `gebietAusgezaehlt`: Zone comptabilisée (Valeur: TRUE)
-   `jaStimmenInProzent`: Pourcentage de votes "oui" (Valeur: 35.86498)
-   `jaStimmenAbsolut`: Nombre absolu de votes "oui" (Valeur: 340)
-   `neinStimmenAbsolut`: Nombre absolu de votes "non" (Valeur: 608)
-   `stimmbeteiligungInProzent`: Taux de participation en pourcentage (Valeur: 69.44848)
-   `eingelegteStimmzettel`: Nombre de bulletins de vote déposés (Valeur: 957)
-   `anzahlStimmberechtigte`: Nombre d'électeurs inscrits (Valeur: 1378)
-   `gueltigeStimmen`: Nombre de votes valides (Valeur: 948)

Pour lire un fichier `.csv` : `fichierCSV <- read.csv("fichierExemple.csv", header = TRUE)`

`header = TRUE` : lorsque le fichier comprend une entête

#### Jointure de deux fichiers

Renommer l'identifiant pour faire une jointure entre le fihier des géométries et le fichier texte à l'aide du package `dplyr`qui permet de manipuler les jeux de données. C'est un outil pour la manipulation de data frame qui permet par exemple de renommer des colonnes.

```{R}
library("dplyr")
communesCH <- rename(communesCH, noCommune=BFS_NUMMER)
names(communesCH)
```

Le nom de `BFS_NUMMER`est bien devenu `noCommune`.

Assembler (jointure) les deux fichiers en un nouveau fichier nommé `communesVotation`. Toutes les données relatives à la votation seront disponibles dans le fichier des communes. Il faut par contre qu'il y ait un élément de jointure commun (ou le spécifier).

```{R}
communesVotation <- merge(communesCH, votation,by='noCommune')
names(communesVotation)
```

### Manipuler des données, communes par canton

La prochaine étape consiste à ajouter des couleurs à la première carte construite. Pour sélectionner des couleurs appropriées pour la cartographie, vous pouvez utiliser un site spécialisé appelé [ColoBrewer](https://colorbrewer2.org/#type=sequential&scheme=BuGn&n=3). Ce site propose des palettes de couleurs soigneusement choisies, garantissant une bonne visibilité pour les daltoniens, une restitution fidèle sur écran, et une lisibilité optimale à l'impression, évitant ainsi les choix inadaptés.

`ColorBrewer` est également disponible sous forme de package R. Vous trouverez plus d'informations à ce sujet en suivant ce [lien](https://www.datanovia.com/en/fr/blog/palette-de-couleurs-rcolorbrewer-de-a-a-z/). Toutes les palettes de couleurs disponibles peuvent être affichées en utilisant la commande `display.brewer.all()`.

```{R}
library("RColorBrewer")
display.brewer.all()
```

`brewer.pal(n, "palette_name")` permet de sortir plusieurs couleurs qui se basent sur une palette de couleur offerte par ColorBrewer. `n` est le nombre de couleurs à sortir et `"palette_name"` est le nom de la palette utilisée. Malheureusement, il n'y a que 12 couleurs dans la palette catégorielle `Set3`, mais il est possible d'interpoler un set de couleur pour en dériver des nouvelles.

```{R}
library("RColorBrewer")
# Palette de 26 couleurs
my_colors <- brewer.pal(12, "Set3") # 12 couleurs (certaines palettes n'offrent pas plus de couleurs)
# ??brewer.pal # exécuter
my_colors <- colorRampPalette(my_colors)(26) # cette commande permet d'ajouter des couleurs, soit les 26 couleurs nécessaires pour 26 cantons suisses (en format hex)
my_colors # 26 couleurs dans la même palette sont disponibles
```

Création d'une carte des communes suisses, par cantons avec les principaux lacs

```{R}
# Attribue une couleur appropriée pour chaque canton en utilisant le numéro de canton
class_of_canton <- cut(as.numeric(communesCH$KANTONSNUM), 26) # cut divise les communes suivant les 26 numéros de cantons
my_colors_communes <- my_colors[as.numeric(class_of_canton)] # attribue une couleur à chaque canton

# Afficher la carte
par(mar=c(0,0,0,0))
plot(communesCH$geometry, col=my_colors_communes ,  bg = "#f2f2f2", lwd=0.1, border="#FFFFFF")
plot(lacs$geometry, col="lightblue", bg="#f2f2f2", lwd=0.1, border= F, add = T)
```

En cartographie et en visualisation, il n'est pas conseillé d'utiliser 26 couleurs pour une seule carte. Le [théorème des quatre couleurs](https://fr.wikipedia.org/wiki/Théorème_des_quatre_couleurs) indique qu'il est possible, en n'utilisant que quatre couleurs différentes, de colorier n'importe quelle carte, ce qui rend la visualisation bien plus légère.

### Aggréger des géométries

Il est possible d'agréger des géométires (points, lignes, polygones) pour passer d'un découpage fin à un découpage plus large, pour passer par exemple d'une couche de communes à une couche de cantons avec la fonction `group_by()` en ayant un identifiant commun pour chaque commune (ici le numéro de canton). La population de chaque canton qui est présente dans une colonne, est sommée à partir de la population des communes dans la fonction `summarise`. Le nom de chaque canton est ensuite ajouté.

```{R}
cantonsCH <- communesCH %>% 
  group_by(KANTONSNUM) %>% 
  summarise(population = sum(EINWOHNERZ, na.rm = TRUE)) # na.rm = TRUE pour ignorer les valeurs NA

# Récupération du nom du canton dans le fichier json
cantonsCH$nomCanton <- json_data$schweiz$vorlagen$kantone[[1]]$geoLevelname

plot(cantonsCH$geometry, col=unique(my_colors_communes)) # couleurs réutilisées de la carte précédente
```

`unique()`: retourne un vecteur supprimant les éléments dupliqués

### Séparer des jeux de données

Il est possible de ne sélectionner qu'une seule partie d'un jeu de données. Par exemple, il est possible de choisir un seul canton parmi tous les cantons suisses.

La commande `subset` permet de sélectionner une partie précise des données en utilisant une requête logique. Dans ce cas particulier, elle peut être utilisée pour ne retenir que les données relatives aux cantons suisses.

```{R}
# Ajout du nom de chaque canton dans "communesVotation"
communesVotation <- inner_join(communesVotation, (as.data.frame(cantonsCH)[, c(1,4)]), by = c("KANTONSNUM" = "KANTONSNUM"))

jura <- subset(communesVotation, communesVotation$KANTONSNUM =="26") # en sachant que le n°26 correspond au Jura
vaud <- subset(communesVotation, communesVotation$nomCanton =="Vaud") # autre solution avec le nom du canton
```

Il est alors possible de n'afficher qu'un seul canton comme le montre l'illustration ci-dessous :

```{R}
plot(jura$geometry)
```

La carte suivante représente la population des communes vaudoises, divisée en trois classes : 0 à 5'000 habitants, 5'001 à 10'000 habitants, 10'001 à 1'000'000 habitants. ``` cut``` divise la colonne ```vaud\$population`en intervalles choisi par`breaks\`\`\`.

```{R}
my_colors2 <- brewer.pal(3, "YlOrRd") # choix d'une palette de couleur

# Création de classes de population
class_of_pop <- cut(vaud$EINWOHNERZ, breaks = c(1000000, 10000, 5000, 0))

my_colors2 <- my_colors2[as.numeric(class_of_pop)]

par(mar=c(0,0,0,0))
plot(vaud$geometry, col=my_colors2,  bg = "#f2f2f2", lwd=0.5, border="#FFFFFF")
plot(lacs$geometry[which(lacs$NAME=="Lac Léman")], col="#D6E2E7", lwd=0.5, border="#FFFFFF", add=T)
```

## GGplot

`GGplot2`[^1] est une librairie de visualisation de données. Elle est utilisée pour la création de graphiques "*declarative*". Elle est développée selon les principes développés par Leland Wilkinson dans son ouvrage "*The Grammar of Graphics*".

[^1]: Pour plus d'info sur GGplot et le `pipe`, voir *Cours R 2 : le tidyverse* de MQ4 donné par G.Guex

### Viridis

Tout comme ColorBrewer, le package [Viridis](https://cran.r-project.org/web/packages/viridis/vignettes/intro-to-viridis.html) contient des échelles de couleurs pour rendre les cartes et graphiques plus jolis, plus facile à lire pour les daltoniens. Il est aussi possible de trouver des échelles de couleurs qui s'adaptent bien à l'écran où qui sont adaptés aux impressions. Ici une carte choroplète (données relatives).

```{R}
library("ggplot2")
library("ggsn") # complément à l'utilisation de ggplot, permettant de gérer
# l'échelle et l'orientation grâce à l'ajout d'une flèche du nord
library("viridis")

# Projection mondiale WGS84
communesVotationWGS84 <- st_transform(communesVotation, crs=4326)

ggplot() +
  geom_sf(data=communesVotationWGS84, aes(fill=ouiPourcentCommune), size=0, alpha=0.9) +
  geom_sf(data=lacs, fill="#cecece", size=0, alpha=0.9) +
  theme_void() +
  scale_fill_viridis(
    breaks=c(20,40,50,60,80),
    name="Oui (%)",
    guide=guide_legend(keyheight=unit(4, units="mm"), keywidth=unit(10, units="mm"), label.position ="right", title.position='top', nrow=5, reverse=TRUE)) +
  labs(
    title="« Mieux vivre à la retraite (initiative pour une 13e rente AVS) », mars 2024",
    subtitle="Pourcentage de oui",
    caption="Données : © OFS, 2024 | Auteur : Romain Loup"
  ) +
  theme(
    text=element_text(color="#22211d"),
    plot.background = element_rect(fill="#f5f5f2", color=NA),
    panel.background = element_rect(fill = "#f5f5f2", color=NA),
    legend.background = element_rect(fill = "#f5f5f2", color=NA),
    plot.title = element_text(size=16, hjust=0.01, color ="#4e4d47", margin=margin(b=-0.1, t=0.4, l=2, unit="cm")),
    plot.subtitle = element_text(size=14, hjust=0.01, color = "#4e4d47", margin = margin(b=-0.1, t=0.43, l=2, unit="cm")),
    plot.caption = element_text(size=7, color="#4e4d47", margin=margin(b=0.1, unit="cm")),
    legend.position.inside = c(0.9, 0.8)
  ) +
  north(communesVotationWGS84, symbol=11, location="bottomleft") +
  scalebar(communesVotationWGS84, dist=30, dist_unit="km", st.size=3, transform=TRUE, model="WGS84", border.size=0.5, height=0.01, box.fill = c("#4e4d47", "#f5f5f2"), st.color="#4e4d47", box.color="#4e4d47")
```

-   `geom_sf` : Affiche des géométrie de type *simple features*. Cette commande peut afficher des points, lignes ou polygones. -\> [description](https://ggplot2.tidyverse.org/reference/ggsf.html)
    -   `aes` : Coordonnées X, Y -\> [description](https://rdrr.io/cran/ggplot2/man/aes.html)
-   `theme_void()` : Thème vide. -\> [description](https://ggplot2.tidyverse.org/reference/ggtheme.html)
-   `scale_fill_viridis` : Thème de couleurs. -\> [description](https://cran.r-project.org/web/packages/viridis/vignettes/intro-to-viridis.html)
    -   `breaks` : ici 5 ruptures entre les couleurs selon les données cartographiées (% de oui)
-   `scalebar` : Permet d'ajouter une échelle et une flèche du nord. Fonctionne avec le complément `ggsn` à `ggplot` -\> [description](https://www.rdocumentation.org/packages/ggsn/versions/0.5.0/topics/scalebar)
-   `labs` et `theme` : pour ce qui est des titres, sous-titres ainsi que leurs styles
-   `scalebar` : propiété de l'échelle (issue du package `ggsn`)
-   `north` : flèche du nord, `symbol` 1 à 18 pour le style de la flèche (issue du package `ggsn`)

### Elements (obligatoires) à afficher sur une carte thématique

-   Titre et sous-titre
-   Légende
-   Echelle (flèche du nord obligatoire uniquement si la carte n'est pas orientée au nord)
-   Sources des données
-   Auteur·e·s
-   Dates (données, fond et de création))

A noter que pour la lisibilité d'une carte *thématique* (et non pour une carte *topographique*), une bonne pratique est de ne *pas* représenter les lacs en bleu afin de concenter l'attention des lecteurs sur l'information principale qui est ici une représentation d'une votation fédérale.

#### Deuxième exemple

```{R}
# Carte n°2
ggplot() +
  geom_sf(
    data = communesVotationWGS84,
    aes(fill = ouiPourcentCommune),
    size = 0,
    alpha = 0.9
  ) +
  geom_sf(
    data = lacs,
    fill = "#CECECE",
    size = 0,
    alpha = 0.9
  ) +
  theme_void() +
  scale_colour_gradient2(
    low = "#d7191c",
    mid = "#ffffbf",
    high = "#2c7bb6",
    midpoint = 50,
    space = "Lab",
    na.value = "grey80",
    guide = guide_legend(
      keyheight = unit(4, units = "mm"),
      keywidth = unit(10, units = "mm"),
      label.position = "right",
      title.position = 'top',
      reverse = TRUE
    ),
    aesthetics = "fill",
    name = "Oui (%)"
  ) +
  labs(title = "« Mieux vivre à la retraite (initiative pour une 13e rente AVS) », mars 2024",
       subtitle = "Pourcentage de oui",
       caption = "Données : © OFS, 2024 | Auteur : Romain Loup") +
  theme(
    text = element_text(color = "#22211d"),
    plot.background = element_rect(fill = "#f5f5f2", color = NA),
    panel.background = element_rect(fill = "#f5f5f2", color = NA),
    legend.background = element_rect(fill = "#f5f5f2", color = NA),
    plot.title = element_text(
      size = 14,
      hjust = 0.01,
      color = "#4e4d47",
      margin = margin(
        b = -0.1,
        t = 0.4,
        l = 2,
        unit = "cm"
      )
    ),
    plot.subtitle = element_text(
      size = 12,
      hjust = 0.01,
      color = "#4e4d47",
      margin = margin(
        b = -0.1,
        t = 0.43,
        l = 2,
        unit = "cm"
      )
    ),
    plot.caption = element_text(
      size = 7,
      color = "#4e4d47",
      margin = margin(b = 0.1, unit = "cm")
    ),
    legend.position.inside = c(0.9, 0.8)
  ) +
  scalebar(
    communesVotationWGS84,
    dist = 30,
    dist_unit = "km",
    st.size = 3,
    transform = TRUE,
    model = "WGS84",
    border.size = 0.5,
    height = 0.01,
    box.fill = c("#4e4d47", "#f5f5f2"),
    st.color = "#4e4d47",
    box.color = "#4e4d47"
  )
```

-   Echelle de couleur divergente [scale_colour_gradient2](https://ggplot2.tidyverse.org/reference/scale_gradient.html)

## Cartes dynamiques

Suite à la création de cartes statiques, il est envisageable de concevoir des cartes dynamiques et interactives. Cependant, le processus de conception diffère légèrement et ce type de carte ne saurait être reproduit sur support papier. Les données peuvent être aisément importées à partir de projets personnels ou encore via des sources de données ouvertes (*open-source*).

### Leaflet

[Leaflet](https://leafletjs.com) est une librairie JavaScript *open-source* très populaire qui permet de créer des cartes interactives.

Cette méthode permet de projeter un fond de carte [OpenStreetMap](https://www.openstreetmap.org/#map=8/46.825/8.224) et d'y afficher les informations souhaitées, comme par exemple des marqueurs (points), des lignes ou des polygones. Le choix de chaque primitive dépend de l'échelle.

-   **Fond de carte** : *Le fond de carte est formé d'un ensemble d'objets géographiques qui doivent être choisis avec attention selon le niveau de détails souhaité, le support de la représentation (papier ou numérique), son objectif et le public auquel s'adresse la carte* (Lambert & Zanin, 2016).

Exemples de primitives :

-   **Point** (marqueur, cercles proportionnels) : restaurants sur la ville de Lausanne, villes sur la carte du monde, toilettes sur le plan de l'Anthropole, cercles propotionnels avec la population par canton, etc
-   **Ligne** : routes sur la carte de la Suisse, rivières sur la carte du canton de Vaud, routes maritimes sur la carte du monde, etc
-   **Polygone** : bâtiments sur le plan du campus de l'Unil, ville de Lausanne sur une carte du canton de Vaud, pays sur une carte du monde, représentation des résultats de votations par communes suisses, etc

Il est possible d'ajouter des tuiles d'un fond de carte et les trois types de primitives avec `Leaflet`. Les différents types d'éléments graphiques sont visibles en exécutant `?addControl` sur la consoles R. Il faudra donc jouer sur les septs variables visuelles définies par Bertin (1967): la position, la taille, la forme, la valeur, la couleur, l'orientation et la texture.

#### Débuter avec Leaflet

1.  Ouvrir le package Leaflet
2.  Créer une carte avec la commande `leaflet()`
3.  Ajouter les tuiles (la carte s'affiche par [tuile](https://en.wikipedia.org/wiki/Tiled_web_map))
4.  [Ajouter le marqueur](http://rstudio.github.io/leaflet/markers.html), en précisant la longitude et la latitude, puis ajouter un *pop-up* avec un texte souhaité, ici "Anthropole". Il est par ailleurs possible d'y ajouter des images ou des liens par exemple.

```{R}
library("leaflet")
myMap <- leaflet()
myMap <- addTiles(myMap) # Ajoute les tuiles OpenStreetMap par défaut
myMap <- addMarkers(myMap, lng=6.584349, lat=46.523704, popup="Anthropole")
myMap <- addMarkers(myMap, lng=6.579111, lat=46.526750, popup="Géopolis")
myMap # Affiche la carte
```

Même résultat avec les *pipes* afin d'éviter la répétition de code.

```{R eval=FALSE}
myMap <- leaflet() %>%
  addTiles() %>%  # Ajoute les tuiles OpenStreetMap par défaut
  addMarkers(lng=6.584349, lat=46.523704, popup="Anthropole") %>%
  addMarkers(lng=6.579111, lat=46.526750, popup="Géopolis")
```

### Overpass Turbo

[Overpass Turbo](https://wiki.openstreetmap.org/wiki/Overpass_turbo) est une [API](https://www.redhat.com/fr/topics/api/what-are-application-programming-interfaces) qui permet d'utiliser des attributs (points, lignes, polygones) issus d'OpenStreetMap. Les données sont entrées par les utilisateurs, il est donc possible qu'elles ne soient pas parfaites. A noter que le server OSM peut être capricieux et la requête lancée plusieurs fois si les données sont lourdes.

Des exemples sont disponibles sur [Cran osmdata](https://cran.r-project.org/web/packages/osmdata/vignettes/osmdata.html).

Cet exemple montre comment obtenir une carte interactive des musées qui sont dans le canton en Vaud (polygone convexe). Un deuxième exemple est effectué par la suite avec les musées du grand Londres.

```{R}
# Obtenir une matrice des musées du canton de Vaud
library("sf")
library("osmdata")

museumP <- getbb("Vaud") %>%
  opq() %>%
  add_osm_feature(key = "tourism", value = "museum") %>%
  osmdata_sf() %>%
  .$osm_points %>%
  subset(!is.na(name))

myMap <- 
  leaflet() %>%
  addTiles() %>% 
  addMarkers(data = museumP, popup = museumP$name)
myMap
```

-   `getbb` : obtenir la *bounding box* (l'enveloppe) de la zone étudiées, possible aussi de mettre des coordonées, par exemple pour Londres : `bbox = c(51.1, 0.1, 51.2, 0.2)`
-   `opq` : *bounding box* qui contient la zone
-   `add_osm_feature` : ajoute les différents *features* à la carte. La liste des différentes paires `key` (clé) et `value` (valeur) est disponible [ici](https://wiki.openstreetmap.org/wiki/Map_features).
Il est possible d'utiliser une clé sans valeur mais pas inversement. Il est aussi possible d'utiliser une négation pour la clé ou la valeur, par exemple : `add_osm_feature (key = "highway", value = "!primary")` affiche les routes (highway), en excluant les routes principales.

Le même type de carte sur le grand Londres, avec un clustering des markers `clusterOptions = markerClusterOptions()` et un [autre fond de carte](https://rstudio.github.io/leaflet/basemaps.html), ce qui rend la lisibilité meilleure.

```{R}
get_museumLondon <- getbb("London") %>%
  opq () %>%
  add_osm_feature("tourism", "museum")

museumPL <- osmdata_sf(get_museumLondon)$osm_points
museumPL <- museumPL[which (museumPL$name != 'is.na'),]

myMapCluster <- leaflet() %>%
  addProviderTiles(providers$CartoDB.Positron) %>%
  addMarkers(data = museumPL,
    popup = museumPL$name,
    clusterOptions = markerClusterOptions()
  )
myMapCluster
```

-   La commande `addProviderTiles()` permet de choisir un fond de carte qui s'adapte au mieux à la thématique, voici une [liste](https://leaflet-extras.github.io/leaflet-providers/preview/) de ce qui existe et [pour les insérer dans R](https://github.com/rstudio/leaflet.providers)
-   La commande `clusterOptions = markerClusterOptions()` permet de créer des clusters quand la carte est dézoomée.

#### Carte des remontées mécaniques dans la région du Valais

La première distinction à faire avec cette carte réside dans le fait que les points sont ici cartographiés, qui représentent les systèmes de transport par câbles(`aerialway`).
A cela s'ajoute deux fonds de carte issus de [SwissTopo](https://map.geo.admin.ch), le premier est la ["carte Siegfried"](https://www.swisstopo.admin.ch/fr/connaissances-faits/histoire-collections/cartes-historiques/carte-siegfried.html), représentée entre 1870 et 1926 et la seconde la carte en couleur de SwissTopo. La [liste de tous les fonds de carte de SwissTopo](https://www.geo.admin.ch/fr/faq-api-geoadmin/) est disponible et utilisable, mais nécessite une adaptation pour être affichée dans R.

```{R}
library("sf")
library("osmdata")
library("leaflet")

# zone et feature
getAerialway <- getbb("Valais") %>%
  opq () %>%
  add_osm_feature(key = "aerialway")

# sélection des lignes
aerialLines <- osmdata_sf(getAerialway)$osm_lines

# adresse fournisseur tuile (SwissTopo)
swissTopo <- "https://wms.geo.admin.ch"

# adresse des fonds de carte SwissTopo
tileSiegfried <- "ch.swisstopo.hiks-siegfried"
tileSwissTopo <- "ch.swisstopo.pixelkarte-farbe"

# Création de la carte
cableMap <-
  leaflet() %>%
  addWMSTiles(swissTopo, 
              layers = tileSiegfried, 
              options = WMSTileOptions(format = "image/png", 
                                       transparent = T ),
              group = 'Siegfried') %>%
  addWMSTiles(swissTopo, 
              layers = tileSwissTopo, 
              options = WMSTileOptions(format = "image/png", 
                                       transparent = T),
              group = 'SwissTopo') %>%
  addPolylines(data = aerialLines, popup = ~name, color = "#FF0000", weight = 2, opacity = 0.9) %>% 
  addLayersControl(baseGroups = c('Siegfried', 'SwissTopo')) %>% # ajout du choix des fonds de carte
  addScaleBar(position = "bottomright", options = scaleBarOptions(imperial = F))
cableMap
```

-   `addWMSTiles` : permet d'afficher des autres fonds de carte disponibles, en spécifiant le format
  -   `group` : permet de donner un identifiant à la couche
-   `addLayersControl` : permet d'ajouter un *controller* de sélection de couche affichée à l'écran
- `addScaleBar` : permet d'ajouter une échelle, ici en bas à droite, sans unité impériales


## Carte interactive : polygones

Il est possible d'utiliser ses propres données pour concevoir des cartes interactives. L'exemple de la votation populaire est alors repris.

Leaflet n'accepte pas de *shapefiles* contenant une dimension Z (altitude). Il faut donc la supprimer :

```{R}
mypalette <- colorNumeric(palette = "viridis",
               domain = communesVotationWGS84$ouiPourcentCommune,
               na.color = "transparent")
mypalette(c(45, 43))

m <- leaflet(communesVotationWGS84) %>%
  addProviderTiles(providers$CartoDB.Positron) %>%
  # addTiles() %>%
  setView(lat = 46.801111, lng = 8.226667, zoom = 7) %>%
  addPolygons(
    fillColor = ~ mypalette(communesVotationWGS84$ouiPourcentCommune),
    weight = 0
    # highlightOptions = highlightOptions(color = "white", weight = 1,
    #                                     bringToFront = F)
  )
m
```

Description des commandes

-   `leaflet(communesVotationWGS84)` : ajout d'un fond de carte Leaflet mettant à disposition les données `communesVotationWGS84` pour les afficher.
-   `addProviderTiles` : décrit plus haut
-   `addTiles` : affiche la carte avec le fond de carte de base
-   `setView` : détermine la point central de la carte en coordonnées WGS84 et il est possible de choisir le niveau de zoom (plus le chiffre est grand, plus la carte est zoomée).
-   `addPolygons` : ajoute les polygones chargés précédemment
    -   `fillColor` : utilise une palette de couleur suivant un indicateur à cartographier
    -   `weight` : contour de chaque polygone, ici 0 dans le but que la carte soit fluide

La première carte n'est pas assez lisible. Pour la suite, elle sera centrée sur le canton de Vaud, le fond de carte sera plus neutre, une échelle sera ajoutée ainsi que de l'interactivité quand la souris passe sur les éléments (`highlight`).

```{R}
# Nom du label
labels <- paste0(communesVotationWGS84$NAME,"\n",round(communesVotationWGS84$ouiPourcentCommune*100)/100, " %", collapse = NULL)
pal <- colorNumeric("viridis", NULL) # fonction de palette de couleurs (divergente)

m2 <- leaflet(communesVotationWGS84) %>%
  addProviderTiles(providers$CartoDB.Positron) %>%
  #addTiles()  %>%
  setView( lat=46.637785, lng=7.2 , zoom=9) %>%
  addPolygons(
    fillColor = ~pal(ouiPourcentCommune),
    weight = 0.3,
    opacity = 1,
    color = "white",
    #dashArray = "3",
    fillOpacity = 0.7,
    highlight = highlightOptions(
      weight = 2,
      color = "#666",
      #dashArray = "",
      fillOpacity = 0.7,
      bringToFront = TRUE),
    label = labels,
    labelOptions = labelOptions(
      style = list("font-weight" = "normal", padding = "3px 8px"),
      textsize = "15px",
      direction = "auto"),
    smoothFactor = 0.2
  ) %>%
  addLegend(pal = pal, values = ~ouiPourcentCommune, opacity = 0.7, title = NULL,
                 position = "bottomright")
m2
```

-   `labels <- paste0(communesVotationWGS84$NAME,"\n",round(communesVotationWGS84$ouiPourcentCommune*100)/100, " %", collapse = NULL)` : préparation de ce qui sera affiché en glissant la souris au-dessus des polygones
-   `addPolygons`
    -   `fillColor = ~pal(ouiPourcentCommune)` : remplit les polygones de la palette de couleur `pal`créée précédemment suivant les données `OuiPourcent`
    -   `dashArray` : permet de mettre des bordures en traits tillés
    -   `highlightOptions` : permet de mettre en évidence un polygone en passant la souris par dessus un élément de la carte. Ici le trait du contour sera plus épais et d'une autre couleur
    -   `label = labels` : le label est ce qui sera affiché en passant la souris par dessus un polygone. Ici un texte est repris de la variable `labels` définie ci-dessus
    -   `labelOptions` : permet de définir la police, la taille, la couleur ou encore l'alignement du texte dans le popup
-   `addLegend` : permet d'ajouter la légende et sa position à la carte. [Description détaillée](https://rdrr.io/github/rstudio/leaflet/man/addLegend.html) des options possibles

Cette carte présente les résultats de la votation sur l'initiative pour une 13e rente AVS en mars 2024, en Suisse. Les communes sont colorées en fonction du pourcentage de votes "oui", avec des cercles proportionnels représentant la population des cantons sur une deuxième carte. Les lacs suisses sont également affichés en arrière-plan.

```{R}
library(leaflet)
library(leaflegend)
cantonsCHWGS84 <- st_transform(cantonsCH, crs = 4326)
lacsWGS84 <- st_transform(lacs, crs = 4326)
# Convertir les géométries de GEOMETRY en MULTIPOLYGON
cantonsCHMP <- st_cast(cantonsCHWGS84, "MULTIPOLYGON") 
# Extrait les coordonnées des géométries des cantons
centroids <- st_centroid(cantonsCHMP) %>% st_coordinates()
coordCantons <- as.data.frame(st_coordinates(st_geometry(cantonsCHMP)))
cantonsCHMP$X <- centroids[,1]
cantonsCHMP$Y <- centroids[,2]

symbols <- makeSymbolsSize(
  values = cantonsCHWGS84$population,
  shape = 'circle',
  color = 'red',
  fillColor = 'red',
  opacity = 0.9,
  baseSize = 25
)

# Titre de la carte
mapTitle = "« Mieux vivre à la retraite (initiative pour une 13e rente AVS) », mars 2024"

leaflet(communesVotationWGS84) %>%
  addProviderTiles(providers$CartoDB.Positron) %>%
  setView(lat=46.637785, lng=8.2 , zoom=7) %>%
  # commune polygons
  addPolygons(
    fillColor = ~pal(ouiPourcentCommune),
    fillOpacity = 0.9,
    color = "white",
    weight = 0.1,
    opacity = 1,
    
    highlight = highlightOptions(
      weight = 2,
      color = "#666",
      fillOpacity = 0.7,
      bringToFront = TRUE,
      sendToBack = TRUE),
    
    label = paste(communesVotationWGS84$NAME, ":", communesVotationWGS84$ouiPourcentCommune, "%"),
    smoothFactor = 0.2,
    group = "Votation"
  ) %>%
  addMarkers(data = cantonsCHMP,
             icon = symbols,
             lat = ~Y, lng = ~X,
             label = paste(cantonsCHMP$nomCanton, ":", cantonsCHMP$population),
             group = "Population") %>%
  addPolygons(data = lacsWGS84,
              weight = 1,
              fillColor = "#dddddd",
              fillOpacity = 0.9,
              color = "white",
              highlight = highlightOptions(
                weight = 1,
                color = "#666",
                fillOpacity = 1,
                bringToFront = TRUE),
              label = lacsWGS84$NAME,
              labelOptions = labelOptions(
                                          style = list(
                                            "color" = "#666",
                                            "font-style" = "italic"
                                          )),
              smoothFactor = 0.2,
              group = "Votation") %>%
  addLayersControl(
    baseGroups = c("Votation","Population"),
    options = layersControlOptions(collapsed = TRUE)
  ) %>%
  addControl(mapTitle, position = "bottomleft") %>%
  addScaleBar(position = "bottomright", options = scaleBarOptions(imperial = F))

```


### Sauver une carte au format `html`

```{R}
library(htmlwidgets)
# saveWidget(cableMap, file="cableMap.html")
```

## Bibliogrpahie

Lambert, N. & Zanin, C. (2016). *Manuel de cartographie: Principes, méthodes, applications*. Paris: Armand Colin.

Wilkinson, L. (2005). *The Grammar of Graphics. Springer-Verlag*. New York: Springer https://doi.org/10.1007/0-387-28695-0

Bertin, J. (1967). Sémiologie graphique: Les diagrammes, les réseaux, les cartes. EHESS.
