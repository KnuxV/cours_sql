> # Lecture 0
>

- Introduction
- Qu'est-ce qu'une base de données ?
- SQL
    - Questions
- Démarrer avec SQLite
- Conseils pour le terminal
- `SELECT`
    - Questions
- `LIMIT`
- `WHERE`
- `NULL`
- `LIKE`
    - Questions
- Plages
    - Questions
- `ORDER BY`
    - Questions
- Fonctions d'agrégation
    - Questions
- Fin

## Introduction

- Les bases de données (et SQL) sont des outils qui peuvent être utilisés pour interagir avec, stocker et gérer des informations. 

- Un tableau stocke un ensemble d'informations
- Chaque ligne d'un tableau stocke un élément de cet ensemble
- Chaque colonne a un attribut de cet élément

- Considérons maintenant un contexte moderne. Disons que vous êtes bibliothécaire et que vous devez organiser des informations sur les titres de livres et les auteurs dans ce diagramme.

- Une façon d'organiser les informations serait d'avoir chaque titre de livre suivi de son auteur, comme ci-dessous.

["Tableau avec les titres de livres suivis de l'auteur"]

    - Notez que chaque livre est maintenant une ligne dans ce tableau.
    - Chaque ligne a deux colonnes - chacune un attribut différent du livre (titre du livre et auteur).
- À l'ère de l'information d'aujourd'hui, nous pouvons stocker nos tableaux en utilisant des logiciels comme Google Sheets au lieu de papier📝 ou de tablettes en pierre🪨. Cependant, dans ce cours, nous parlerons de bases de données et non de feuilles de calcul.
- Trois raisons de passer des feuilles de calcul aux bases de données sont :
    - Échelle : Les bases de données peuvent stocker non seulement des éléments se comptant par dizaines de milliers, mais aussi des millions et des milliards.
    - Capacité de mise à jour : Les bases de données sont capables de gérer plusieurs mises à jour de données par seconde.
    - Vitesse : Les bases de données permettent une recherche plus rapide des informations. Cela est dû au fait que les bases de données nous donnent accès à différents algorithmes pour récupérer des informations. En revanche, les feuilles de calcul qui permettent uniquement l'utilisation de Ctrl+F ou Cmd+F pour parcourir les résultats un par un.

## Qu'est-ce qu'une base de données ?

- Une base de données est un moyen d'organiser des données de manière à pouvoir effectuer quatre opérations CRUD:
    - CREATE
    - READ
    - UPDATE
    - DELETE
- Un système de gestion de base de données (DBMS) est un moyen d'interagir avec une base de données en utilisant une interface graphique ou un langage textuel.
- Exemples de DBMS : MySQL, Oracle, PostgreSQL, SQLite, Microsoft Access, MongoDB, etc.
- Le choix d'un DBMS repose sur des facteurs tels que :
    - Coût : logiciel propriétaire vs logiciel gratuit,
    - Quantité de support : les logiciels libres et open source comme MySQL, PostgreSQL et SQLite ont l'inconvénient de devoir configurer la base de données soi-même,
    - Poids : les systèmes plus complets comme MySQL ou PostgreSQL sont plus lourds et nécessitent plus de calcul pour fonctionner que les systèmes comme SQLite.
- Dans ce cours, nous commencerons par SQLite.

## SQL

- SQL signifie Structured Query Language. C'est un langage utilisé pour interagir avec les bases de données, par lequel vous pouvez créer, lire, mettre à jour et supprimer des données dans une base de données. Quelques points importants sur SQL :
    - il est structuré, comme nous le verrons dans ce cours,
    - il a certains mots-clés qui peuvent être utilisés pour interagir avec la base de données, et
    - c'est un langage de requête - il peut être utilisé pour poser des questions sur les données à l'intérieur d'une base de données.
- Dans cette leçon, nous allons apprendre à écrire quelques requêtes SQL simples.

### Questions

> Existe-t-il des sous-ensembles de SQL ?

- SQL est une norme à la fois de l'American National Standards Institute (ANSI) et de l'Organisation internationale de normalisation (ISO). La plupart des DBMS supportent un sous-ensemble du langage SQL. Par exemple, pour SQLite, nous utilisons un sous-ensemble de SQL qui est supporté par SQLite. Si nous voulions porter notre code vers un autre système comme MySQL, il est probable que nous devrions changer une partie de la syntaxe.

## Démarrer avec SQLite

- Il est utile de noter que SQLite n'est pas seulement quelque chose que nous utilisons pour ce cours, mais une base de données utilisée dans de nombreuses autres applications, y compris les téléphones, les applications de bureau et les sites web.

