# Normals

> À la suite d'un nouveau record annuel de chaleur établi en 2022 — le dernier d'une série d'années records — les températures moyennes de la surface des océans dans le monde ont augmenté depuis début mars. En excluant les régions polaires, elles sont environ deux dixièmes de degré Celsius plus chaudes que ce que les scientifiques ont jamais observé à cette période de l'année via les données satellites.
>
> The Washington Post, 28 avril 2023

## Démonstration

```sqlite
$ sqlite3 normals.db                                                     sqlite> .tables                                                         normals                                                                 sqlite> SELECT "latitude", "longitude", "0m", "250m", "500m" FROM "normals"  
```



## Problème à Résoudre

Comment savons-nous si les températures des océans sont plus basses ou plus élevées que la "normale" ? Qu'est-ce qu'une température "normale" ? Il s'avère que les scientifiques ont développé une métrique appelée "Climate Normal". Une Climate Normal caractérise les aspects du climat terrestre — ses conditions météorologiques à long terme — sur une période de 30 ans. Une métrique importante est la température des océans.

Dans une base de données appelée `normals.db`, en utilisant une table appelée `normals`, explorez certaines des données les plus récentes de Climate Normal pour comprendre ce qui constitue une température océanique normale dans le monde.

## Schéma

Les données dans `normals.db` peuvent être visualisées comme suit :

![Figure des Normales Climatiques](https://cs50.harvard.edu/sql/2024/psets/0/normals/normals.jpg)

Remarquez que la Terre est divisée en une grille de coordonnées — des points qui peuvent identifier presque n'importe quel endroit sur Terre. Chaque coordonnée est une combinaison de latitude et de longitude. Et remarquez comment chaque coordonnée au-dessus d'un océan a une certaine température normale de la surface de l'océan qui lui est associée !

Dans `normals.db`, vous trouverez une seule table de coordonnées, `normals`. Dans la table `normals`, vous trouverez les colonnes suivantes :

- `id`, qui identifie de manière unique chaque ligne (coordonnée) de la table
- `latitude`, qui est le degré de latitude (exprimé en format décimal) pour la coordonnée
- `longitude`, qui est le degré de longitude (exprimé en format décimal) pour la coordonnée
- `0m`, qui est la température normale de la surface de l'océan (c'est-à-dire la température normale à 0 mètre de profondeur), en degrés Celsius, à la coordonnée
- `5m`, qui est la température normale de l'océan à 5 mètres de profondeur, en degrés Celsius, à la coordonnée
- `10m`, qui est la température normale de l'océan à 10 mètres de profondeur, en degrés Celsius, à la coordonnée

Et les observations continuent jusqu'à `5500m`, ou 5500 mètres de profondeur, pour certaines coordonnées !

Puisque `normals` est une table large, si vous voulez prévisualiser le contenu, envisagez de `SELECT`ionner uniquement les colonnes `latitude`, `longitude`, et quelques colonnes de profondeur.

## Spécification

Pour chacune des questions suivantes, vous devez écrire une seule requête SQL qui produit les résultats spécifiés par chaque problème. Votre réponse doit prendre la forme d'une seule requête SQL. Vous ne devez pas faire d'hypothèses sur les `id`s de certaines observations : vos requêtes doivent être exactes même si l'`id` d'une observation particulière était différent. Enfin, chaque requête doit retourner uniquement les données nécessaires pour répondre à la question.

1. Dans `1.sql`, écrivez une requête SQL pour trouver la température normale de la surface de l'océan dans le Golfe du Maine, au large des côtes du Massachusetts. Pour trouver cette température, regardez les données associées à 42,5° de latitude et -69,5° de longitude.
   - Rappelez-vous que vous pouvez trouver la température normale de la surface de l'océan dans la colonne `0m`, qui signifie 0 mètre de profondeur !
2. Dans `2.sql`, écrivez une requête SQL pour trouver la température normale du capteur le plus profond près du Golfe du Maine, à la même coordonnée ci-dessus.
   - Le capteur le plus profond enregistre les températures à 225 mètres de profondeur, vous pouvez donc trouver ces données dans la colonne `225m`.
