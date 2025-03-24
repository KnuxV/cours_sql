
![Beau Temps, Matin Clair](https://cs50.harvard.edu/sql/2024/psets/0/views/2.jpeg)
Beau Temps, Matin Clair par Katsushika Hokusai

## Démonstration

```sql
$ sqlite3 views.db                                                       sqlite> .tables                                                         views                                                                   sqlite> SELECT "english_title", "artist" FROM "views";  
```



## Problème à Résoudre

De 1830 à 1832, l'artiste japonais Katsushika Hokusai a créé 36 estampes représentant 36 vues différentes du Mont Fuji, un volcan sur l'île de Honshū au Japon. Parmi les œuvres les plus célèbres de la série figurent Beau Temps, Matin Clair et La Grande Vague de Kanagawa. Les estampes sont devenues si influentes qu'un autre artiste japonais de l'époque, Utagawa Hiroshige, a créé sa propre série de 36 estampes, chacune représentant une nouvelle vue du Fuji.

Dans `views.db`, vous trouverez des détails sur les 36 estampes créées respectivement par Hokusai et Hiroshige. Au total, vous disposerez de données sur 72 estampes. Bien que ces estampes soient généralement appréciées pour leur esthétique, la science informatique permet désormais de fournir des informations numériques supplémentaires sur les œuvres d'art. En plus du titre et de l'auteur de chaque estampe, vous trouverez des statistiques couramment utilisées dans l'analyse computationnelle des images : la couleur moyenne, la luminosité, le contraste et l'entropie. Dans les fichiers `.sql` fournis, écrivez des requêtes SQL pour répondre à des questions sur cette base de données de 72 estampes et les statistiques sur leur composition.

### Curieux d'en savoir plus sur la couleur moyenne, la luminosité, le contraste ou l'entropie ?

- **Couleur moyenne** : La couleur obtenue en additionnant les valeurs des canaux rouge, vert et bleu de chaque pixel, puis en divisant chaque somme par le nombre de pixels dans l'image.
- **Luminosité** : Définie comme la clarté ou l'obscurité d'une image, sur une échelle de 0 à 1. Une image blanche mesurerait 1, tandis qu'une image noire mesurerait 0.
- **Contraste** : Défini comme l'étendue des différences de luminosité dans une image, sur une échelle de 0 à 1. Un contraste plus élevé (différences de luminosité plus importantes) permet aux objets de se démarquer. Un contraste plus faible (différences de luminosité plus faibles) signifie que les objets ne se démarquent pas autant.
- **Entropie** : Une mesure de l'aléatoire d'une image, souvent utilisée pour caractériser la complexité d'une œuvre d'art. Une entropie de 10 est assez élevée, tandis qu'une entropie de 0 est très faible.

## Distribution Code

Pour ce problème, vous devrez accéder à `views.db`, ainsi que plusieurs fichiers `.sql` dans lesquels vous écrirez vos requêtes.


## Schéma

Dans `views.db`, vous trouverez une seule table, `views`. Dans la table `views`, vous trouverez les colonnes suivantes :

- `id` : Identifie de manière unique chaque ligne (estampe) de la table.
- `print_number` : Identifie le numéro de l'estampe dans la série de Hokusai ou Hiroshige.
- `english_title` : Le titre en anglais de l'estampe.
- `japanese_title` : Le titre en japonais de l'estampe.
- `artist` : Le nom de famille de l'artiste de l'estampe.
- `average_color` : La représentation hexadécimale de la couleur obtenue en faisant la moyenne des couleurs de chaque pixel de l'image.
- `brightness` : Un nombre correspondant à la luminosité ou à l'obscurité globale de l'image.
- `contrast` : Un nombre représentant l'étendue de la différence entre les zones claires et sombres de l'image.
- `entropy` : Une mesure utilisée pour quantifier la complexité de l'œuvre d'art.

## Spécification

Pour chacune des questions suivantes, vous devez écrire une seule requête SQL qui produit les résultats spécifiés par chaque problème. Votre réponse doit prendre la forme d'une seule requête SQL. Vous ne devez pas faire d'hypothèses sur les `id` de certaines observations : vos requêtes doivent être exactes même si l'`id` d'une observation particulière était différent. Enfin, chaque requête doit retourner uniquement les données nécessaires pour répondre à la question.

1. Dans `1.sql`, écrivez une requête SQL qui pourrait intéresser un traducteur : listez, côte à côte, le titre japonais et le titre anglais de chaque estampe. Assurez-vous que le titre japonais est la première colonne, suivi du titre anglais.
2. Dans `2.sql`, écrivez une requête SQL pour lister les couleurs moyennes des estampes de Hokusai dont le titre anglais contient "rivière". (À titre indicatif, ont-elles une teinte de bleu ?)
3. Dans `3.sql`, écrivez une requête SQL pour compter combien d'estampes de Hokusai contiennent "Fuji" dans le titre anglais. Bien que toutes les estampes de Hokusai se soient concentrées sur le Mont Fuji, combien ont inclus "Fuji" dans le titre ?
4. Dans `4.sql`, écrivez une requête SQL pour compter combien d'estampes de Hiroshige ont des titres anglais qui font référence à la "Capitale de l'Est". Les estampes de Hiroshige ont été créées pendant la période "Edo" du Japon, en référence à la capitale orientale d'Edo, aujourd'hui Tokyo.
5. Dans `5.sql`, écrivez une requête SQL pour trouver la valeur de contraste la plus élevée des estampes de Hokusai. Nommez la colonne "Contraste Maximum". L'estampe de Hokusai la plus contrastée a-t-elle vraiment beaucoup de contraste ?
6. Dans `6.sql`, écrivez une requête SQL pour trouver l'entropie moyenne des estampes de Hiroshige, arrondie à deux décimales. Appelez la colonne résultante "Entropie Moyenne de Hiroshige".
7. Dans `7.sql`, écrivez une requête SQL pour lister les titres anglais des 5 estampes les plus lumineuses de Hiroshige, de la plus lumineuse à la moins lumineuse. Comparez-les à cette liste sur Wikipedia pour voir si vos résultats correspondent à l'esthétique de l'estampe.
8. Dans `8.sql`, écrivez une requête SQL pour lister les titres anglais des 5 estampes de Hokusai ayant le moins de contraste, du moins contrasté au plus contrasté. Comparez-les à cette liste sur Wikipedia pour voir si vos résultats correspondent à l'esthétique de l'estampe.
9. Dans `9.sql`, écrivez une requête SQL pour trouver le titre anglais et l'artiste de l'estampe ayant la luminosité la plus élevée.
10. Dans `10.sql`, écrivez une requête SQL pour répondre à une question de votre choix sur les estampes. La requête doit :
    - Utiliser `AS` pour renommer une colonne.
    - Impliquer au moins une condition, en utilisant `WHERE`.
    - Trier par au moins une colonne, en utilisant `ORDER BY`.

## Utilisation

Pour tester vos requêtes au fur et à mesure que vous les écrivez dans vos fichiers `.sql`, vous pouvez interroger la base de données en exécutant :

```
.read FILENAME
```

où `FILENAME` est le nom du fichier contenant votre requête SQL. Par exemple :

```
.read 1.sql
```

Vous pouvez également exécuter :

```
$ cat FILENAME | sqlite3 views.db > output.txt
```

pour rediriger la sortie de la requête vers un fichier texte nommé `output.txt`. (Cela peut être utile pour vérifier combien de lignes sont retournées par votre requête !)

## Comment Tester


- L'exécution de `1.sql` produit une table avec 2 colonnes et 72 lignes.
- L'exécution de `2.sql` produit une table avec 1 colonne et 3 lignes.
- L'exécution de `3.sql` produit une table avec 1 colonne et 1 ligne.
- L'exécution de `4.sql` produit une table avec 1 colonne et 1 ligne.
- L'exécution de `5.sql` produit une table avec 1 colonne et 1 ligne.
- L'exécution de `6.sql` produit une table avec 1 colonne et 1 ligne.
- L'exécution de `7.sql` produit une table avec 1 colonne et 5 lignes.
- L'exécution de `8.sql` produit une table avec 1 colonne et 5 lignes.
- L'exécution de `9.sql` produit une table avec 2 colonnes et 1 ligne.

`10.sql` dépend de vous !

Notez que le nombre de lignes ne comprend pas les lignes d'en-tête qui ne montrent que les noms des colonnes.