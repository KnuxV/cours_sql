# Introduction aux Données et aux Bases de Données

## 1. Qu'est-ce qu'une Donnée ?

### Définition
Une donnée (data) est une information brute, non interprétée, qui peut être stockée et traitée par un ordinateur. Elle représente des faits, des concepts, des mesures ou des instructions sous une forme qui peut être communiquée, interprétée et traitée.

### Caractéristiques des données
- **Objectivité** : Les données sont des faits bruts, sans interprétation ni contexte
- **Atomicité** : Une donnée est généralement la plus petite unité d'information utilisable
- **Persistance** : Les données peuvent être stockées pour une utilisation ultérieure
- **Transformabilité** : Elles peuvent être transformées en information utile après traitement

### Types de valeurs de données
1. **Données numériques (Numeric Data)**
   - **Entiers (Integers)** (ex: 1, 42, -7) : nombres sans partie décimale
   - **Réels/Flottants (Floats)** (ex: 3.14, 0.5, -2.75) : nombres avec partie décimale
   - **Booléens (Booleans)** (Vrai/Faux, 1/0) : valeurs binaires représentant une condition

2. **Données alphanumériques (Alphanumeric Data)**
   - **Chaînes de caractères (Strings)** (ex: "Bonjour", "Paris", "A123") : séquences de caractères
   - **Caractères (Characters)** (ex: 'A', '9', '&') : symboles individuels

3. **Données temporelles (Temporal Data)**
   - **Dates** (ex: 2023-10-01) : jour, mois, année
   - **Heures (Times)** (ex: 14:30:00) : heure, minute, seconde
   - **Horodatages (Timestamps)** (ex: 2023-10-01 14:30:00) : combinaison date et heure

4. **Données spéciales (Special Data)**
   - **BLOB (Binary Large Object)** : images, fichiers audio, vidéos
   - **Données géospatiales (Geospatial Data)** : coordonnées, formes, régions
   - **Données JSON/XML** : structures hiérarchiques de données

### Exemples concrets de données
- **Informations personnelles** :
  - Nom : "Alice Dupont"
  - Âge : 30
  - Adresse email : alice.dupont@example.com
  - Numéro de téléphone : +33 6 12 34 56 78

- **Données de transaction** :
  - Identifiant de transaction : TX-12345
  - Date : 2023-10-01
  - Montant : 150,75 €
  - Méthode de paiement : "Carte de crédit"

- **Données météorologiques** :
  - Température : 22,5°C
  - Humidité : 65%
  - Vitesse du vent : 12 km/h
  - Précipitations : 0 mm

- **Données de produit** :
  - Code produit : "PRD-789"
  - Nom : "Smartphone Galaxy S23"
  - Prix : 899,99 €
  - Stock disponible : 45 unités

### Le cycle de vie des données
1. **Création/Acquisition** : collecte de données brutes
2. **Stockage** : enregistrement dans un support (disque dur, base de données)
3. **Traitement** : nettoyage, transformation, agrégation
4. **Analyse** : extraction d'informations utiles
5. **Visualisation/Utilisation** : présentation des résultats
6. **Archivage/Suppression** : conservation à long terme ou effacement

## 2. Types de Bases de Données

Une base de données (database) est une collection organisée de données structurées, stockées électroniquement dans un système informatique. Elle est conçue pour stocker, gérer et récupérer efficacement des informations.

### a. Bases de Données Relationnelles (Relational Databases)

#### Structure et principes
- **Tables** (ou relations) : les données sont organisées en tableaux composés de lignes (enregistrements) et de colonnes (attributs)
- **Schéma** : structure prédéfinie qui spécifie les types de données pour chaque colonne
- **Clés primaires (Primary Keys)** : identifiants uniques pour chaque enregistrement
- **Clés étrangères (Foreign Keys)** : références aux clés primaires d'autres tables, créant des relations
- **Normalisation** : processus d'organisation des données pour réduire la redondance

#### Types de relations
- **One-to-One (1:1)** : Une ligne d'une table est associée à une seule ligne d'une autre table.
  - Exemple : Une personne a un seul passeport, et un passeport appartient à une seule personne.
