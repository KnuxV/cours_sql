# Cours 5 ([Version originale](https://cs50.harvard.edu/sql/2024/notes/4/))

- [Introduction](#introduction)
- [Vues (Views)](#vues-views)
- [Simplification](#simplification)
- [Agrégation](#agrégation)
- [Expression Commune de Table (CTE)](#expression-commune-de-table-cte)
- [Partitionnement](#partitionnement)
- [Sécurisation](#sécurisation)
- [Suppressions Logiques (Soft Deletions)](#suppressions-logiques-soft-deletions)

## Introduction

- Jusqu'à présent, nous avons appris des concepts qui nous permettent de concevoir des bases de données complexes et d'y écrire des données. Maintenant, nous allons explorer des moyens d'obtenir des vues à partir de ces bases de données.
- Revenons à la base de données contenant des livres présélectionnés pour le International Booker Prize. Voici un aperçu des tables de cette base de données.

!["Tables contenant des livres et des auteurs avec une relation plusieurs-à-plusieurs"](https://cs50.harvard.edu/sql/2024/notes/4/images/4.jpg)
- Pour trouver un livre écrit par l'auteur Han Kang, nous devrions parcourir chacune des trois tables ci-dessus — d'abord en trouvant l'ID de l'auteur, puis les IDs des livres correspondants et enfin les titres des livres. Au lieu de cela, existe-t-il un moyen de regrouper les informations connexes des trois tables dans une seule vue ?
- Oui, nous pouvons utiliser la commande `JOIN` en SQL pour combiner des lignes de deux tables ou plus en fonction d'une colonne liée entre elles. Voici une représentation visuelle de la manière dont ces tables pourraient être jointes afin d'aligner les auteurs et leurs livres.

!["Table joignant livres, authored et auteurs"](https://cs50.harvard.edu/sql/2024/notes/4/images/8.jpg)

Cela permet de voir facilement que Han Kang a écrit "The White Book".
- On peut aussi imaginer supprimer les colonnes d'ID ici, de sorte que notre vue ressemble à ce qui suit.

!["Table joignant livres, authored et auteurs avec les colonnes d'ID supprimées"](https://cs50.harvard.edu/sql/2024/notes/4/images/10.jpg)

## Vues (Views)

- Une vue est une table virtuelle définie par une requête.
- Supposons que nous écrivions une requête pour joindre trois tables, comme dans l'exemple précédent, puis sélectionnons les colonnes pertinentes. La nouvelle table créée par cette requête peut être enregistrée en tant que vue, à interroger ultérieurement.
- Les vues sont utiles pour :
    - simplifier : regrouper des données de différentes tables pour les interroger plus simplement,
    - agréger : exécuter des fonctions d'agrégation, comme trouver la somme, et stocker les résultats,
    - partitionner : diviser les données en morceaux logiques,
    - sécuriser : masquer les colonnes qui doivent rester sécurisées. Bien qu'il existe d'autres façons dont les vues peuvent être utiles, dans cette conférence, nous nous concentrerons sur les quatre points ci-dessus.

## Simplification

- Ouvrons `longlist.db` sur SQLite et exécutons la commande `.schema` pour vérifier que les trois tables que nous avons vues dans l'exemple précédent sont créées : `authors`, `authored` et `books`.
- Pour sélectionner les livres écrits par Fernanda Melchor, nous écririons cette requête imbriquée.

```sql
SELECT "title" FROM "books"
WHERE "id" IN (
    SELECT "book_id" FROM "authored"
    WHERE "author_id" = (
        SELECT "id" FROM "authors"
        WHERE "name" = 'Fernanda Melchor'
    )
);
```
- La requête ci-dessus est complexe — il y a trois requêtes `SELECT` dans la requête imbriquée. Pour simplifier cela, utilisons d'abord `JOIN` pour créer une vue contenant les auteurs et leurs livres.
- Dans un nouveau terminal, connectons-nous à nouveau à `longlist.db`, et exécutons la requête suivante.

```sql
SELECT "name", "title" FROM "authors"
JOIN "authored" ON "authors"."id" = "authored"."author_id"
JOIN "books" ON "books"."id" = "authored"."book_id";
```

    - Notez qu'il est important de spécifier comment deux tables sont jointes, ou les colonnes sur lesquelles elles sont jointes.
    - Astuce : La colonne de clé primaire d'une table est généralement jointe à la colonne de clé étrangère correspondante de l'autre table !
    - L'exécution de cette requête affichera une table contenant tous les noms d'auteurs à côté des titres des livres qu'ils ont écrits.
- Pour enregistrer la table virtuelle créée à l'étape précédente en tant que vue, nous devons modifier la requête.

```sql
CREATE VIEW "longlist" AS
SELECT "name", "title" FROM "authors"
JOIN "authored" ON "authors"."id" = "authored"."author_id"
JOIN "books" ON "books"."id" = "authored"."book_id";
```

La vue créée ici s'appelle `longlist`. Cette vue peut maintenant être utilisée exactement comme nous utiliserions une table en SQL.
- Écrivons une requête pour voir toutes les données dans cette vue.

```sql
SELECT * FROM "longlist";
```
- En utilisant cette vue, nous pouvons considérablement simplifier la requête nécessaire pour trouver les livres écrits par Fernanda Melchor.

```sql
SELECT "title" FROM "longlist" WHERE "name" = 'Fernanda Melchor';
```
- Une vue, étant une table virtuelle, ne consomme pas beaucoup plus d'espace disque pour être créée. Les données d'une vue sont toujours stockées dans les tables sous-jacentes, mais restent accessibles via cette vue simplifiée.

### Questions

> Peut-on manipuler les vues pour les trier ou les afficher différemment ?

- Oui, nous pouvons trier les livres dans une vue de la même manière que nous le ferions dans une table.
    - Par exemple, affichons les données de la vue `longlist`, triées par les titres des livres.

```sql
SELECT "name", "title"
FROM  "longlist"
ORDER BY "title";
```
    - Nous pourrions également avoir la vue elle-même triée. Nous pouvons le faire en incluant une clause `ORDER BY` dans la requête utilisée pour créer la vue.

## Agrégation

- Dans `longlist.db`, nous avons une table contenant des notes individuelles attribuées à chaque livre. Dans les semaines précédentes, nous avons vu comment trouver la note moyenne de chaque livre, arrondie à 2 décimales.

```sql
SELECT "book_id", ROUND(AVG("rating"), 2) AS "rating"
FROM "ratings"
GROUP BY "book_id";
```
- Les résultats de la requête ci-dessus peuvent être rendus plus utiles en affichant le titre de chaque livre, et peut-être l'année où chaque livre a été présélectionné. Ces informations sont présentes dans la table `books`.

```sql
SELECT "book_id", "title", "year", ROUND(AVG("rating"), 2) AS "rating"
FROM "ratings"
JOIN "books" ON "ratings"."book_id" = "books"."id"
GROUP BY "book_id";
```

    - Ici, nous utilisons un `JOIN` pour combiner des informations des tables `ratings` et `books`, en les joignant sur la colonne de l'ID du livre.
    - Remarquez l'ordre des opérations dans cette requête — en particulier, le placement de l'opération `GROUP BY` à la fin de la requête après la jonction des deux tables.
- Ces données agrégées peuvent être stockées dans une vue.

```sql
CREATE VIEW "average_book_ratings" AS
SELECT "book_id" AS "id", "title", "year", ROUND(AVG("rating"), 2) AS "rating"
FROM "ratings"
JOIN "books" ON "ratings"."book_id" = "books"."id"
GROUP BY "book_id";
```

    - Maintenant, regardons les données dans cette vue.

```sql
SELECT * FROM "average_book_ratings";
```
- En ajoutant plus de données à la table `ratings`, pour obtenir un agrégat à jour, il suffit de requêter à nouveau la vue en utilisant une commande `SELECT` comme ci-dessus !
- Chaque fois qu'une vue est créée, elle est ajoutée au schéma. Nous pouvons vérifier cela en exécutant `.schema` pour observer que `longlist` et `average_book_ratings` font désormais partie du schéma de cette base de données.
- Pour créer des vues temporaires qui ne sont pas stockées dans le schéma de la base de données, nous pouvons utiliser `CREATE TEMPORARY VIEW`. Cette commande crée une vue qui existe uniquement pendant la durée de notre connexion avec la base de données.
- Pour trouver la note moyenne des livres par année, nous pouvons utiliser la vue que nous avons déjà créée.

```sql
SELECT "year", ROUND(AVG("rating"), 2) AS "rating"
FROM "average_book_ratings"
GROUP BY "year";
```

Remarquez que nous sélectionnons la colonne `rating` de `average_book_ratings`, qui contient déjà les notes moyennes par livre. Ensuite, nous les regroupons par année et calculons à nouveau les notes moyennes, ce qui nous donne la note moyenne par année !
- Nous pouvons stocker les résultats dans une vue temporaire.

```sql
CREATE TEMPORARY VIEW "average_ratings_by_year" AS
SELECT "year", ROUND(AVG("rating"), 2) AS "rating" FROM "average_book_ratings"
GROUP BY "year";
```

### Questions

> Les vues temporaires peuvent-elles être utilisées pour tester si une requête fonctionne ou non ?

- Oui, c'est un excellent cas d'utilisation pour les vues temporaires ! Pour généraliser un peu, les vues temporaires sont utilisées lorsque nous voulons organiser des données d'une certaine manière sans réellement stocker cette organisation à long terme.

## Expression Commune de Table (CTE)

- Une vue régulière existe pour toujours dans notre schéma de base de données. Une vue temporaire existe pendant la durée de notre connexion avec la base de données. Une CTE est une vue qui existe pour une seule requête.
- Recréons la vue contenant les notes moyennes des livres par année en utilisant une CTE au lieu d'une vue temporaire. Tout d'abord, nous devons supprimer la vue temporaire existante afin de pouvoir réutiliser le nom `average_book_ratings`.

```sql
DROP VIEW "average_book_ratings";
```
- Ensuite, nous créons une CTE contenant les notes moyennes par livre. Nous utilisons ensuite les notes moyennes par livre pour calculer les notes moyennes par année, de la même manière que nous l'avons fait précédemment.

```sql
WITH "average_book_ratings" AS (
    SELECT "book_id", "title", "year", ROUND(AVG("rating"), 2) AS "rating" FROM "ratings"
    JOIN "books" ON "ratings"."book_id" = "books"."id"
    GROUP BY "book_id"
)
SELECT "year" ROUND(AVG("rating"), 2) AS "rating" FROM "average_book_ratings"
GROUP BY "year";
```

## Partitionnement

- Les vues peuvent être utilisées pour partitionner des données, ou les diviser en morceaux plus petits qui seront utiles pour nous ou une application. Par exemple, le site web du International Booker Prize a une page de livres présélectionnés pour chaque année où le prix a été décerné. Cependant, notre base de données stocke tous les livres présélectionnés dans une seule table. Pour créer le site web, ou à d'autres fins, il pourrait être utile d'avoir une table (ou vue) différente de livres pour chaque année.
- Créons une vue pour stocker les livres présélectionnés en 2022.

```sql
CREATE VIEW "2022" AS
SELECT "id", "title" FROM "books"
WHERE "year" = 2022;
```

    - Nous pouvons également voir les données dans cette vue.

```sql
SELECT * FROM "2022";
```

### Questions

> Les vues peuvent-elles être mises à jour ?

- Non, car les vues ne contiennent pas de données comme les tables. Les vues extraient en fait des données des tables sous-jacentes chaque fois qu'elles sont interrogées. Cela signifie que lorsqu'une table sous-jacente est mise à jour, la prochaine fois que la vue est interrogée, elle affichera les données mises à jour de la table !

## Sécurisation

- Les vues peuvent être utilisées pour améliorer la sécurité de la base de données en limitant l'accès à certaines données.
- Prenons l'exemple d'une base de données d'une entreprise de covoiturage avec une table `rides` qui ressemble à ce qui suit.

!["Table des trajets contenant la destination, l'origine et les passagers"](https://cs50.harvard.edu/sql/2024/notes/4/images/41.jpg)
- Si nous devions donner ces données à un analyste, dont le travail est de trouver les itinéraires de trajet les plus populaires, il serait inutile et en fait, non sécurisé de lui donner les noms des passagers individuels. Les noms des passagers sont probablement classés comme informations personnelles identifiables (PII) que les entreprises ne sont pas autorisées à partager sans discernement.
- Les vues peuvent être utiles dans cette situation — nous pouvons partager avec l'analyste une vue contenant l'origine et la destination des trajets, mais pas les noms des passagers.
- Pour essayer cela, ouvrons `rideshare.db` dans notre terminal. L'exécution de `.schema` devrait révéler une table appelée `rides` dans cette base de données.
- Nous pouvons créer une vue avec les colonnes pertinentes, tout en omettant complètement la colonne `rider`. Mais nous irons un peu plus loin ici, et créerons une colonne `rider` pour afficher un passager anonyme pour chaque ligne du tableau. Cela indiquera à l'analyste que bien que nous ayons des noms de passagers dans la base de données, les noms ont été anonymisés pour des raisons de sécurité.

```sql
CREATE VIEW "analysis" AS
SELECT "id", "origin", "destination", 'Anonymous' AS "rider"
FROM "rides";
```

    - Nous pouvons interroger cette vue pour nous assurer qu'elle est sécurisée.

```sql
SELECT * FROM "analysis";
```
- Bien que nous puissions créer une vue qui anonymise les données, SQLite ne permet pas le contrôle d'accès. Cela signifie que notre analyste pourrait simplement interroger la table `rides` d'origine et voir tous les noms de passagers que nous avons pris soin d'omettre dans la vue `analysis`.

## Suppressions Logiques (Soft Deletions)

- Comme nous l'avons vu dans les semaines précédentes, une suppression logique implique de marquer une ligne comme supprimée au lieu de la retirer de la table.
- Par exemple, une œuvre d'art intitulée "Farmers working at dawn" est marquée comme supprimée de la table `collections` en changeant la valeur de la colonne `deleted` de 0 à 1.

!["Suppression logique d'une ligne en changeant la valeur "deleted" de 0 à 1"](https://cs50.harvard.edu/sql/2024/notes/4/images/46.jpg)
- Nous pouvons imaginer créer une vue pour afficher uniquement l'art qui n'est pas supprimé.
- Pour essayer cela, ouvrons `mfa.db` dans notre terminal. La table `collections` n'a pas encore de colonne `deleted`, nous devons donc l'ajouter. La valeur par défaut ici sera 0, pour indiquer que la ligne n'est pas supprimée.

```sql
ALTER TABLE "collections"
ADD COLUMN "deleted" INTEGER DEFAULT 0;
```
- Maintenant, effectuons une suppression logique sur l'œuvre "Farmers working at dawn", en la mettant à jour pour avoir 1 dans la colonne `deleted`.

```sql
UPDATE "collections"
SET "deleted" = 1
WHERE "title" = 'Farmers working at dawn';
```
- Nous pouvons créer une vue pour afficher les informations sur les lignes qui ne sont pas supprimées.

```sql
CREATE VIEW "current_collections" AS
SELECT "id", "title", "accession_number", "acquired"
FROM "collections"
WHERE "deleted" = 0;
```

    - Nous pouvons afficher les données dans cette vue pour vérifier que "Farmers working at dawn" n'est pas présent.

```sql
SELECT * FROM "current_collections";
```
    - Lors de la suppression logique d'une ligne de la table sous-jacente `collections`, elle sera retirée de la vue `current_collections` lors de toute interrogation ultérieure.
- Nous savons déjà qu'il n'est pas possible d'insérer des données dans une vue ou de les supprimer d'une vue. Cependant, nous pouvons configurer un déclencheur qui insère ou supprime de la table sous-jacente ! Le déclencheur `INSTEAD OF` nous permet de le faire.

```sql
CREATE TRIGGER "delete"
INSTEAD OF DELETE ON "current_collections"
FOR EACH ROW
BEGIN
    UPDATE "collections" SET "deleted" = 1
    WHERE "id" = OLD."id";
END;
```

    - Chaque fois que nous essayons de supprimer des lignes de la vue, ce déclencheur mettra à jour la colonne `deleted` de la ligne dans la table sous-jacente `collections`, complétant ainsi la suppression logique.
    - Nous utilisons le mot-clé `OLD` dans notre clause de mise à jour pour indiquer que l'ID de la ligne mise à jour dans `collections` doit être le même que l'ID de la ligne que nous essayons de supprimer de `current_collections`.
- Maintenant, nous pouvons supprimer une ligne de la vue `current_collections`.

```sql
DELETE FROM "current_collections"
WHERE "title" = 'Imaginative landscape';
```

Nous pouvons vérifier que cela a fonctionné en interrogeant la vue.

```sql
SELECT * FROM "current_collections";
```
- De même, nous pouvons créer un déclencheur qui insère des données dans la table sous-jacente lorsque nous essayons de les insérer dans une vue.
- Il y a deux situations à considérer ici. Nous pourrions essayer d'insérer dans une vue une ligne qui existe déjà dans la table sous-jacente, mais qui a été supprimée logiquement. Nous pouvons écrire le déclencheur suivant pour gérer cette situation.

```sql
CREATE TRIGGER "insert_when_exists"
INSTEAD OF INSERT ON "current_collections"
FOR EACH ROW
WHEN NEW."accession_number" IN (
    SELECT "accession_number" FROM "collections"
)
BEGIN
    UPDATE "collections"
    SET "deleted" = 0
    WHERE "accession_number" = NEW."accession_number";
END;
```

    - Le mot-clé `WHEN` est utilisé pour vérifier si le numéro d'accession de l'œuvre existe déjà dans la table `collections`. Cela fonctionne parce qu'un numéro d'accession, comme nous le savons des semaines précédentes, identifie de manière unique chaque œuvre d'art dans cette table.
    - Si l'œuvre existe dans la table sous-jacente, nous définissons sa valeur `deleted` sur 0, indiquant une annulation de la suppression logique.
- La deuxième situation se produit lorsque nous essayons d'insérer une ligne qui n'existe pas dans la table sous-jacente. Le déclencheur suivant gère cette situation.

```sql
CREATE TRIGGER "insert_when_new"
INSTEAD OF INSERT ON "current_collections"
FOR EACH ROW
WHEN NEW."accession_number" NOT IN (
    SELECT "accession_number" FROM "collections"
)
BEGIN
    INSERT INTO "collections" ("title", "accession_number", "acquired")
    VALUES (NEW."title", NEW."accession_number", NEW."acquired");
END;
```

    - Lorsque le numéro d'accession des données insérées n'est pas déjà présent dans `collections`, il insère la ligne dans la table.