## Conseils pour le terminal

Voici quelques conseils utiles pour écrire du code SQL sur le terminal.

- Pour effacer l'écran du terminal, appuyez sur Ctrl + L.
- Pour obtenir la ou les instructions précédemment exécutées sur le terminal, appuyez sur la touche fléchée vers le haut.
- Si votre requête SQL est trop longue et s'enroule autour du terminal, vous pouvez appuyer sur Entrée et continuer à écrire la requête sur la ligne suivante.
- Pour quitter une base de données ou l'environnement SQLite, utilisez `.quit`.
- Pour visualiser les différents tableaux dans la base de données utilisiez `.tables`
- `.mode column` permet une visualisation plus facile.

## `SELECT`

- Quelles données sont réellement dans notre base de données ? Pour répondre à cette question, nous allons utiliser notre premier mot-clé SQL, `SELECT`, qui nous permet de sélectionner certaines (ou toutes) lignes d'un tableau à l'intérieur de la base de données.
- Dans l'environnement SQLite, exécutez :

```sql
SELECT *
FROM "nom_du_tableau";
```

Cela sélectionne toutes les lignes du tableau appelé `nom_du_tableau`.
- La sortie que nous obtenons contient toutes les colonnes de toutes les lignes de ce tableau, ce qui fait beaucoup de données. Nous pouvons la simplifier en sélectionnant une colonne particulière, par exemple le titre, du tableau. Essayons :

```sql
SELECT "nom_de_la_colonne"
FROM "nom_du_tableau";
```

- Maintenant, nous voyons une liste des titres dans ce tableau. Mais que faire si nous voulons voir les titres et les auteurs dans nos résultats de recherche ? Pour cela, nous exécutons :

```sql
SELECT "colonne1", "colonne2"
FROM nom_du_tableau;
```

### Questions

> Est-il nécessaire d'utiliser les doubles guillemets (“) autour des noms de tableaux et de colonnes ?

- Il est bon d'utiliser des doubles guillemets autour des noms de tableaux et de colonnes, qui sont appelés identificateurs SQL. SQL a également des chaînes de caractères (string en anglais) et nous utilisons des guillemets simples autour des chaînes de caractères pour les différencier des identificateurs.

> Comment savons-nous quels tableaux et colonnes se trouvent dans une base de données ?

- Le schéma de la base de données contient la structure de la base de données, y compris les noms des tableaux et des colonnes.  Dans le terminal, on utilise la commande `.schema`

> SQLite 3 est-il sensible à la casse ? Pourquoi certaines parties de la requête sont-elles en majuscules et d'autres en minuscules ?

- SQLite n'est pas sensible à la casse. Cependant, nous suivons certaines conventions de style. Observez cette requête :

```sql
SELECT *
FROM "tableau";
```

Les mots-clés SQL sont écrits en majuscules. Cela est particulièrement utile pour améliorer la lisibilité des requêtes plus longues. Les noms de tableaux et de colonnes sont en minuscules.

## `LIMIT`

- Si une base de données contient des millions de lignes, il ne serait peut-être pas judicieux de sélectionner toutes ses lignes. Au lieu de cela, nous pourrions simplement vouloir jeter un coup d'œil aux données qu'elle contient. Nous utilisons le mot-clé SQL `LIMIT` pour spécifier le nombre de lignes dans la sortie de la requête.
```sql
  SELECT title, air_date 
  FROM episodes 
  LIMIT 5;
```

> | title                | air_date   |
> | -------------------- | ---------- |
> | Lost My Marbles      | 2002-01-21 |
> | Castleblanca         | 2002-01-22 |
> | R-Fair City          | 2002-01-23 |
> | Snow Day to be Exact | 2002-01-24 |
> | Sensible Flats       | 2002-01-25 |

Cette requête nous donne les 5 premiers titres et data de la base de données. Les titres sont ordonnés de la même manière dans la sortie de cette requête que dans la base de données.

## `WHERE`

- Le mot-clé `WHERE` est utilisé pour sélectionner des lignes en fonction d'une condition ; il affichera les lignes pour lesquelles la condition spécifiée est vraie.
- ```sql
  SELECT "title"
  FROM "episodes"
  WHERE "season" = 1;
  ```