- **One-to-Many (1:N)** : Une ligne d'une table est associée à plusieurs lignes d'une autre table.
  - Exemple : Un auteur peut écrire plusieurs livres, mais chaque livre a un seul auteur.
- **Many-to-Many (N:N)** : Plusieurs lignes d'une table sont associées à plusieurs lignes d'une autre table.
  - Exemple : Des étudiants peuvent s'inscrire à plusieurs cours, et chaque cours peut avoir plusieurs étudiants.

#### Caractéristiques principales

- **Langage SQL (Structured Query Language)** pour interagir avec les données
- **Jointures (Joins)** entre tables pour combiner des données connexes
- **Contraintes d'intégrité** pour assurer la qualité des données

#### Exemples de SGBDR populaires
- **MySQL** : open-source, très utilisé pour les applications web
- **PostgreSQL** : open-source, fonctionnalités avancées, extensible
- **Oracle Database** : solution d'entreprise robuste, fonctionnalités complètes
- **Microsoft SQL Server** : bien intégré à l'écosystème Microsoft
- **SQLite** : base de données légère, intégrée à l'application, sans serveur

#### Exemple simplifié

#### 1. Relationnel


#### Table Clients:
```markdown
| ID_Client | Nom     | Prénom  | Email               | Téléphone      |
|-----------|---------|---------|---------------------|----------------|
| 1         | Dupont  | Marie   | marie.d@example.com | +33612345678   |
| 2         | Martin  | Jean    | jean.m@example.com  | +33698765432   |
| 3         | Durand  | Sophie  | sophie.d@example.com| +33655555555   |
```
#### Table Vendeurs:
```
| ID_Vendeur | Nom     | Prénom  | Email               | Téléphone      |
|------------|---------|---------|---------------------|----------------|
| 1          | Lefevre | Pierre  | pierre.l@example.com| +33611111111   |
| 2          | Moreau  | Paul    | paul.m@example.com  | +33622222222   |
| 3          | Cheval  | Claire  | claire.c@example.com| +33633333333   |
```

#### Table Commandes:
```
| ID_Commande | ID_Client | ID_Vendeur | Date       | Montant | Statut      |
|-------------|-----------|------------|------------|---------|-------------|
| 101         | 1         | 1          | 2023-09-15 | 125,50  | Livré       |
| 102         | 2         | 2          | 2023-09-20 | 78,90   | En cours    |
| 103         | 1         | 1          | 2023-10-01 | 45,00   | En attente  |
| 104         | 3         | 3          | 2023-10-05 | 200,00  | Livré       |
| 105         | 2         | 2          | 2023-10-10 | 99,99   | En cours    |
| 106         | 3         | 1          | 2023-10-15 | 50,00   | En attente  |
```

#### 2. Non-Relationel (Excel)

| ID_Commande | ID_Client | Nom_Client | Prénom_Client | Email_Client         | Téléphone_Client | ID_Vendeur | Nom_Vendeur | Prénom_Vendeur | Email_Vendeur         | Téléphone_Vendeur | Date       | Montant | Statut      |
|-------------|-----------|------------|---------------|----------------------|------------------|------------|-------------|----------------|-----------------------|--------------------|------------|---------|-------------|
| 101         | 1         | Dupont     | Marie         | marie.d@example.com  | +33612345678     | 1          | Lefevre     | Pierre         | pierre.l@example.com  | +33611111111        | 2023-09-15 | 125,50  | Livré       |
| 102         | 2         | Martin     | Jean          | jean.m@example.com   | +33698765432     | 2          | Moreau      | Paul           | paul.m@example.com    | +33622222222        | 2023-09-20 | 78,90   | En cours    |
| 103         | 1         | Dupont     | Marie         | marie.d@example.com  | +33612345678     | 1          | Lefevre     | Pierre         | pierre.l@example.com  | +33611111111        | 2023-10-01 | 45,00   | En attente  |
| 104         | 3         | Durand     | Sophie        | sophie.d@example.com | +33655555555     | 3          | Cheval      | Claire         | claire.c@example.com  | +33633333333        | 2023-10-05 | 200,00  | Livré       |
| 105         | 2         | Martin     | Jean          | jean.m@example.com   | +33698765432     | 2          | Moreau      | Paul           | paul.m@example.com    | +33622222222        | 2023-10-10 | 99,99   | En cours    |
| 106         | 3         | Durand     | Sophie        | sophie.d@example.com | +33655555555     | 1          | Lefevre     | Pierre         | pierre.l@example.com  | +33611111111        | 2023-10-15 | 50,00   | En attente  |

