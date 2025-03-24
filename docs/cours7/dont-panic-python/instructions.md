[# Don’t Panic!](https://cs50.harvard.edu/sql/2024/psets/6/dont-panic/python/)

![Une salle de serveur vide dans un bureau la nuit, dans le style d'un film noir de détective](https://cs50.harvard.edu/sql/2024/psets/6/dont-panic/python/dont-panic.jpg)

## Problème à Résoudre

Vous êtes un « pentester » (testeur de pénétration) formé. Après votre succès dans une opération précédente, une nouvelle entreprise vous a engagé pour effectuer un test de pénétration et rapporter les vulnérabilités dans leur système de données. Cette fois, vous suspectez pouvoir faire mieux en écrivant un programme en Python qui automatise votre piratage.

Pour réussir cette opération secrète, vous devrez :

- Vous connecter, via Python, à une base de données SQLite.
- Modifier, dans votre programme Python, le mot de passe de l'administrateur.

Si vous n'avez pas d'expérience avec Python, pas de souci ! Ce problème vous guidera à chaque étape.

## Démonstration

```SQL
$ ls                                                                                                
dont-panic.db  hack.py  reset.sql                                                                   
$ python hack.py                                                                                    
Enter a password: hacked!                                                                           
$ sqlite3 dont-panic.db                                                                             
sqlite> SELECT "password"                                                                           
   ...> FROM "users"                                                                                
   ...> WHERE "username" = 'admin';    
   
   +----------+                                                                                        
| password |                                                                                        
+----------+                                                                                        
| hacked!  |                                                                                        
+----------+                                                                                        
sqlite> .quit  
```

## Code de Distribution

Pour ce problème, vous aurez besoin d'accéder à une base de données, un fichier Python, et un ensemble d'instructions SQL pour réinitialiser la base de données au cas où votre piratage échouerait la première fois (ne vous inquiétez pas si c'est le cas !).


## Spécification

Dans `hack.py`, écrivez un programme Python pour réaliser les tâches suivantes :

- Connectez-vous, via Python, à une base de données SQLite.
- Modifiez, dans votre programme Python, le mot de passe de l'administrateur.

Lorsque votre programme dans `hack.py` est exécuté sur une nouvelle instance de la base de données, il devrait produire les résultats ci-dessus.

L'horloge tourne !

## Guide

Si vous êtes nouveau dans Python (ou dans la connexion de Python avec SQL !), ce guide vous accompagnera à chaque étape de ce problème.

### Python

Lorsque vous téléchargez le code de distribution pour ce problème, vous devriez remarquer un fichier nommé `hack.py`. Vous pouvez dire que ce programme est un programme Python car il se termine par `.py`. L'extension `.py` identifie les fichiers comme des fichiers Python, tout comme l'extension `.sql` identifie les fichiers comme un ensemble d'instructions SQL.

Au début, `hack.py` ne devrait contenir qu'une seule ligne de code Python :

```python
print("Hacked!")
```

Pour exécuter ce programme Python, assurez-vous que lorsque vous tapez `ls`, vous voyez `hack.py` parmi les fichiers de votre répertoire actuel. Ensuite, exécutez ce qui suit dans votre terminal :

```bash
python hack.py
```

Vous devriez voir « Hacked! » dans votre fenêtre de terminal. Pas vraiment un piratage, mais vous êtes sur la bonne voie !

### Connexion à une Base de Données

Maintenant que vous pouvez exécuter votre programme Python, l'étape suivante consiste à connecter votre programme à `dont-panic.db`. Pour ce faire, vous devrez utiliser la bibliothèque CS50 pour Python. Une bibliothèque est une collection de code que quelqu'un d'autre a écrit pour résoudre un problème (et, surtout, que vous pouvez utiliser dans votre propre programme !). Dans ce cas, l'une des problèmes que la bibliothèque CS50 pour Python vous aide à résoudre est le processus de connexion à une base de données SQLite.

Pour utiliser la fonctionnalité SQL de la bibliothèque CS50 dans votre propre programme, remplacez `print("Hacked!")` par ce qui suit :

```python
from cs50 import SQL
```

Cette ligne de code Python indique que votre programme doit récupérer (« importer ») des outils liés à SQL depuis la bibliothèque CS50, appelée `cs50`.

Avec cette bibliothèque maintenant incluse dans votre programme, établir une connexion à `dont-panic.db` est aussi simple qu'une seule ligne de code Python :

```python
from cs50 import SQL

db = SQL("sqlite:///dont-panic.db")
```

Vous pouvez décomposer cette ligne de code en les éléments suivants :

- `db = SQL(...)`, qui établit une connexion à la base de données donnée en entrée, entre les parenthèses. Cette ligne de code garantit également que vous pouvez faire référence à votre connexion de base de données sous le nom `db` par la suite dans votre programme.
- `sqlite:///dont-panic.db` est une URL (similaire à une URL de site web !) qui identifie la base de données à laquelle se connecter (`dont-panic.db`) et le dialecte SQL à utiliser (`sqlite`, dans ce cas, par opposition à `mysql` ou `postgres`).

Si vous vous sentez plus à l'aise, vous pouvez en apprendre davantage sur cette ligne de code dans la documentation de la bibliothèque CS50 pour Python.

Essayez d'exécuter votre programme maintenant. Vous ne verrez peut-être rien se passer, et si c'est le cas, c'est bon signe !

### Exécution d'Instructions SQL avec Python

La fonctionnalité SQL de la bibliothèque CS50 pour Python est fournie avec une méthode appelée `execute`. Une méthode reçoit une entrée et produit une sortie. Par exemple, une méthode peut prendre une instruction SQL en entrée, exécuter cette instruction SQL sur une base de données, et vous renvoyer les résultats de l'instruction SQL. En fait, c'est exactement ce que fait la méthode `execute` !

```python
from cs50 import SQL

db = SQL("sqlite:///dont-panic.db")
db.execute(
    """
    UPDATE "users"
    SET "password" = 'hacked!'
    WHERE "username" = 'admin';
    """
)
```

Remarquez que, à l'intérieur des parenthèses associées à la méthode `execute`, vous avez maintenant écrit une requête SQL entièrement formée. Lorsque vous exécutez votre programme Python, la requête SQL sera exécutée sur la base de données.

Après avoir exécuté votre programme, essayez d'ouvrir `dont-panic.db` avec `sqlite3`. Lorsque vous consultez le mot de passe de l'administrateur, vous devriez constater qu'il est maintenant « hacked! ».

Si à tout moment vous souhaitez réinitialiser `dont-panic.db` à son état d'origine, rappelez-vous que vous pouvez utiliser `reset.sql`.

### Instructions Préparées

Imaginez que vous souhaitiez qu'un utilisateur détermine le nouveau mot de passe administratif au fur et à mesure que votre programme Python s'exécute.

Rappelez-vous du cours que une instruction préparée est une requête SQL avec des espaces réservés pour des valeurs qui sont insérées (« interpolées ») plus tard. Puisque vous ne savez pas quel mot de passe votre utilisateur choisira, le mieux que vous puissiez faire est de définir un espace réservé pour le mot de passe et permettre à votre programme d'interpoler le mot de passe choisi par l'utilisateur plus tard. L'utilisation d'une instruction préparée peut donc aider !

La bibliothèque CS50 pour Python prend en charge l'utilisation d'instructions préparées. Tout d'abord, modifiez votre programme pour accepter une entrée de l'utilisateur :

```python
from cs50 import SQL

db = SQL("sqlite:///dont-panic.db")
password = input("Enter a password: ")
db.execute(
    """
    UPDATE "users"
    SET "password" = 'oops!'
    WHERE "username" = 'admin';
    """
)
```

Remarquez que si vous exécutez maintenant votre programme, vous serez invité à entrer un mot de passe. Tout ce que vous entrez est stocké dans la variable nommée `password`. Une variable est un nom pour une valeur qui peut changer.

Maintenant, modifiez votre requête SQL pour qu'elle soit une instruction préparée. Dans la bibliothèque CS50, vous pouvez utiliser un `?` pour représenter un espace réservé pour une valeur que vous fournirez plus tard.

```python
from cs50 import SQL

db = SQL("sqlite:///dont-panic.db")
password = input("Enter a password: ")
db.execute(
    """
    UPDATE "users"
    SET "password" = ?
    WHERE "username" = 'admin';
    """
)
```

La dernière étape, bien sûr, consiste à indiquer à la méthode `execute` quelle valeur elle doit substituer à l'espace réservé. Pour ce faire, vous pouvez ajouter la valeur à substituer après la requête SQL, séparée par une virgule :

```python
from cs50 import SQL

db = SQL("sqlite:///dont-panic.db")
password = input("Enter a password: ")
db.execute(
    """
    UPDATE "users"
    SET "password" = ?
    WHERE "username" = 'admin';
    """,
    password
)
```

Maintenant, essayez d'exécuter votre programme et de consulter les modifications dans `dont-panic.db` !

