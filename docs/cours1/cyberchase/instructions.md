## [Cyberchase](https://cs50.harvard.edu/sql/2024/psets/0/cyberchase/#cyberchase)

![cyb_img](https://cs50.harvard.edu/sql/2024/psets/0/cyberchase/cyberchase.jpg)
## Problème à Résoudre

Bienvenue dans le Cyberespace ! Cyberchase est une série télévisée animée et éducative pour enfants, diffusée par le Public Broadcasting Service (PBS) des États-Unis depuis 2002. Conçue à l'origine pour « montrer aux enfants que les mathématiques sont partout et que tout le monde peut être bon en maths », le monde de Cyberchase tourne autour de Jackie, Matt et Inez qui s'associent à Digit, un « cybird », pour empêcher Hacker de prendre le contrôle du Cyberespace et d'infecter la Carte Mère. En chemin, le quatuor apprend des compétences en mathématiques, en sciences et en résolution de problèmes pour contrecarrer les tentatives de Hacker.

Dans une base de données appelée `cyberchase.db`, en utilisant une table appelée `episodes`, trouvez des réponses aux questions de PBS sur les épisodes de Cyberchase jusqu'à présent.

## Démonstration

```sqlite
sqlite3 cyberchase.db                                                  sqlite> .tables                                                         episodes                                                                 sqlite> SELECT "title" FROM "episodes"      
```



## Code de Distribution

Pour ce problème, vous devrez utiliser `cyberchase.db`, ainsi que plusieurs fichiers `.sql` dans lesquels vous écrirez vos requêtes.

L'exécution de `sqlite3 cyberchase.db` devrait ouvrir la base de données dans `sqlite3`, via laquelle vous exécuterez des requêtes SQL. 

## Schéma

Chaque base de données a un certain « schéma » — les tables et colonnes dans lesquelles les données sont organisées. Dans `cyberchase.db`, vous trouverez une seule table, `episodes`. Dans la table `episodes`, vous trouverez les colonnes suivantes :

- `id`, qui identifie de manière unique chaque ligne (épisode) de la table
- `season`, qui est le numéro de la saison dans laquelle l'épisode a été diffusé
- `episode_in_season`, qui est le numéro de l'épisode dans sa saison donnée
- `title`, qui est le titre de l'épisode
- `topic`, qui identifie les idées que l'épisode visait à enseigner
- `air_date`, qui est la date (exprimée au format `AAAA-MM-JJ`) à laquelle l'épisode a été « diffusé » (c'est-à-dire publié)
- `production_code`, qui est l'ID unique utilisé par PBS pour faire référence à chaque épisode en interne

## Spécification

Pour chacune des questions suivantes, vous devez écrire une seule requête SQL qui produit les résultats spécifiés par chaque problème. Votre réponse doit prendre la forme d'une seule requête SQL. Vous ne devez faire aucune supposition sur les `id` de certains épisodes : vos requêtes doivent être exactes même si l'`id` d'un épisode particulier était différent. Enfin, chaque requête doit renvoyer uniquement les données nécessaires pour répondre à la question : si le problème vous demande uniquement de sortir les noms des épisodes, par exemple, votre requête ne doit pas également sortir la date de diffusion de chaque épisode.

1. Dans `1.sql`, écrivez une requête SQL pour lister les titres de tous les épisodes de la saison originale de Cyberchase, la Saison 1.
2. Dans `2.sql`, listez le numéro de saison et le titre du premier épisode de chaque saison.
3. Dans `3.sql`, trouvez le code de production de l'épisode « Hackerized! ».
4. Dans `4.sql`, écrivez une requête pour trouver les titres des épisodes qui n'ont pas encore de sujet répertorié.
5. Dans `5.sql`, trouvez le titre de l'épisode des fêtes diffusé le 31 décembre 2004.
6. Dans `6.sql`, listez les titres des épisodes de la saison 6 (2008) qui ont été publiés en avance, en 2007.
7. Dans `7.sql`, écrivez une requête SQL pour lister les titres et sujets de tous les épisodes enseignant les fractions.
8. Dans `8.sql`, écrivez une requête qui compte le nombre d'épisodes publiés au cours des 6 dernières années, de 2018 à 2023 inclus.
    - Vous trouverez peut-être utile de savoir que vous pouvez utiliser `BETWEEN` avec des dates, comme `BETWEEN '2000-01-01' AND '2000-12-31'`.