### b. Bases de Données Hiérarchiques (Hierarchical Databases)

#### Structure et principes
- **Organisation arborescente** des données (structure parent-enfant)
- Chaque enregistrement (nœud) a un seul parent, mais peut avoir plusieurs enfants
- Les données sont accédées en suivant un chemin hiérarchique depuis la racine

#### Caractéristiques principales
- **Simplicité conceptuelle** facile à comprendre
- **Accès rapide** aux données lorsque la structure hiérarchique est connue
- **Relations un-à-plusieurs** bien gérées
- **Navigation efficace** de parent à enfant

#### Exemples de SGBD hiérarchiques
- **IBM Information Management System (IMS)** : utilisé dans les mainframes
- **Windows Registry** : stockage hiérarchique de la configuration Windows
- **Systèmes de fichiers** : organisation des fichiers et dossiers

#### Cas d'utilisation typiques
- Systèmes bancaires traditionnels
- Systèmes de gestion de documents
- Applications nécessitant une navigation verticale rapide

#### Exemple simplifié
```
Organisation (Racine)
├── Département Marketing
│   ├── Équipe Médias Sociaux
│   │   ├── Employé: Alice
│   │   └── Employé: Bob
│   └── Équipe Publicité
│       └── Employé: Charles
└── Département Technique
    ├── Équipe Développement
    │   ├── Employé: David
    │   └── Employé: Emma
    └── Équipe Infrastructure
        └── Employé: Frank
```

### c. Bases de Données Réseau (Network Databases)

#### Structure et principes
- Extension du modèle hiérarchique permettant des relations plusieurs-à-plusieurs
- Les enregistrements sont organisés en "ensembles" où un enregistrement propriétaire peut être lié à plusieurs enregistrements membres
- Un enregistrement membre peut appartenir à plusieurs ensembles (contrairement au modèle hiérarchique)

#### Caractéristiques principales
- **Flexibilité** dans la représentation des relations complexes
- **Navigation bidirectionnelle** entre les enregistrements liés
- **Performances optimisées** pour les chemins d'accès prédéfinis

#### Exemples de SGBD réseau
- **Integrated Data Store (IDS)** développé par Charles Bachman
- **IDMS** (Integrated Database Management System)
- **RDM Embedded** (pour systèmes embarqués)

#### Cas d'utilisation typiques
- Applications industrielles complexes
- Systèmes de fabrication
- Certains systèmes bancaires hérités

#### Exemple simplifié
```
Étudiants                     Cours
  │                              │
Alice ── [inscription] ──> Mathématiques
  │                              │
  │── [inscription] ──> Sciences
  │
Bob ── [inscription] ──> Histoire
  │
Charlie ── [inscription] ──> Mathématiques
  │                              │
  │── [inscription] ──> Histoire

```

### d. Bases de Données Orientées Objet (Object-Oriented Databases)

#### Structure et principes
- **Données stockées sous forme d'objets**, comme dans la programmation orientée objet
- Les objets encapsulent à la fois les données (attributs) et les comportements (méthodes)
- **Héritage** et **polymorphisme** sont supportés
- **Identité d'objet** unique pour chaque instance

#### Caractéristiques principales
- **Correspondance directe** avec les langages orientés objet
- **Types de données complexes** facilement représentables
- **Encapsulation** des données et comportements

#### Exemples de SGBD orientés objet
- **db4o** (database for objects)
- **ObjectDB**
- **Versant Object Database**
- **ObjectStore**

#### Cas d'utilisation typiques
- Applications de CAO (Conception Assistée par Ordinateur)
- Systèmes multimédias
- Applications scientifiques avec structures de données complexes