3. Dans `3.sql`, choisissez un endroit de votre choix et écrivez une requête SQL pour trouver la température normale à 0 mètre, 100 mètres, et 200 mètres. Vous pourriez trouver Google Earth utile si vous souhaitez trouver des coordonnées à utiliser !
4. Dans `4.sql`, écrivez une requête SQL pour trouver la température normale de la surface de l'océan la plus basse.
5. Dans `5.sql`, écrivez une requête SQL pour trouver la température normale de la surface de l'océan la plus élevée.
6. Dans `6.sql`, écrivez une requête SQL pour retourner toutes les températures normales de l'océan à 50m de profondeur, ainsi que leurs degrés respectifs de latitude et de longitude, dans la Mer d'Arabie. Incluez les colonnes latitude, longitude, et température. Pour simplifier, supposez que la Mer d'Arabie est encadrée par les quatre coordonnées suivantes :
   - 20° de latitude, 55° de longitude
   - 20° de latitude, 75° de longitude
   - 0° de latitude, 55° de longitude
   - 0° de latitude, 75° de longitude
7. Dans `7.sql`, écrivez une requête SQL pour trouver la température moyenne de la surface de l'océan, arrondie à deux décimales, le long de l'équateur. Appelez la colonne résultante "Average Equator Ocean Surface Temperature".
   - Les températures de la surface de l'océan à l'équateur peuvent être trouvées à toutes les longitudes entre les latitudes -0,5° et 0,5°, inclus.
8. Dans `8.sql`, écrivez une requête SQL pour trouver les 10 endroits avec la température normale de la surface de l'océan la plus basse, triés du plus froid au plus chaud. Si deux endroits ont la même température normale de la surface de l'océan, triez par latitude, de la plus petite à la plus grande. Incluez les colonnes latitude, longitude, et température de surface.
9. Dans `9.sql`, écrivez une requête SQL pour trouver les 10 endroits avec la température normale de la surface de l'océan la plus élevée, triés du plus chaud au plus froid. Si deux endroits ont la même température normale de la surface de l'océan, triez par latitude, de la plus petite à la plus grande. Incluez les colonnes latitude, longitude, et température de surface.
10. Il y a 180 degrés entiers de latitude. Dans `10.sql`, écrivez une requête SQL pour déterminer combien de points de latitude nous avons au moins un point de donnée. (Pourquoi pourrions-nous ne pas avoir de points de données pour toutes les latitudes ?)

## Utilisation

Pour tester vos requêtes au fur et à mesure que vous les écrivez dans vos fichiers `.sql`, vous pouvez interroger la base de données en exécutant

```
.read FILENAME
```

où `FILENAME` est le nom du fichier contenant votre requête SQL. Par exemple,

```
.read 1.sql
```

Vous pouvez également exécuter

```
$ cat FILENAME | sqlite3 normals.db > output.txt
```

pour rediriger la sortie de la requête vers un fichier texte appelé `output.txt`. (Cela peut être utile pour vérifier combien de lignes sont retournées par votre requête !)

## Comment Tester

- L'exécution de `1.sql` produit une table avec 1 colonne et 1 ligne.
- L'exécution de `2.sql` produit une table avec 1 colonne et 1 ligne.
- `3.sql` dépend de vous !
- L'exécution de `4.sql` produit une table avec 1 colonne et 1 ligne.
- L'exécution de `5.sql` produit une table avec 1 colonne et 1 ligne.
- L'exécution de `6.sql` produit une table avec 3 colonnes et 383 lignes.
- L'exécution de `7.sql` produit une table avec 1 colonne et 1 ligne.
- L'exécution de `8.sql` produit une table avec 3 colonnes et 10 lignes.
- L'exécution de `9.sql` produit une table avec 3 colonnes et 10 lignes.
- L'exécution de `10.sql` produit une table avec 1 colonne et 1 ligne.

Notez que les comptes de lignes n'incluent pas les lignes d'en-tête qui ne montrent que les noms de colonnes.


## Remerciements

Données extraites du World Ocean Atlas 2023, disponibles publiquement auprès de la National Oceanic and Atmospheric Administration (NOAA).