[# From the Deep](https://cs50.harvard.edu/sql/2024/psets/6/deep/)

![From the Deep](https://cs50.harvard.edu/sql/2024/psets/6/deep/deep.jpg)

## Problème à Résoudre

Vous êtes un chercheur opérant un sous-marin télécommandé, l'AquaByte Explorer, qui collecte en continu des observations depuis le fond de l'océan. (AquaByte, bien que fictif, est un peu comme le vrai SuBastion !). AquaByte envoie des données depuis les profondeurs, les stockant dans une base de données située sur plusieurs bateaux à la surface de l'océan.

Dans un fichier nommé `answers.md`, votre tâche consiste à analyser les compromis (trade-offs) dans quelques conceptions potentielles pour le système de base de données distribué d'AquaByte !


## Contexte

AquaByte est capable d'envoyer plusieurs milliers d'observations par minute, souvent à des moments particuliers de la journée. Par exemple, AquaByte observe le plus activement en soirée et tôt le matin, lorsque certains poissons peuvent être vus plus fréquemment.

Chaque ligne de données envoyée à la surface par AquaByte est étiquetée avec une clé primaire : dans ce cas, l'horodatage exact de l'observation au format suivant : `YYYY-MM-DD HH:MM:SS.SSS`. Il s'agit du format ISO8601, si vous êtes curieux !

## Collecte d'Observations

Pour simplifier, supposons qu'AquaByte a envoyé les 6 observations suivantes au cours du 1er novembre 2023. Pour la visualisation, chaque observation est représentée par un rectangle bleu étiqueté par sa clé primaire (l'horodatage auquel l'observation a été prise).

![Timestamps](https://cs50.harvard.edu/sql/2024/psets/6/deep/timestamps.jpg)

Pour vérifier votre compréhension, à quel moment la plupart des observations d'AquaByte ont-elles eu lieu ?

- Entre minuit et 1h du matin le 1er novembre 2023

  C'est correct !

- Entre midi et 13h le 1er novembre 2023

  Pas tout à fait.

- Entre 16h et 17h le 1er novembre 2023

  Pas tout à fait.

## Partitions

AquaByte prévoit d'envoyer ses six observations à un réseau de bateaux à la surface : Bateau A, Bateau B et Bateau C. Plus de bateaux permettent à AquaByte d'envoyer plus de données que ce qu'un seul bateau pourrait stocker. Cependant, plus de bateaux entraînent plus de complexité : en supposant que les données, une fois stockées, ne doivent pas être déplacées, à quel bateau AquaByte doit-il envoyer chaque observation ? Il s'agit en fait d'un problème de partitionnement des données.

### Partitionnement Aléatoire

Une approche consiste pour AquaByte à envoyer ses observations de manière aléatoire à chaque bateau, comme illustré ci-dessous. Avec les questions suivantes, analysez les compromis techniques de cette décision de conception.

![Random Partitioning](https://cs50.harvard.edu/sql/2024/psets/6/deep/random.jpg)

Considérez les deux questions suivantes. Une fois que vous avez une idée en tête, choisissez la réponse qui correspond le mieux à votre réflexion en cliquant sur le menu déroulant approprié.

Les observations seront-elles probablement réparties uniformément sur tous les bateaux, même si AquaByte collecte le plus souvent des observations entre minuit et 1h ? Pourquoi ou pourquoi pas ?

- Les observations seront réparties uniformément.

  C'est correct. Les observations seront réparties uniformément car chacune est attribuée de manière aléatoire à l'un des trois bateaux. En d'autres termes, une observation a autant de chances d'être envoyée à n'importe quel bateau. Ainsi, même si AquaByte collecte le plus souvent des observations entre minuit et 1h, les observations seront uniformément réparties parmi les bateaux.

- Les observations ne seront pas réparties uniformément.

  Pas tout à fait. Les observations seront réparties uniformément car chacune est attribuée de manière aléatoire à l'un des trois bateaux. En d'autres termes, une observation a autant de chances d'être envoyée à n'importe quel bateau. Ainsi, même si AquaByte collecte le plus souvent des observations entre minuit et 1h, les observations seront uniformément réparties parmi les bateaux.

Supposons qu'un chercheur souhaite interroger toutes les observations entre minuit et 1h. Sur combien de bateaux devra-t-il exécuter la requête ?

- Le chercheur devra exécuter la requête sur tous les bateaux.

  C'est correct. Comme les observations sont attribuées de manière aléatoire à n'importe quel bateau, chaque observation entre minuit et 1h pourrait se trouver sur l'un des trois bateaux. Si une requête n'est exécutée que sur un seul bateau, il y a une chance qu'elle ait manqué des observations, stockées sur d'autres bateaux, qu'elle aurait dû retourner.

- Le chercheur devra exécuter la requête sur seulement certains des bateaux.

  Pas tout à fait. Considérez que chaque observation entre minuit et 1h a autant de chances de se trouver sur l'un des trois bateaux. Donc, si le chercheur exécute la requête sur un seul des bateaux, il y a une chance que la requête ait manqué des observations sur d'autres bateaux.

Sur la base de ce que vous avez appris ci-dessus, des cours et de votre propre intuition, écrivez 2 à 3 phrases qui décrivent à la fois les raisons d'adopter cette approche et les raisons de ne pas l'adopter. Écrivez les phrases dans `answers.md`, dans la section « Random Partitioning ».

### Partitionnement par Heure

Supposons que, pour les raisons que vous avez écrites, l'équipe AquaByte décide de ne pas envoyer les observations d'AquaByte de manière aléatoire à chaque bateau. Au lieu de cela, un membre de l'équipe propose de partitionner les données par heure de la journée. Par exemple,

- Le Bateau A recevra toutes les observations entre 0h et 7h (c'est-à-dire, de minuit à 7h59), inclus.
- Le Bateau B recevra toutes les observations entre 8h et 15h (c'est-à-dire, de 8h00 à 15h59), inclus.
- Le Bateau C recevra toutes les observations entre 16h et 23h (c'est-à-dire, de 16h00 à 23h59), inclus.

Avec les questions suivantes, analysez les compromis techniques de cette décision de conception.

![Partitioning by Hour](https://cs50.harvard.edu/sql/2024/psets/6/deep/hour.jpg)

Considérez les deux questions suivantes. Une fois que vous avez une idée en tête, choisissez la réponse qui correspond le mieux à votre réflexion en cliquant sur le menu déroulant approprié.

Les observations seront-elles probablement réparties uniformément sur tous les bateaux, même si AquaByte collecte le plus souvent des observations entre minuit et 1h ? Pourquoi ou pourquoi pas ?

- Les observations seront réparties uniformément.

  Pas tout à fait. Les observations ne seront pas réparties uniformément si AquaByte collecte le plus souvent des observations entre minuit et 1h. Puisque la plupart des observations sont collectées entre minuit et 1h, et que le Bateau A recevra toutes les observations entre 0h et 7h (c'est-à-dire, de minuit à 7h59), inclus, le Bateau A recevra la plupart des observations.

- Les observations ne seront pas réparties uniformément.

  C'est correct. Puisque la plupart des observations sont collectées entre minuit et 1h, et que le Bateau A recevra toutes les observations entre 0h et 7h (c'est-à-dire, de minuit à 7h59), inclus, le Bateau A recevra la plupart des observations.

Supposons qu'un chercheur souhaite interroger toutes les observations entre minuit et 1h. Sur combien de bateaux devra-t-il exécuter la requête ?

- Le chercheur devra exécuter la requête sur tous les bateaux.

  Pas tout à fait. Le Bateau A recevra toutes les observations entre 0h et 7h (c'est-à-dire, de minuit à 7h59), inclus. Cela signifie que toutes les observations entre minuit et 1h peuvent être trouvées sur le Bateau A.

- Le chercheur devra exécuter la requête sur seulement certains des bateaux.

  C'est correct. Le Bateau A recevra toutes les observations entre 0h et 7h (c'est-à-dire, de minuit à 7h59), inclus. Cela signifie que toutes les observations entre minuit et 1h peuvent être trouvées sur le Bateau A.

Sur la base de ce que vous avez appris ci-dessus, des cours et de votre propre intuition, écrivez 2 à 3 phrases qui décrivent à la fois les raisons d'adopter cette approche et les raisons de ne pas l'adopter. Écrivez les phrases dans `answers.md`, dans la section « Partitioning by Hour ».

### Partitionnement par Valeur de Hachage

Supposons que, pour les raisons que vous avez identifiées ci-dessus, l'équipe AquaByte décide de ne pas envoyer les observations à certains bateaux en fonction de l'heure à laquelle elles ont été collectées. Au lieu de cela, un membre de l'équipe propose de partitionner les observations par la valeur de hachage (hash value) de leur horodatage.

Si vous n'êtes pas familier avec ce terme, une valeur de hachage est générée par une fonction de hachage. Une fonction de hachage est un algorithme qui prend une entrée (comme un horodatage, par exemple) et, en fonction de l'entrée, produit un nombre arbitraire (la valeur de hachage).

Par exemple, supposons que l'équipe développe une fonction de hachage qui attribue une valeur comprise entre 0 et 1499, inclus, à chaque clé primaire potentielle qu'AquaByte pourrait envoyer au système de données distribué. Voici quelques exemples d'entrées et de sorties :

- L'algorithme calcule la valeur de hachage 45 pour `2023-11-01 00:00:01.020`.
- L'algorithme calcule la valeur de hachage 588 pour `2023-11-01 16:21:59.924`.
- L'algorithme calcule la valeur de hachage 1200 pour `2023-11-01 00:00:04.127`.

Il est important de noter que la fonction de hachage est cohérente : elle calculera toujours la même valeur de hachage pour le même horodatage. La fonction de hachage répartit également les horodatages uniformément sur toutes les valeurs de hachage possibles : c'est-à-dire qu'une seule observation n'a pas plus de chances de se voir attribuer une valeur de hachage qu'une autre. Avec les questions suivantes, analysez les compromis techniques de cette décision de conception.

![Hash Partitioning](https://cs50.harvard.edu/sql/2024/psets/6/deep/hash.jpg)

Considérez les trois questions suivantes. Une fois que vous avez une idée en tête, choisissez la réponse qui correspond le mieux à votre réflexion en cliquant sur le menu déroulant approprié.

Les observations seront-elles probablement réparties uniformément sur tous les bateaux, même si AquaByte collecte le plus souvent des observations entre minuit et 1h ? Pourquoi ou pourquoi pas ?

- Les observations seront réparties uniformément.

  C'est correct. Une seule observation n'a pas plus de chances de se voir attribuer une valeur de hachage qu'une autre, ce qui signifie qu'une seule observation pourrait être envoyée à l'un des trois bateaux disponibles.

- Les observations ne seront pas réparties uniformément.

  Pas tout à fait. Bien qu'AquaByte collecte le plus souvent des observations entre minuit et 1h, une seule observation n'a pas plus de chances de se voir attribuer une valeur de hachage qu'une autre. Une seule observation pourrait être envoyée à l'un des trois bateaux disponibles.

Supposons qu'un chercheur souhaite interroger toutes les observations entre minuit et 1h. Sur combien de bateaux devra-t-il exécuter la requête ?

- Le chercheur devra probablement exécuter la requête sur tous les bateaux.

  C'est correct. Chaque observation dans une plage d'observations pourrait se voir attribuer une valeur de hachage arbitraire : les valeurs de hachage elles-mêmes ne sont pas dans une plage spécifiée. Pour cette raison, la requête serait mieux exécutée sur tous les bateaux.

- Le chercheur devra probablement exécuter la requête sur seulement certains des bateaux.

  Pas tout à fait. Il est certes possible de connaître la valeur de hachage d'une observation spécifique. Mais chaque observation dans une plage d'observations pourrait se voir attribuer une valeur de hachage arbitraire : les valeurs de hachage elles-mêmes ne sont pas dans une plage spécifiée. Pour cette raison, la requête serait mieux exécutée sur tous les bateaux.

Supposons qu'un chercheur souhaite interroger une observation spécifique, qui s'est produite exactement à `2023-11-01 00:00:01.020`. Sur combien de bateaux devra-t-il exécuter la requête ?

- Le chercheur devra exécuter la requête sur tous les bateaux.

  Pas tout à fait. Il est possible de connaître la valeur de hachage d'un horodatage spécifique, ce qui peut indiquer au chercheur où exécuter la requête.

- Le chercheur devra exécuter la requête sur seulement certains des bateaux.

  C'est correct. Il est possible de connaître la valeur de hachage d'un horodatage spécifique, ce qui peut indiquer au chercheur où exécuter la requête.

Sur la base de ce que vous avez appris ci-dessus, des cours et de votre propre intuition, écrivez 2 à 3 phrases qui décrivent à la fois les raisons d'adopter cette approche et les raisons de ne pas l'adopter. Écrivez les phrases dans `answers.md`, dans la section « Partitioning by Hash Value ».

