Voici une version modifiée du guide SQLite avec Python, incluant des explications supplémentaires sur les concepts propres à la bibliothèque `sqlite3`, notamment `cursor`, `fetchone`, `fetchmany`, `fetchall`, `commit`, `execute`, et `executemany`.

---

# SQLite avec Python : Guide Pratique

Ce guide se concentre sur les commandes Python pour manipuler une base de données SQLite existante (`pokedex.sqlite`) en utilisant les opérations CRUD (Create, Read, Update, Delete).

## Table des matières

1. Connexion à une base de données existante
2. Opérations CRUD avec `sqlite3`
    - Création (Create)
    - Lecture (Read)
    - Mise à jour (Update)
    - Suppression (Delete)
3. Utilisation avec Pandas
4. Exemples pratiques avec la base de données Pokémon

## Connexion à une base de données existante

Pour commencer à travailler avec une base de données SQLite existante, utilisez le module `sqlite3` de Python :

```python
import sqlite3

# Connexion à une base de données existante
conn = sqlite3.connect('pokedex.sqlite')

# Création d'un curseur pour exécuter des commandes SQL
curseur = conn.cursor()

# Vérifier que la connexion fonctionne
curseur.execute("SELECT sqlite_version();")
version = curseur.fetchone()
print(f"Version SQLite: {version[0]}")
```

### Explications :
- **`sqlite3.connect('pokedex.sqlite')`** : Établit une connexion à la base de données SQLite nommée `pokedex.sqlite`.
- **`conn.cursor()`** : Crée un objet curseur qui permet d'exécuter des commandes SQL.
- **`curseur.execute("SELECT sqlite_version();")`** : Exécute une commande SQL pour obtenir la version de SQLite.
- **`curseur.fetchone()`** : Récupère le premier résultat de la requête exécutée.

## Opérations CRUD avec `sqlite3`

### Création (Create)

#### Créer une nouvelle table

```python
# Création d'une nouvelle table
curseur.execute('''
CREATE TABLE IF NOT EXISTS pokemon_types (
    id INTEGER PRIMARY KEY,
    pokemon_id INTEGER,
    type_name TEXT,
    FOREIGN KEY (pokemon_id) REFERENCES pokedex(id)
)
''')

# Enregistrer les modifications
conn.commit()
```

#### Insérer des données

```python
# Insertion d'un seul enregistrement
curseur.execute('''
INSERT INTO pokemon_types (pokemon_id, type_name) VALUES (?, ?)
''', (1, 'grass'))

# Insertion de plusieurs enregistrements
types_data = [
    (1, 'poison'),
    (2, 'grass'),
    (2, 'poison'),
    (3, 'grass'),
    (3, 'poison'),
    (4, 'fire')
]
curseur.executemany('INSERT INTO pokemon_types (pokemon_id, type_name) VALUES (?, ?)', types_data)

# Enregistrer les modifications
conn.commit()
```

### Explications :
- **`curseur.execute()`** : Exécute une commande SQL unique.
- **`curseur.executemany()`** : Exécute une commande SQL pour plusieurs enregistrements.
- **`conn.commit()`** : Enregistre les modifications effectuées dans la base de données.

### Lecture (Read)

#### Exécuter des requêtes simples

```python
# Sélection de tous les enregistrements d'une table
curseur.execute('SELECT * FROM pokedex LIMIT 5')
pokemons = curseur.fetchall()

# Affichage des résultats
for pokemon in pokemons:
    print(pokemon)
```

#### Différentes méthodes de récupération

```python
curseur.execute('SELECT name, hp, attack FROM pokedex')
premier_pokemon = curseur.fetchone()  # Récupère uniquement le premier résultat
quelques_pokemons = curseur.fetchmany(3)  # Récupère les 3 résultats suivants
tous_les_pokemons = curseur.fetchall()  # Récupère tous les résultats restants
```

### Explications :
- **`curseur.fetchone()`** : Récupère le premier résultat de la requête.
- **`curseur.fetchmany(n)`** : Récupère les `n` résultats suivants.
- **`curseur.fetchall()`** : Récupère tous les résultats restants.

#### Requêtes avec filtres

```python
# Sélection avec condition
curseur.execute('SELECT name, hp FROM pokedex WHERE type LIKE ?', ('%fire%',))
fire_pokemons = curseur.fetchall()
for pokemon in fire_pokemons:
    print(f"Pokémon de feu: {pokemon[0]} - HP: {pokemon[1]}")

# Recherche avec plusieurs conditions
curseur.execute('''
SELECT name, attack, defense
FROM pokedex
WHERE hp > ? AND defense > ?
''', (50, 60))
tough_pokemons = curseur.fetchall()
```

#### Accéder aux colonnes par nom

```python
# Configurer la connexion pour retourner des dictionnaires
conn = sqlite3.connect('pokedex.sqlite')
conn.row_factory = sqlite3.Row

curseur = conn.cursor()
curseur.execute('SELECT id, name, hp, attack FROM pokedex LIMIT 3')

for row in curseur.fetchall():
    print(f"ID: {row['id']}, Nom: {row['name']}, HP: {row['hp']}")
```

### Mise à jour (Update)

```python
# Mise à jour d'un seul enregistrement
curseur.execute('''
UPDATE pokedex
SET hp = ?, attack = ?
WHERE id = ?
''', (50, 55, 1))

# Mise à jour de plusieurs enregistrements
curseur.execute('''
UPDATE pokedex
SET s_defense = s_defense * 1.1
WHERE type LIKE ?
''', ('%grass%',))

conn.commit()
```

### Suppression (Delete)

