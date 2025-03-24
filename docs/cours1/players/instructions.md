# Joueurs

![Équipe des Boston Americans de 1901](https://cs50.harvard.edu/sql/2024/psets/0/players/red_sox.jpeg)  
L'équipe des Boston Americans de 1901 (maintenant les Red Sox)

## Problème à Résoudre

Si vous n'êtes pas familier, le baseball est un sport populaire dans lequel deux équipes de 9 joueurs alternent entre la batte (frapper une balle) et le champ (attraper et lancer les balles frappées). Des points ("runs") sont marqués lorsqu'un joueur de l'équipe à la batte frappe une balle et touche toutes les bases avant que les joueurs de l'équipe au champ n'aient la chance de le retirer ("out"). Le baseball est sans doute le plus populaire aux États-Unis et au Canada, où la MLB (Major League Baseball) sert d'association professionnelle pour les joueurs depuis 1876.

Dans une base de données appelée `players.db`, en utilisant une table appelée `players`, répondez à des questions sur les joueurs de la MLB qui ont joué de 1871 à 2023.

## Démonstration

```sql
$ sqlite3 players.db                                                     sqlite> .tables                                                         players                                                                 sqlite> SELECT "first_name", "last_name", "debut"                       	...> FROM "players"                                                      ...> LIMIT 10;     
```



## Code de Distribution

Pour ce problème, vous devrez utiliser `players.db`, ainsi que plusieurs fichiers `.sql` dans lesquels vous écrirez vos requêtes.


## Schéma

Dans `players.db`, vous trouverez une seule table, `players`. Dans la table `players`, vous trouverez les colonnes suivantes :

- `id`, qui identifie de manière unique chaque ligne (joueur) dans la table
- `first_name`, qui est le prénom du joueur
- `last_name`, qui est le nom de famille du joueur
- `bats`, qui est le côté (`R` pour droit ou `L` pour gauche) où le joueur frappe
- `throws`, qui est la main (`R` pour droit ou `L` pour gauche) avec laquelle le joueur lance
- `weight`, qui est le poids du joueur en livres
- `height`, qui est la taille du joueur en pouces
- `debut`, qui est la date (exprimée sous la forme `YYYY-MM-DD`) à laquelle le joueur a commencé sa carrière en MLB
- `final_game`, qui est la date (exprimée sous la forme `YYYY-MM-DD`) à laquelle le joueur a joué son dernier match en MLB
- `birth_year`, qui est l'année de naissance du joueur
- `birth_month`, qui est le mois (exprimé en entier) de naissance du joueur
- `birth_day`, qui est le jour de naissance du joueur
- `birth_city`, qui est la ville où le joueur est né
- `birth_state`, qui est l'état où le joueur est né
- `birth_country`, qui est le pays où le joueur est né

## Spécification

1. Dans `1.sql`, écrivez une requête SQL pour trouver la ville natale (y compris la ville, l'état et le pays) de Jackie Robinson.
2. Dans `2.sql`, écrivez une requête SQL pour trouver le côté (par exemple, droit ou gauche) où Babe Ruth frappait.
3. Dans `3.sql`, écrivez une requête SQL pour trouver les ids des lignes pour lesquelles une valeur dans la colonne `debut` est manquante.
4. Dans `4.sql`, écrivez une requête SQL pour trouver les prénoms et noms de famille des joueurs qui ne sont pas nés aux États-Unis. Triez les résultats par ordre alphabétique par prénom, puis par nom de famille.
5. Dans `5.sql`, écrivez une requête SQL pour retourner les prénoms et noms de famille de tous les batteurs droitiers. Triez les résultats par ordre alphabétique par prénom, puis par nom de famille.
6. Dans `6.sql`, écrivez une requête SQL pour retourner le prénom, le nom de famille et la date de début des joueurs nés à Pittsburgh, Pennsylvanie (PA). Triez les résultats d'abord par date de début—du plus récent au plus ancien—puis par ordre alphabétique par prénom, suivi du nom de famille.
7. Dans `7.sql`, écrivez une requête SQL pour compter le nombre de joueurs qui frappaient (ou ont frappé) de la main droite et lançaient (ou ont lancé) de la main gauche, ou vice versa.
8. Dans `8.sql`, écrivez une requête SQL pour trouver la taille et le poids moyens, arrondis à deux décimales, des joueurs de baseball qui ont débuté le ou après le 1er janvier 2000. Retournez les colonnes avec les noms "Average Height" et "Average Weight", respectivement.
9. Dans `9.sql`, écrivez une requête SQL pour trouver les joueurs qui ont joué leur dernier match en MLB en 2022. Triez les résultats par ordre alphabétique par prénom, puis par nom de famille.
10. Dans `10.sql`, écrivez une requête SQL pour répondre à une question de votre choix. Cette requête doit :
    - Utiliser `AS` pour renommer une colonne
    - Impliquer au moins une condition, en utilisant `WHERE`
    - Trier par au moins une colonne en utilisant `ORDER BY`

Vous sentez-vous plus à l'aise ?

Vous pouvez éventuellement tenter les requêtes suivantes, qui peuvent nécessiter des connaissances avancées !

1. Écrivez une seule requête SQL pour lister les prénoms et noms de famille de tous les joueurs de taille supérieure à la moyenne, triés du plus grand au plus petit, puis par prénom et nom de famille.

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
$ cat FILENAME | sqlite3 players.db > output.txt
```

pour rediriger la sortie de la requête vers un fichier texte appelé `output.txt`. (Cela peut être utile pour vérifier combien de lignes sont retournées par votre requête !)

## Comment Tester



- L'exécution de `1.sql` donne une table avec 3 colonnes et 1 ligne.
- L'exécution de `2.sql` donne une table avec 1 colonne et 1 ligne.
- L'exécution de `3.sql` donne une table avec 1 colonne et 213 lignes.
- L'exécution de `4.sql` donne une table avec 2 colonnes et 2814 lignes.
- L'exécution de `5.sql` donne une table avec 2 colonnes et 12878 lignes.
- L'exécution de `6.sql` donne une table avec 3 colonnes et 134 lignes.
- L'exécution de `7.sql` donne une table avec 1 colonne et 1 ligne.
- L'exécution de `8.sql` donne une table avec 2 colonnes et 1 ligne.
- L'exécution de `9.sql` donne une table avec 2 colonnes et 516 lignes.

`10.sql` dépend de vous !

Notez que les comptes de lignes n'incluent pas les lignes d'en-tête qui ne montrent que les noms de colonnes.