#### Exemple simplifié
```python
# Définition d'une classe (objet)
class Personne:
    def __init__(self, nom: str, prenom: str, date_naissance: date, contacts: List[str] = None):
        self.nom = nom
        self.prenom = prenom
        self.date_naissance = date_naissance
        self.contacts = contacts if contacts is not None else []

    def calculer_age(self) -> int:
        # Méthode pour calculer l'âge
        today = date.today()
        age = today.year - self.date_naissance.year
        if (today.month, today.day) < (self.date_naissance.month, self.date_naissance.day):
            age -= 1
        return age

# Exemple d'utilisation
pers1 = Personne("Dupont", "Marie", date(1985, 5, 15))
```

### e. Bases de Données NoSQL (NoSQL Databases)

#### Structure et principes
- **"Not Only SQL"** : alternatives aux bases de données relationnelles
- **Schéma flexible** ou sans schéma prédéfini
- Conçues pour la scalabilité horizontale (distribution sur plusieurs serveurs)
- Optimisées pour des cas d'utilisation spécifiques

#### Types principaux de bases de données NoSQL

1. **Bases de données orientées documents (Document-Oriented Databases)**
   
   - Stockent des documents (généralement JSON ou BSON)
   - Chaque document contient toutes ses données associées
   - **Exemples** : MongoDB, CouchDB, Firestore
   - **Cas d'utilisation** : applications web, catalogues, gestion de contenu
   
   Exemple (document JSON):
   ```json
   [
     {
       "id": "client-123",
       "nom": "Dupont",
       "prenom": "Marie",
       "email": "marie.d@example.com",
       "adresses": [
         {
           "type": "livraison",
           "rue": "123 rue de Paris",
           "ville": "Lyon"
         },
         {
           "type": "facturation",
           "rue": "45 avenue Victor Hugo",
           "ville": "Paris"
         }
       ]
     },
     {
       "id": "client-124",
       "nom": "Martin",
       "prenom": "Jean",
       "email": "jean.m@example.com",
       "adresse": {
         "type": "principale",
         "rue": "78 boulevard Saint-Germain",
         "ville": "Marseille"
       }
     }
   ]
   ```
   
2. **Bases de données clé-valeur (Key-Value Databases)**
   - Stockent des paires clé-valeur simples
   - Très rapides pour les opérations de lecture/écriture
   - **Exemples** : Redis, DynamoDB, Riak
   - **Cas d'utilisation** : caching, sessions utilisateur, préférences

   Exemple:
   ```
   client-123:nom = "Dupont"
   client-123:prenom = "Marie"
   client-123:email = "marie.d@example.com"
   client-123:adresse:livraison:rue = "123 rue de Paris"
   client-123:adresse:livraison:ville = "Lyon"
   client-123:adresse:facturation:rue = "45 avenue Victor Hugo"
   client-123:adresse:facturation:ville = "Paris"
   
   or 
   
   client-123 = {
     "nom": "Dupont",
     "prenom": "Marie",
     "email": "marie.d@example.com",
     "adresses": [
       {
         "type": "livraison",
         "rue": "123 rue de Paris",
         "ville": "Lyon"
       },
       {
         "type": "facturation",
         "rue": "45 avenue Victor Hugo",
         "ville": "Paris"
       }
     ]
   }
   ```
   
3. **Bases de données en colonnes (Column-Family Databases)**
   
   - Stockent les données par colonnes plutôt que par lignes
   - Optimisées pour les requêtes analytiques sur de grands volumes
   - **Exemples** : Cassandra, HBase, Google Bigtable
   - **Cas d'utilisation** : analyses big data, séries temporelles, télémétrie
   
4. **Bases de données orientées graphes (Graph Databases)**
   - Stockent des entités (nœuds) et leurs relations (arêtes)
   - Optimisées pour explorer les relations entre données
   - **Exemples** : Neo4j, JanusGraph, Amazon Neptune
   - **Cas d'utilisation** : réseaux sociaux, systèmes de recommandation, détection de fraude

   Exemple simplifié:
   ```
   (Personne:Marie)-[:AMI_DE]->(Personne:Jean)
   ```

#### Caractéristiques communes des bases NoSQL
- **Scalabilité horizontale** : capacité à distribuer les données sur plusieurs serveurs
- **Haute disponibilité** : souvent conçues pour éviter les points de défaillance uniques
- **Performances élevées** pour certains types d'opérations spécifiques
- **Cohérence éventuelle** plutôt que transactions ACID strictes (dans de nombreux cas)