```python
# Suppression d'un enregistrement
curseur.execute('DELETE FROM pokedex WHERE id = ?', (150,))

# Suppression avec condition
curseur.execute('DELETE FROM pokedex WHERE hp < ? AND speed < ?', (40, 40))

conn.commit()
```

### Transactions

SQLite supporte les transactions, qui permettent de grouper plusieurs opérations et de les annuler en cas d'erreur :

```python
try:
    # Les opérations suivantes seront dans une transaction
    curseur.execute("UPDATE pokedex SET hp = hp + 5 WHERE id = 1")
    curseur.execute("UPDATE pokedex SET attack = attack + 3 WHERE id = 1")

    # Si tout va bien, on valide les changements
    conn.commit()
    print("Modifications validées avec succès")

except sqlite3.Error as e:
    # En cas d'erreur, on annule tout
    conn.rollback()
    print(f"Une erreur est survenue: {e}")
```

### Fermeture de la connexion

```python
# Fermer le curseur et la connexion lorsque vous avez terminé
curseur.close()
conn.close()
```

## Utilisation avec Pandas

Pandas facilite grandement l'analyse de données issues de bases SQLite.

### Lecture de données

```python
import pandas as pd
import sqlite3

# Connexion à la base de données
conn = sqlite3.connect('pokedex.sqlite')

# Exécution d'une requête et conversion en DataFrame
df = pd.read_sql_query("SELECT * FROM pokedex", conn)

# Affichage des premières lignes
print(df.head())

# Statistiques descriptives
print(df.describe())
```

### Filtrage et analyse

```python
# Filtrage de données
fire_pokemon = df[df['type'].str.contains('fire', case=False)]
print(f"Nombre de Pokémon de type feu : {len(fire_pokemon)}")

# Statistiques par groupe
stats_by_type = df.groupby('type').agg({
    'hp': 'mean',
    'attack': 'mean',
    'defense': 'mean'
}).reset_index()

print("Statistiques moyennes par type:")
print(stats_by_type)
```

### Écriture vers la base de données

```python
# Création d'un nouveau DataFrame
new_pokemons = pd.DataFrame({
    'id': [151, 152],
    'name': ['mew', 'chikorita'],
    'hp': [100, 45],
    'attack': [100, 49],
    'defense': [100, 65],
    'type': ['{psychic}', '{grass}']
})

# Écriture dans la base de données
new_pokemons.to_sql('new_pokemon_data', conn, if_exists='replace', index=False)

# Vérification
verification = pd.read_sql_query("SELECT * FROM new_pokemon_data", conn)
print(verification)

# Fermeture de la connexion
conn.close()
```

## Exemples pratiques avec la base de données Pokémon

Voici un exemple de comment explorer et manipuler la base de données Pokémon que vous avez :

```python
import sqlite3
import pandas as pd

# Connexion à la base de données
conn = sqlite3.connect('pokedex.sqlite')
curseur = conn.cursor()

# Affichage des 5 premiers Pokémon
curseur.execute('SELECT * FROM pokedex LIMIT 5')
columns = [description[0] for description in curseur.description]
rows = curseur.fetchall()

print("Structure de la table :")
print(columns)
print("\nDonnées :")
for row in rows:
    print(row)

# Utilisation de pandas pour une analyse plus avancée
df_pokemon = pd.read_sql_query("SELECT * FROM pokedex", conn)

# Analyse statistique par type
def extract_types(type_string):
    """Extraction des types depuis la chaîne {type1,type2}"""
    if not isinstance(type_string, str):
        return []
    return type_string.strip('{}').split(',')

# Créer des colonnes pour chaque type primaire
df_pokemon['primary_type'] = df_pokemon['type'].apply(
    lambda x: extract_types(x)[0] if extract_types(x) else None
)

# Statistiques par type primaire
stats_by_primary = df_pokemon.groupby('primary_type').agg({
    'hp': 'mean',
    'attack': 'mean',
    'defense': 'mean',
    'speed': 'mean'
}).reset_index()

print("\nStatistiques moyennes par type primaire:")
print(stats_by_primary)

# Trouver les Pokémon les plus équilibrés (stats similaires)
df_pokemon['stat_std'] = df_pokemon[['hp', 'attack', 'defense', 's_attack', 's_defense', 'speed']].std(axis=1)
balanced_pokemon = df_pokemon.sort_values('stat_std').head(5)

print("\nPokémon aux statistiques les plus équilibrées:")
print(balanced_pokemon[['name', 'hp', 'attack', 'defense', 'stat_std']])

# Fermeture de la connexion
conn.close()
```

### Exemple de sortie pour la requête de base :

```
id	name	height	weight	hp	attack	defense	s_attack	s_defense	speed	type	evo_set	info
1	bulbasaur	7	69	45	49	49	65	65	45	{grass,poison}	1	A strange seed was planted on its back at birth. The plant sprouts and grows with this POKéMON.
2	ivysaur	10	130	60	62	63	80	80	60	{grass,poison}	1	When the bulb on its back grows large, it appears to lose the ability to stand on its hind legs.
3	venusaur	20	1000	80	82	83	100	100	80	{grass,poison}	1	The plant blooms when it is absorbing solar energy. It stays on the move to seek sunlight.
4	charmander	6	85	39	52	43	60	50	65	{fire}	2	Obviously prefers hot places. When it rains, steam is said to spout from the tip of its tail.
5	charmeleon	11	190	58	64	58	80	65	80	{fire}	2	When it swings its burning tail, it elevates the temperature to unbearably high levels.
```

---

Ce guide devrait maintenant être plus clair et informatif, avec des explications supplémentaires sur les concepts clés de la bibliothèque `sqlite3`.