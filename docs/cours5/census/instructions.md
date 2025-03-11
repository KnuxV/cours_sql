# [Recenseur](https://cs50.harvard.edu/sql/2024/psets/4/census/)

![Les chaînes de montagnes de l'Himalaya dans le style d'une peinture népalaise](../../assets/nepal.png) « Les chaînes de montagnes de l'Himalaya dans le style d'une peinture népalaise », générée par DALL·E 2

## Problème à résoudre

Vous êtes un recenseur (census taker) travaillant pour le gouvernement népalais. Alors que vous franchissez une dernière colline, votre souffle se coupe à la vue d'un lever de soleil himalayen, projetant une lueur sur le village que vous avez parcouru si loin pour atteindre. Votre guide, un local, s'arrête brusquement. Sous le bruissement constant de vos papiers de recensement (census papers), vous ressentez une démangeaison de curiosité. Après tout, ce n'est pas tous les jours que votre travail vous emmène dans un village comme celui-ci.

Dans `census.db`, traitez vos données en vues (views) que le gouvernement népalais peut utiliser pour la tenue des dossiers.

## Démonstration

```sqlite
sqlite3 census.db
SELECT district, locality, families FROM census;
.quit		
```

## Code de distribution

Pour ce problème, vous devrez télécharger `census.db`, ainsi que quelques fichiers `.sql` dans lesquels vous écrirez vos requêtes (queries).

## Schéma

Dans `census.db`, vous trouverez une seule table, `census`. Dans la table `census`, vous trouverez les colonnes suivantes :

- `id`, qui identifie de manière unique chaque enregistrement de recensement (census record)
- `district`, qui est le nom du district de l'enregistrement de recensement
- `locality`, qui est le nom de la localité de l'enregistrement de recensement au sein du district
- `families`, qui est le nombre de familles associées à l'enregistrement de recensement
- `households`, qui est le nombre total de ménages associés à l'enregistrement de recensement (plusieurs familles peuvent vivre dans le même ménage)
- `population`, qui est la population associée à l'enregistrement de recensement
- `male`, qui est le nombre de personnes associées à l'enregistrement de recensement qui se sont identifiées comme étant de sexe masculin
- `female`, qui est le nombre de personnes associées à l'enregistrement de recensement qui se sont identifiées comme étant de sexe féminin

## Spécification

Dans chacun des fichiers `.sql` correspondants, écrivez une instruction SQL (SQL statement) pour créer chacune des vues (views) suivantes des données dans `census.db`. Notez que, bien que les vues puissent être créées à partir d'autres vues, chacune de vos vues doit être autonome (c'est-à-dire ne pas dépendre d'une vue antérieure).

#### Rural

Dans `rural.sql`, écrivez une instruction SQL pour créer une vue nommée `rural`. Cette vue doit contenir tous les enregistrements de recensement relatifs à une municipalité rurale (identifiée par l'inclusion de « rural » dans leur nom). Assurez-vous que la vue contient toutes les colonnes de la table `census`.

#### Total

Dans `total.sql`, écrivez une instruction SQL pour créer une vue nommée `total`. Cette vue doit contenir les sommes pour chaque colonne numérique dans `census`, à travers tous les districts et localités. Assurez-vous que la vue contient chacune des colonnes suivantes :

- `families`, qui est la somme des familles de chaque localité au Népal.
- `households`, qui est la somme des ménages de chaque localité au Népal.
- `population`, qui est la somme de la population de chaque localité au Népal.
- `male`, qui est la somme des personnes s'identifiant comme étant de sexe masculin de chaque localité au Népal.
- `female`, qui est la somme des personnes s'identifiant comme étant de sexe féminin de chaque localité au Népal.

#### Par District

Dans `by_district.sql`, écrivez une instruction SQL pour créer une vue nommée `by_district`. Cette vue doit contenir les sommes pour chaque colonne numérique dans `census`, regroupées par `district`. Assurez-vous que la vue contient chacune des colonnes suivantes :

- `district`, qui est le nom du district.
- `families`, qui est le nombre total de familles dans le district.
- `households`, qui est le nombre total de ménages dans le district.
- `population`, qui est la population totale du district.
- `male`, qui est le nombre total de personnes s'identifiant comme étant de sexe masculin dans le district.
- `female`, qui est le nombre total de personnes s'identifiant comme étant de sexe féminin dans le district.

#### Le plus peuplé

Dans `most_populated.sql`, écrivez une instruction SQL pour créer une vue nommée `most_populated`. Cette vue doit contenir, dans l'ordre du plus grand au plus petit, les districts les plus peuplés du Népal. Assurez-vous que la vue contient chacune des colonnes suivantes :

- `district`, qui est le nom du district.
- `families`, qui est le nombre total de familles dans le district.
- `households`, qui est le nombre total de ménages dans le district.
- `population`, qui est la population totale du district.
- `male`, qui est le nombre total de personnes s'identifiant comme étant de sexe masculin dans le district.
- `female`, qui est le nombre total de personnes s'identifiant comme étant de sexe féminin dans le district.

## Utilisation

Pour tester vos vues au fur et à mesure que vous les écrivez dans vos fichiers `.sql`, vous pouvez exécuter une requête (query) sur la base de données en exécutant

```
.read FILENAME
```

où `FILENAME` est le nom du fichier contenant votre requête SQL. Par exemple,

```
.read rural.sql
```

Gardez à l'esprit que vous pouvez également utiliser

```
DROP VIEW name;
```

où `name` est le nom de votre vue, pour supprimer une vue avant de la recréer.
