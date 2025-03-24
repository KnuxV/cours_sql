# [Cours 1](https://cs50.harvard.edu/sql/2024/notes/0/)

- [Introduction](#introduction)
- [Qu'est-ce qu'une base de données ?](#quest-ce-quune-base-de-données)
- [SQL](#sql)
- [Démarrer avec SQLite](#démarrer-avec-sqlite)
- [Conseils pour le terminal](#conseils-pour-le-terminal)
- [`SELECT`](#select)
- [`LIMIT`](#limit)
- [`WHERE`](#where)
- [`NULL`](#null)
- [`LIKE`](#like)
- [Plages](#plages)
- [`ORDER BY`](#order-by)
- [Fonctions d'agrégation](#fonctions-dagrégration)

[Vidéo en anglais](https://video.cs50.io/vHYeChEf2lA?screen=6UZILCexEZ4) (facultatif)

## Introduction

- Les bases de données (et SQL) sont des outils qui peuvent être utilisés pour interagir avec, stocker et gérer des informations. Bien que les outils que nous utilisons dans ce cours soient nouveaux, une base de données est une idée ancienne.
- Regardez ce diagramme vieux de quelques milliers d'années. Il a des lignes et des colonnes, et semble contenir des indemnités pour les travailleurs d'un temple. On pourrait appeler ce diagramme un tableau, ou même une feuille de calcul.

![Tableau avec les indemnités des travailleurs du temple](https://cs50.harvard.edu/sql/2024/notes/0/images/templeworkerstipends.jpg)

- Sur la base de ce que nous voyons dans le diagramme ci-dessus, nous pouvons conclure que :
  - Un tableau stocke un ensemble d'informations (ici, les indemnités des travailleurs).
  - Chaque ligne d'un tableau stocke un élément de cet ensemble (ici, un travailleur).
  - Chaque colonne a un attribut de cet élément (ici, l'indemnité pour un mois particulier).
- Considérons maintenant un contexte moderne. Disons que vous êtes bibliothécaire et que vous devez organiser des informations sur les titres de livres et les auteurs dans ce diagramme.

!["Titres de livres et auteurs - Non organisés"](https://cs50.harvard.edu/sql/2024/notes/0/images/books.jpg)

- Une façon d'organiser les informations serait d'avoir chaque titre de livre suivi de son auteur, comme ci-dessous.

!["Tableau avec les titres de livres suivis de l'auteur"](https://cs50.harvard.edu/sql/2024/notes/0/images/bookstable.jpg)

```
- Notez que chaque livre est maintenant une ligne dans ce tableau.
- Chaque ligne a deux colonnes — chacune un attribut différent du livre (titre du livre et auteur).
```

- À l'ère de l'information d'aujourd'hui, nous pouvons stocker nos tableaux en utilisant des logiciels comme Google Sheets au lieu de papier📝 ou de tablettes en pierre🪨. Cependant, dans ce cours, nous parlerons de bases de données et non de feuilles de calcul.
- Trois raisons de passer des feuilles de calcul aux bases de données sont :
  - Échelle : Les bases de données peuvent stocker non seulement des éléments se comptant en dizaines de milliers, mais aussi en millions et en milliards.
  - Capacité de mise à jour : Les bases de données sont capables de gérer plusieurs mises à jour de données par seconde.
  - Vitesse : Les bases de données permettent une recherche plus rapide des informations. Cela est dû au fait que les bases de données nous donnent accès à différents algorithmes pour récupérer des informations. En revanche, les feuilles de calcul permettent uniquement l'utilisation de Ctrl+F ou Cmd+F pour parcourir les résultats un par un.

## Qu'est-ce qu'une base de données ?

- **CRUD**: Une base de données est un moyen d'organiser des données de manière à pouvoir effectuer quatre opérations sur celles-ci :
  - CREATE (créer)
  - READ (lire)
  - UPDATE (mettre à jour)
  - DELETE (supprimer)
- Un système de gestion de base de données (DBMS) est un moyen d'interagir avec une base de données en utilisant une interface graphique ou un langage textuel.
- Exemples de DBMS : MySQL, Oracle, PostgreSQL, SQLite, Microsoft Access, MongoDB, etc.
- Le choix d'un DBMS dépend de facteurs tels que :
  - Coût : logiciel propriétaire vs logiciel gratuit,
  - Quantité de support : les logiciels libres et open source comme MySQL, PostgreSQL et SQLite ont l'inconvénient de devoir configurer la base de données soi-même,
  - Poids : les systèmes plus complets comme MySQL ou PostgreSQL sont plus lourds et nécessitent plus de calculs pour fonctionner que des systèmes comme SQLite.
- Dans ce cours, nous commencerons par SQLite, puis nous passerons à MySQL et PostgreSQL.

## SQL

- SQL signifie Structured Query Language (langage de requête structuré). C'est un langage utilisé pour interagir avec les bases de données, grâce auquel vous pouvez créer, lire, mettre à jour et supprimer des données dans une base de données. Quelques points importants sur SQL :
  - il est structuré, comme nous le verrons dans ce cours,
  - il a certains mots-clés qui peuvent être utilisés pour interagir avec la base de données, et
  - c'est un langage de requête — il peut être utilisé pour poser des questions sur les données à l'intérieur d'une base de données.
- Dans cette leçon, nous apprendrons à écrire quelques requêtes SQL simples.

## Démarrer avec SQLite

- Il est utile de noter que SQLite n'est pas seulement quelque chose que nous utilisons pour ce cours, mais une base de données utilisée dans de nombreuses autres applications, y compris les téléphones, les applications de bureau et les sites web.
- Considérons maintenant une base de données de livres qui ont été présélectionnés pour le Prix International Booker. Chaque année, il y a 13 livres sur la liste restreinte et notre base de données contient 5 années de telles listes restreintes.
- Avant de commencer à interagir avec cette base de données :
  - Connectez-vous à votre compte github, et ouvrez le [lien suivant](https://github.com/KnuxV/sql_cours1#).
  - Ouvrez le répertoire github dans codespace
  - L'environnement SQLite est déjà configuré dans votre Codespace ! Ouvrez-le dans le terminal.

## Conseils pour le terminal

Voici quelques commandes essentielles pour naviguer dans le terminal et manipuler SQLite efficacement.

### Commandes de base du terminal

- `ls` - Affiche le contenu du répertoire actuel
- `cd dossier` - Change de répertoire vers "dossier"
- `cd ..` - Remonte d'un niveau dans l'arborescence
- `mkdir nom_dossier` - Crée un nouveau dossier
- `touch nom_fichier` - Crée un nouveau fichier vide
- `rm nom_fichier` - Supprime un fichier
- `rm -r nom_dossier` - Supprime un dossier et son contenu
- `pwd` - Affiche le chemin du répertoire actuel
- `cat nom_fichier` - Affiche le contenu d'un fichier

### Astuces générales du terminal

- Pour effacer l'écran du terminal, appuyez sur `Ctrl + L`
- Pour accéder aux commandes précédemment exécutées, utilisez la touche `↑` (flèche vers le haut)
- Pour interrompre une commande en cours d'exécution, appuyez sur `Ctrl + C`
- Pour compléter automatiquement un nom de fichier ou de dossier, utilisez la touche `Tab`

### Commandes spécifiques à SQLite

- `.quit` - Quitter l'environnement SQLite
- `.help` - Afficher l'aide et la liste des commandes disponibles
- `.tables` - Afficher toutes les tables dans la base de données
- `.schema [nom_table]` - Afficher la structure d'une table
- `.mode column` - Afficher les résultats en colonnes (plus lisible)
- `.headers on` - Afficher les noms des colonnes dans les résultats
- `.mode csv` - Afficher les résultats au format CSV
- `.mode box` - Afficher les résultats dans des cadres (très lisible)
- `.open nom_fichier.db` - Ouvrir une base de données

### Conseils pour les requêtes SQL

- Si votre requête SQL est trop longue, vous pouvez appuyer sur `Entrée` et continuer sur la ligne suivante
- N'oubliez pas de terminer vos requêtes SQL par un point-virgule `;`
- Pour exécuter un fichier SQL, utilisez `.read nom_fichier.sql`
- Pour enregistrer les résultats d'une requête dans un fichier, utilisez `.output nom_fichier.txt`

## `SELECT`

- Quelles données se trouvent réellement dans notre base de données ? Pour répondre à cette question, nous utiliserons notre premier mot-clé SQL, `SELECT`, qui nous permet de sélectionner certaines (ou toutes) les lignes d'un tableau à l'intérieur de la base de données.
- Dans l'environnement SQLite, exécutez :

```sql
SELECT *
FROM "longlist";
```

Cela sélectionne toutes les lignes du tableau appelé `longlist`.

- La sortie que nous obtenons contient toutes les colonnes de toutes les lignes de ce tableau, ce qui représente beaucoup de données. Nous pouvons la simplifier en sélectionnant une colonne particulière, par exemple le titre, dans le tableau. Essayons :

```sql
SELECT "title"
FROM "longlist";
```

- Maintenant, nous voyons une liste des titres dans ce tableau. Mais que faire si nous voulons voir les titres et les auteurs dans nos résultats de recherche ? Pour cela, nous exécutons :

```sql
SELECT "title", "author"
FROM longlist;
```

## `LIMIT`

- Si une base de données contient des millions de lignes, il ne serait peut-être pas judicieux de sélectionner toutes ses lignes. Au lieu de cela, nous pourrions simplement vouloir jeter un coup d'œil aux données qu'elle contient. Nous utilisons le mot-clé SQL `LIMIT` pour spécifier le nombre de lignes dans la sortie de la requête.

- ```sql
  SELECT "title"
  FROM "longlist"
  LIMIT 10;
  ```

```

Cette requête nous donne les 10 premiers titres de la base de données. Les titres sont triés de la même manière dans la sortie de cette requête que dans la base de données.
​
## `WHERE`
​
- Le mot-clé `WHERE` est utilisé pour sélectionner des lignes en fonction d'une condition ; il affiche les lignes pour lesquelles la condition spécifiée est vraie.
- ```sql
SELECT "title", "author"
FROM "longlist"
WHERE "year" = 2023;
```

Cela nous donne les titres et les auteurs des livres présélectionnés en 2023. Notez que `2023` n'est pas entre guillemets car c'est un entier (integer), et non une chaîne de caractères (string) ou un identifiant.

- Les opérateurs qui peuvent être utilisés pour spécifier des conditions en SQL sont `=` (“égal à”), `!=` (“différent de”) et `<>` (également “différent de”).
- Pour sélectionner les livres qui ne sont pas des couvertures rigides (hardcovers), nous pouvons exécuter la requête :

```sql
SELECT "title", "format"
FROM "longlist"
WHERE "format" != 'hardcover';
```

```
- Notez que `hardcover` est entre guillemets simples car c'est une chaîne de caractères SQL et non un identifiant.
```

- `!=` peut être remplacé par l'opérateur `<>` pour obtenir les mêmes résultats. La requête modifiée serait :

```sql
SELECT "title", "format"
FROM "longlist"
WHERE "format" <> 'hardcover';
```

- Une autre façon d'obtenir les mêmes résultats est d'utiliser le mot-clé SQL `NOT`. La requête modifiée serait :

```sql
SELECT "title", "format"
FROM "longlist"
WHERE NOT "format" = 'hardcover';
```

- Pour combiner des conditions, nous pouvons utiliser les mots-clés SQL `AND` et `OR`. Nous pouvons également utiliser des parenthèses pour indiquer comment combiner les conditions dans une instruction conditionnelle composée.
- Pour sélectionner les titres et les auteurs des livres présélectionnés en 2022 ou 2023 :

```sql
SELECT "title", "author"
FROM "longlist"
WHERE "year" = 2022 OR "year" = 2023;
```

- Pour sélectionner les livres présélectionnés en 2022 ou 2023 qui n'étaient pas des couvertures rigides :

```sql
SELECT "title", "format"
FROM "longlist"
WHERE ("year" = 2022 OR "year" = 2023) AND "format" != 'hardcover';
```

Ici, les parenthèses indiquent que la clause `OR` doit être évaluée avant la clause `AND`.

## `NULL`

- Il est possible que les tableaux aient des données manquantes. `NULL` est un type utilisé pour indiquer que certaines données n'ont pas de valeur, ou n'existent pas dans le tableau.
- Par exemple, les livres de notre base de données ont un traducteur en plus d'un auteur. Cependant, seuls certains des livres ont été traduits en anglais. Pour les autres livres, la valeur du traducteur sera `NULL`.
- Les conditions utilisées avec `NULL` sont `IS NULL` et `IS NOT NULL`.
- Pour sélectionner les livres pour lesquels les traducteurs n'existent pas, nous pouvons exécuter :

```sql
SELECT "title", "translator"
FROM "longlist"
WHERE "translator" IS NULL;
```

- Essayons l'inverse : sélectionner les livres pour lesquels les traducteurs existent.

```sql
SELECT "title", "translator"
FROM "longlist"
WHERE "translator" IS NOT NULL;
```

## `LIKE`

- Ce mot-clé est utilisé pour sélectionner des données qui correspondent approximativement à la chaîne de caractères spécifiée. Par exemple, `LIKE` pourrait être utilisé pour sélectionner des livres qui ont un certain mot ou une certaine phrase dans leur titre.
- `LIKE` est combiné avec les opérateurs `%` (correspond à n'importe quels caractères autour d'une chaîne donnée) et `_` (correspond à un seul caractère).
- Pour sélectionner les livres avec le mot “love” dans leurs titres, nous pouvons exécuter :

```sql
SELECT "title"
FROM "longlist"
WHERE "title" LIKE '%love%';
```

`%` correspond à 0 ou plusieurs caractères, donc cette requête correspondra aux titres de livres qui ont 0 ou plusieurs caractères avant et après “love” — c'est-à-dire, les titres qui contiennent “love”.

- Pour sélectionner les livres dont le titre commence par “The”, nous pouvons exécuter :

```sql
SELECT "title"
FROM "longlist"
WHERE "title" LIKE 'The%';
```

- La requête ci-dessus pourrait également retourner des livres dont les titres commencent par “Their” ou “They”. Pour sélectionner uniquement les livres dont les titres commencent par le mot “The”, nous pouvons ajouter un espace.

```sql
SELECT "title"
FROM "longlist"
WHERE "title" LIKE 'The %';
```

- Étant donné qu'il y a un livre dans le tableau dont le nom est soit “Pyre” soit “Pire”, nous pouvons le sélectionner en exécutant :

```sql
SELECT "title"
FROM "longlist"
WHERE "title" LIKE 'P_re';
```

Cette requête pourrait également retourner des titres de livres comme “Pore” ou “Pure” s'ils existaient dans notre base de données, car `_` correspond à n'importe quel caractère unique.

## Plages

- Nous pouvons également utiliser les opérateurs `<`, `>`, `<=` et `>=` dans nos conditions pour correspondre à une plage de valeurs. Par exemple, pour sélectionner tous les livres présélectionnés entre les années 2019 et 2022 (inclus), nous pouvons exécuter :

```sql
SELECT "title", "author"
FROM "longlist"
WHERE "year" >= 2019 AND "year" <= 2022;
```

- Une autre façon d'obtenir les mêmes résultats est d'utiliser les mots-clés `BETWEEN` et `AND` pour spécifier des plages inclusives. Nous pouvons exécuter :

```sql
SELECT "title", "author"
FROM "longlist"
WHERE "year" BETWEEN 2019 AND 2022;
```

- Pour sélectionner les livres qui ont une note de 4.0 ou plus, nous pouvons exécuter :

```sql
SELECT "title", "rating"
FROM "longlist"
WHERE "rating" > 4.0;
```

- Pour limiter davantage les livres sélectionnés par le nombre de votes, et n'avoir que les livres avec au moins 10 000 votes, nous pouvons exécuter :

```sql
SELECT "title", "rating", "votes"
FROM "longlist"
WHERE "rating" > 4.0 AND "votes" > 10000;
```

- Pour sélectionner les livres qui ont moins de 300 pages, nous pouvons exécuter :

```sql
SELECT "title", "pages"
FROM "longlist"
WHERE "pages" < 300;
```

## `ORDER BY`

- Le mot-clé `ORDER BY` nous permet d'organiser les lignes retournées dans un certain ordre spécifié.
- La requête suivante sélectionne les 10 livres les moins bien notés de notre base de données.

```sql
SELECT "title", "rating"
FROM "longlist"
ORDER BY "rating" LIMIT 10;
```

- Notez que nous obtenons les 10 livres les moins bien notés car `ORDER BY` choisit l'ordre croissant par défaut.
- Au lieu de cela, pour sélectionner les 10 meilleurs livres :

```sql
SELECT "title", "rating"
FROM "longlist"
ORDER BY "rating" DESC LIMIT 10;
```

Notez l'utilisation du mot-clé SQL `DESC` pour spécifier l'ordre décroissant. `ASC` peut être utilisé pour spécifier explicitement l'ordre croissant.

- Pour sélectionner les 10 meilleurs livres par note et inclure également le nombre de votes comme critère de départage, nous pouvons exécuter :

```sql
SELECT "title", "rating", "votes"
FROM "longlist"
ORDER BY "rating" DESC, "votes" DESC
LIMIT 10;
```

Notez que pour chaque colonne dans la clause `ORDER BY`, nous spécifions l'ordre croissant ou décroissant.

## Fonctions d'agrégation

- `COUNT`, `AVG`, `MIN`, `MAX`, et `SUM` sont appelées fonctions d'agrégation et nous permettent d'effectuer les opérations correspondantes sur plusieurs lignes de données. Par leur nature même, chacune des fonctions d'agrégation suivantes ne retournera qu'une seule sortie — la valeur agrégée.
- Pour trouver la note moyenne de tous les livres dans la base de données :

```sql
SELECT AVG("rating")
FROM "longlist";
```

- Pour arrondir la note moyenne à 2 décimales :

```sql
SELECT ROUND(AVG("rating"), 2)
FROM "longlist";
```

- Pour renommer la colonne dans laquelle les résultats sont affichés :

```sql
SELECT ROUND(AVG("rating"), 2) AS "note moyenne"
FROM "longlist";
```

Notez l'utilisation du mot-clé SQL `AS` pour renommer les colonnes.

- Pour sélectionner la note maximale dans la base de données :

```sql
SELECT MAX("rating")
FROM "longlist";
```

- Pour sélectionner la note minimale dans la base de données :

```sql
SELECT MIN("rating")
FROM "longlist";
```

- Pour compter le nombre total de votes dans la base de données :

```sql
SELECT SUM("votes")
FROM "longlist";
```

- Pour compter le nombre de livres dans notre base de données :

```sql
SELECT COUNT(*)
FROM "longlist";
```

```
- Souvenez-vous que nous avons utilisé `*` pour sélectionner chaque ligne et colonne de la base de données. Dans ce cas, nous essayons de compter chaque ligne de la base de données et donc nous utilisons le `*`.
```

- Pour compter le nombre de traducteurs :

```sql
SELECT COUNT("translator")
FROM "longlist";
```

```
- Nous observons que le nombre de traducteurs est inférieur au nombre de lignes dans la base de données. C'est parce que la fonction `COUNT` ne compte pas les valeurs `NULL`.
```

- Pour compter le nombre d'éditeurs dans la base de données :

```sql
SELECT COUNT("publisher")
FROM "longlist";
```

- Comme pour les traducteurs, cette requête comptera le nombre de valeurs d'éditeurs qui ne sont pas `NULL`. Cependant, cela peut inclure des doublons. Un autre mot-clé SQL, `DISTINCT`, peut être utilisé pour s'assurer que seules les valeurs distinctes sont comptées.

```sql
SELECT COUNT(DISTINCT "publisher")
FROM "longlist";
```