|                title                |
|-------------------------------------|
| Lost My Marbles                     |
| Castleblanca                        |
| R-Fair City                         |
| Snow Day to be Exact                |
| Sensible Flats                      |
| Zeus on the Loose                   |
| The Poddleville Case                |
| And They Counted Happily Ever After |
| Clock Like An Egyptian              |
| Secrets of Symmetria                |
| A Day at the Spa                    |
| Of All The Luck                     |
| Eureeka                             |
| Cool It                             |
| Find Those Gleamers                 |
| Codename: Icky                      |
| Return to Sensible Flats            |
| Problem Solving in Shangri-La       |
| Send in the Clones                  |
| Trading Places                      |
| Less Than Zero                      |
| Model Behavior                      |
| Fortress of Attitude                |
| Size Me Up                          |
| A Battle of Equals                  |
| Out of Sync                         |




Cela nous donne les titres des épisodes de la saison 1. Notez que `1` n'est pas entre guillemets car c'est un entier, et non une chaîne de caractères ou un identificateur.
- Les opérateurs qui peuvent être utilisés pour spécifier des conditions en SQL sont `=` (“égal à”), `!=` (“différent de”) et `<>` (également “différent de”).

    - Notez qu'il faut utiliser les guillemets sumple pour faire une comparison avec une chaîne de caractères en SQL (et les guillemets doubles si nom de colonne). 

On peut aussi utiliser le mot-clé SQL `NOT`. Une requête serait :

```sql
SELECT "title
FROM "episodes"
WHERE NOT "season" = 1;
```

- Pour combiner des conditions, nous pouvons utiliser les mots-clés SQL `AND` et `OR`. Nous pouvons également utiliser des parenthèses pour indiquer comment combiner les conditions dans une instruction conditionnelle composée.
- Pour sélectionner les titres et les auteurs des livres présélectionnés en 2022 ou 2023 :

```sql
SELECT "title", "air_date" 
from "episodes" 
where 
	"season"=1 
	OR 
	"season"=2;
```

- Pour sélectionner les livres présélectionnés en 2022 ou 2023 qui n'étaient pas des reliures cartonnées :

```sql
SELECT "title", "topic"
FROM "episodes"
WHERE ("season" = 1 OR "season" = 2) AND "topic" != 'Navigation';
```

Ici, les parenthèses indiquent que la clause `OR` doit être évaluée avant la clause `AND`.

## `NULL`

- Il est possible que les tableaux aient des données manquantes. `NULL` est un type utilisé pour indiquer que certaines données n'ont pas de valeur, ou n'existent pas dans le tableau.
- Les conditions utilisées avec `NULL` sont `IS NULL` et `IS NOT NULL`.

```sql
SELECT "title",
FROM "episodes"
WHERE "topic" IS NULL;
```

- Essayons l'inverse : sélectionner les épisodes pour lesquels il n'y a pas de topic

```sql
SELECT "title",
FROM "episodes"
WHERE "topic" IS NOT NULL;
```

## `LIKE`

- Ce mot-clé est utilisé pour sélectionner des données qui correspondent approximativement à la chaîne de caractères spécifiée. Par exemple, `LIKE` pourrait être utilisé pour sélectionner des livres qui ont un certain mot ou une certaine phrase dans leur titre.
- `LIKE` est combiné avec les opérateurs `%` (correspond à n'importe quels caractères autour d'une chaîne de caractères donnée) et `_` (correspond à un seul caractère).
- Pour sélectionner les épisodes avec le mot “love” dans leurs titres, nous pouvons exécuter :

```sql
SELECT "title"
FROM "episodes"
WHERE "title" LIKE '%love%';
```

`%` correspond à 0 ou plusieurs caractères, donc cette requête correspondra aux titres de livres qui ont 0 ou plusieurs caractères avant et après “love” - c'est-à-dire, les titres qui contiennent “love”.
- Pour sélectionner les livres dont le titre commence par “The”, nous pouvons exécuter :

```sql
SELECT "title"
FROM "episodes"
WHERE "title" LIKE 'The%';
```

- La requête ci-dessus pourrait également retourner des épisodes dont les titres commencent par “Their” ou “They”. Pour sélectionner uniquement les épisodes dont les titres commencent par le mot “The”, nous pouvons ajouter un espace.

```sql
SELECT "title"
FROM "episodes"
WHERE "title" LIKE 'The %';
```

### Questions

> Pouvons-nous utiliser plusieurs symboles `%` ou `_` dans une requête ?

- Oui, nous pouvons ! Exemple 1 : Si nous voulions sélectionner des épisodes dont les titres commencent par “The” et ont “of” quelque part au milieu, nous pourrions exécuter :

