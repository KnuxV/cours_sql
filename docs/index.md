## 📊 Bienvenue au Cours SQL

## 🗂️ Contenu du Cours

### [Cours 0 : Introduction rapide au monde de la données](cours0/index.md)
Introduction générale sur les données et les bases de données

### [Cours0: Histoire](cours0/histoire.md)

### 📖 [Cours 1 : Les Fondamentaux en SQL](cours1/index.md)
Concepts de base du langage SQL et vous familiarise avec la syntaxe essentielle pour interroger des bases de données. 

- **Thèmes abordés :**
  - Syntaxe de base des requêtes (SELECT, FROM, WHERE)
  - Filtrage conditionnel avec opérateurs de comparaison
  - Tri des résultats (ORDER BY) et limitation (LIMIT)
  - Introduction à SQLite et à l'environnement de ligne de commande

- **Exercices pratiques :**
  - [Cyberchase](cours1/cyberchase/instructions.md) : Série d'exercices guidés pour pratiquer les requêtes SELECT simples dans une base de données de programme TV
  - [Normals](cours1/normals/instructions.md) : Analyse de données météorologiques avec filtrage avancé
  - [Players](cours1/players/instructions.md) : Manipulation de statistiques sportives avec tri et conditions multiples
  - [Views](cours1/views/instructions.md) : Création de vues pour simplifier l'accès aux données complexes
- Lien vers le github/codespace: [Cliquez ici](https://github.com/KnuxV/sql_cours1#)

### 📊 [Cours 2 : Requêtes Avancées et Agrégations](cours2/index.md)
Techniques de requêtes en introduisant les fonctions d'agrégation et les jointures. 

- **Thèmes abordés :**
  - Fonctions d'agrégation pour l'analyse statistique (COUNT, SUM, AVG, MAX, MIN)
  - Regroupement et filtrage de données agrégées (GROUP BY, HAVING)
  - Types de jointures et leurs applications (INNER JOIN, LEFT JOIN, RIGHT JOIN)
  - Sous-requêtes et expressions de table communes

- **Exercices pratiques :**
  - [DESE](cours2/dese/instructions.md) : Analyse complexe de données éducatives pour extraire des métriques de performance
  - [Moneyball](cours2/moneyball/instructions.md) : Application des techniques d'agrégation pour analyser des statistiques de baseball
  - [Packages](cours2/packages/instructions.md) : Gestion des dépendances logicielles avec jointures multiples

### 📈 [Cours 3 : Modélisation et Schémas](cours3/index.md)
Principes de conception de bases de données, la normalisation et la création de schémas efficaces. 

- **Thèmes abordés :**
  - Conception de schémas relationnels
  - Formes normales et processus de normalisation
  - Clés primaires, étrangères et contraintes d'intégrité
  - Diagrammes entité-relation (ER)

- **Exercices pratiques :**
  - [ATL](cours3/atl/instructions.md) : Analyse et optimisation d'un schéma de base de données aéroportuaire
  - [Connect](cours3/connect/instructions.md) : Création d'un modèle relationnel complet à partir de spécifications
  - [Doghnut](cours3/doghnut/instructions.md) : Application des principes de normalisation dans la conception d'un système de gestion

### 🔍 [Cours 4 : Manipulation Avancée des Données](cours4/index.md)
Techniques avancées pour manipuler des ensembles de données complexes.

- **Thèmes abordés :**
  - Sous-requêtes corrélées et non corrélées
  - Analyse de séries temporelles avec fonctions de fenêtrage (WINDOW)
  - Transactions et principes ACID
  - Contrôle de concurrence et verrous

- **Exercices pratiques :**
  - [Don't Panic](cours4/dont-panic/instructions.md) : Système de gestion d'incidents utilisant des transactions et des sous-requêtes
  - [Météorites](cours4/meteorites/instructions.md) : Analyse scientifique de données de météorites avec fonctions de fenêtrage

### 💾 [Cours 5 : Indexation et Performance](cours5/index.md)
Optimisation des performances des bases de données. 
- **Thèmes abordés :**
  - Types d'index et leur fonctionnement interne
  - Analyse des plans d'exécution avec EXPLAIN QUERY PLAN
  - Compromis entre espace et temps lors de l'indexation
  - Techniques d'optimisation des requêtes complexes

- **Exercices pratiques :**
  - [Optimization](cours5/optimization/instructions.md) : Techniques diverses pour améliorer la performance des requêtes
  - [Indexes](cours5/indexes/instructions.md) : Création et utilisation stratégique d'index pour accélérer des requêtes spécifiques

### 🔐 [Cours 6 : Indexation Avancée et Optimisation](cours6/index.md)
Techniques d'optimisation des bases de données en explorant l'indexation avancée, l'analyse des plans d'exécution et les stratégies de performance pour les applications à forte charge.

- **Thèmes abordés :**
  - Indexation avancée et indexation sur plusieurs colonnes
  - Compromis entre espace disque et performance des requêtes
  - Analyse des goulots d'étranglement avec EXPLAIN QUERY PLAN
  - Concurrence et transactions pour l'intégrité des données

- **Exercices pratiques :**
  - [Harvard](cours6/harvard/instructions.md) : Optimisation d'une base de données universitaire de cours avec création d'index stratégiques
  - [Snap](cours6/snap/instructions.md) : Écriture de requêtes efficaces pour une application de messagerie utilisant des index existants

### 🌐 [Cours 7 : Systèmes de Gestion de Bases de Données Avancés](cours7/index.md)
Différents systèmes de gestion de bases de données, leurs caractéristiques spécifiques et les techniques avancées pour la mise à l'échelle des applications.

- **Thèmes abordés :**
  - Différences entre SQLite, MySQL, PostgreSQL et MariaDB
  - Types de données spécialisés comme JSONB dans PostgreSQL
  - Procédures stockées et fonctions personnalisées
  - Stratégies de partitionnement et mise à l'échelle horizontale

- **Exercices pratiques :**
  - [LinkedIn](cours7/linkedin/instructions.md) : Conception d'un schéma complexe inspiré de LinkedIn avec MySQL
  - [From the Deep](cours7/deep/instructions.md) : Implémentation de stratégies de partitionnement pour gérer des données volumineuses
  - [Don't Panic Python](cours7/dont-panic-python/instructions.md) : Interaction avec des bases de données via Python et protection contre les injections SQL



FOR LATER [documentation de connexion aux bases de données](https://documentation.unistra.fr/DNUM/Pedagogie/MAI_VIE/co/connexionApplicationBdD.html) 