9. Dans `9.sql`, écrivez une requête qui compte le nombre d'épisodes publiés au cours des 6 premières années de Cyberchase, de 2002 à 2007 inclus.
10. Dans `10.sql`, écrivez une requête SQL pour lister les id, titres et codes de production de tous les épisodes. Triez les résultats par code de production, du plus ancien au plus récent.
11. Dans `11.sql`, listez les titres des épisodes de la saison 5, en ordre alphabétique inverse.
12. Dans `12.sql`, comptez le nombre de titres d'épisodes uniques.
13. Dans `13.sql`, écrivez une requête SQL pour explorer une question de votre choix. Cette requête doit :
    - Impliquer au moins une condition, en utilisant `WHERE` avec `AND` ou `OR`

Vous sentez-vous plus à l'aise ?

Vous pouvez éventuellement essayer les requêtes ci-dessous, qui peuvent nécessiter des connaissances avancées !

1. Écrivez une requête SQL pour trouver les titres des épisodes diffusés pendant la période des fêtes, généralement en décembre aux États-Unis.
    - Votre requête doit produire une table avec une seule colonne pour le titre de chaque épisode.
    - Essayez de trouver une meilleure solution que `LIKE` si vous le pouvez !
2. Écrivez une requête SQL pour trouver, pour chaque année, le premier jour de l'année où PBS a publié un épisode de Cyberchase.
    - Votre requête doit produire une table avec deux colonnes, une pour l'année et une pour le mois et le jour les plus précoces où un épisode a été publié cette année-là.

## Utilisation

Pour tester vos requêtes au fur et à mesure que vous les écrivez dans vos fichiers `.sql`, vous pouvez interroger la base de données en exécutant

```
.read FILENAME
```

où `FILENAME` est le nom du fichier contenant votre requête SQL. Par exemple,

```
.read 1.sql
```

Vous pouvez également exécuter (dans votre terminal, plutôt qu'à l'intérieur de `sqlite3`)

```
$ cat FILENAME | sqlite3 cyberchase.db > output.txt
```

pour rediriger la sortie de la requête vers un fichier texte nommé `output.txt`. (Cela peut être utile pour vérifier combien de lignes sont renvoyées par votre requête !)

## Comment Tester

Réponse à la prochaine séance

- L'exécution de `1.sql` produit une table avec 1 colonne et 26 lignes.
- L'exécution de `2.sql` produit une table avec 2 colonnes et 14 lignes.
- L'exécution de `3.sql` produit une table avec 1 colonne et 1 ligne.
- L'exécution de `4.sql` produit une table avec 1 colonne et 26 lignes.
- L'exécution de `5.sql` produit une table avec 1 colonne et 1 ligne.
- L'exécution de `6.sql` produit une table avec 1 colonne et 2 lignes.
- L'exécution de `7.sql` produit une table avec 2 colonnes et 6 lignes.
- L'exécution de `8.sql` produit une table avec 1 colonne et 1 ligne.
- L'exécution de `9.sql` produit une table avec 1 colonne et 1 ligne.
- L'exécution de `10.sql` produit une table avec 3 colonnes et 140 lignes.
- L'exécution de `11.sql` produit une table avec 1 colonne et 10 lignes.
- L'exécution de `12.sql` produit une table avec 1 colonne et 1 ligne.

`13.sql` dépend de vous !

Notez que les comptes de lignes n'incluent pas les lignes d'en-tête qui ne montrent que les noms de colonnes.