```sql
SELECT "title"
FROM "episodes"
WHERE "title" LIKE 'The%love%';
```

- Exemple 2 : Si nous savions qu'il y a un épisode dans le tableau dont le titre commence par “T” et contient 9 lettres, nous pouvons essayer de le trouver en exécutant :

```sql
SELECT "title"
FROM "episodes"
WHERE "title" LIKE 'T______';';
```

> La comparaison des chaînes de caractères est-elle sensible à la casse en SQL ?

- Dans SQLite, la comparaison des chaînes de caractères avec `LIKE` est par défaut insensible à la casse, tandis que la comparaison des chaînes de caractères avec `=` est sensible à la casse. (Notez que, dans d'autres DBMS, la configuration de votre base de données peut changer cela !)

## Plages

- Nous pouvons également utiliser les opérateurs `<`, `>`, `<=` et `>=` dans nos conditions pour correspondre à une plage de valeurs. 

```sql
SELECT "title"
FROM "episodes"
WHERE "season" >= 2 AND "season" < 5;
```

- Une autre façon d'obtenir les mêmes résultats est d'utiliser les mots-clés `BETWEEN` et `AND` pour spécifier des plages **inclusives**. Nous pouvons exécuter :

```sql
SELECT "title"
FROM "episodes"
WHERE "season" BETWEEN 2 AND 4;
```

### Questions

> Pour les opérateurs de plage comme `<` et `>`, les valeurs de la base de données doivent-elles être des entiers ?

- Non, les valeurs peuvent être des entiers ou des nombres à virgule flottante (c'est-à-dire, “décimaux” ou “réels”). Lors de la création d'une base de données, il existe des moyens de définir ces types de données pour les colonnes.

## `ORDER BY`

- Le mot-clé `ORDER BY` nous permet d'organiser les lignes retournées dans un certain ordre spécifié.
- La requête suivante sélectionne les 10 livres les moins bien notés de notre base de données.

```sql
SELECT "title","air_date"
FROM "episodes"
ORDER BY "air_date" LIMIT 10;
```

- Notez que l'ordre est croissant par défaut.
- Au lieu de cela, pour sélectionner les 10 derniers éléments :

```sql
SELECT "title", "air_date"
FROM "episodes"
ORDER BY "air_date" DESC LIMIT 10;
```

Notez l'utilisation du mot-clé SQL `DESC` pour spécifier l'ordre décroissant. `ASC` peut être utilisé pour spécifier explicitement l'ordre croissant.
- Comme critère de départage, nous pouvons exécuter un deuxième `ORDER BY`:

```sql
SELECT * 
from episodes 
ORDER BY 
	season DESC, 
	episode_in_season ASC;
```

Notez que pour chaque colonne dans la clause `ORDER BY`, nous spécifions l'ordre croissant ou décroissant.

### Questions

> Pour trier les livres par épisodes par ordre alphabétique, pouvons-nous utiliser `ORDER BY` ?

- Oui, nous pouvons. La requête serait :

```sql
SELECT "title"
FROM "episodes"
ORDER BY "title";
```

## Fonctions d'agrégation

- `COUNT`, `AVG`, `MIN`, `MAX`, et `SUM` sont appelées fonctions d'agrégation et nous permettent d'effectuer les opérations correspondantes sur plusieurs lignes de données. Par leur nature même, chacune des fonctions d'agrégation suivantes ne retournera qu'une seule sortie - la valeur agrégée.
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

- Souvenez-vous que nous avons utilisé `*` pour sélectionner chaque ligne et colonne de la base de données. Dans ce cas, nous essayons de compter chaque ligne de la base de données et donc nous utilisons le `*`.
- Pour compter le nombre de traducteurs :

```sql
SELECT COUNT("translator")
FROM "longlist";
```

- Nous observons que le nombre de traducteurs est inférieur au nombre de lignes dans la base de données. C'est parce que la fonction `COUNT` ne compte pas les valeurs `NULL`.
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

### Questions

> L'utilisation de `MAX` avec la colonne de titre vous donnerait-elle le titre de livre le plus long ?

- Non, l'utilisation de `MAX` avec la colonne de titre vous donnerait le titre “le plus grand” (ou dans ce cas, le dernier) par ordre alphabétique. De même, `MIN` donnera le premier titre par ordre alphabétique.

## Fin

- Cela conclut la Lecture 0 sur les requêtes en SQL ! Pour quitter l'invite SQLite, vous pouvez taper le mot-clé SQLite `.quit` et cela devrait vous ramener au terminal habituel.
- 