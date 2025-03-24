[# Happy to Connect](https://cs50.harvard.edu/sql/2024/psets/6/connect/)

![Logo LinkedIn](https://cs50.harvard.edu/sql/2024/psets/6/connect/linkedin.png)

## Problème à Résoudre

Vous vous souvenez peut-être d'un [problème précédent](https://cs50.harvard.edu/sql/2024/psets/2/connect/) 
où LinkedIn est « le plus grand réseau professionnel au monde » avec pour mission de « connecter les professionnels 
du monde entier pour les rendre plus productifs et couronnés de succès ». 
Il est donc probable qu'ils utilisent des serveurs de bases de données complets pour garantir que leur plateforme est [hautement disponible](https://www.digitalocean.com/community/tutorials/what-is-high-availability) dans le monde entier.

Dans un fichier nommé `schema.sql` dans un dossier nommé `sentimental-connect`, écrivez un ensemble d'instructions SQL pour concevoir un schéma de base de données MySQL que LinkedIn pourrait utiliser.

## Spécification

Votre tâche consiste à créer une base de données MySQL pour LinkedIn à partir de zéro. Les détails de mise en œuvre dépendent de vous, bien que vous deviez au minimum vous assurer que votre base de données répond aux spécifications de la plateforme et qu'elle peut représenter les données d'exemple données. Vous êtes invité à utiliser ou à améliorer votre conception d'une base de données SQLite — gardez simplement à l'esprit que vous disposez désormais d'un nouvel ensemble de types !

### Plateforme

#### Utilisateurs (Users)

Le cœur de la plateforme LinkedIn est ses utilisateurs. Votre base de données doit pouvoir représenter les informations suivantes concernant les utilisateurs de LinkedIn :

- Leur prénom et nom de famille (first and last name)
- Leur nom d'utilisateur (username)
- Leur mot de passe (password)

Gardez à l'esprit que, si une entreprise suit les meilleures pratiques, les mots de passe des applications sont « hachés » (hashed). Pas besoin de vous inquiéter du hachage des mots de passe ici, bien qu'il puisse être utile de savoir que certains algorithmes de hachage peuvent produire des chaînes de jusqu'à 128 caractères.

#### Écoles et Universités (Schools and Universities)

LinkedIn permet également des comptes officiels d'écoles ou d'universités, comme celui de Harvard, afin que les anciens élèves (c'est-à-dire ceux qui ont fréquenté l'établissement) puissent identifier leur affiliation. Assurez-vous que la base de données de LinkedIn peut stocker les informations suivantes sur chaque école :

- Le nom de l'école (name of the school)
- Le type d'école (type of school)
- L'emplacement de l'école (school’s location)
- L'année de fondation de l'école (year in which the school was founded)

Vous devez supposer que LinkedIn ne permet aux écoles de choisir qu'un seul des trois types suivants : « Primaire » (Primary), « Secondaire » (Secondary) et « Enseignement Supérieur » (Higher Education).

#### Entreprises (Companies)

LinkedIn permet aux entreprises de créer leurs propres pages, comme celle de LinkedIn elle-même, afin que les employés puissent identifier leur emploi passé ou actuel avec l'entreprise. Assurez-vous que la base de données de LinkedIn peut stocker les informations suivantes pour chaque entreprise :

- Le nom de l'entreprise (name of the company)
- Le secteur d'activité de l'entreprise (company’s industry)
- L'emplacement de l'entreprise (company’s location)

Vous devez supposer que LinkedIn ne permet aux entreprises de choisir qu'un seul des trois secteurs suivants : « Technologie » (Technology), « Éducation » (Education) et « Commerce » (Business).

#### Connexions (Connections)

Enfin, l'essence de LinkedIn est sa capacité à faciliter les connexions entre les personnes. Assurez-vous que la base de données de LinkedIn peut prendre en charge chacune des connexions suivantes.

**Connexions avec des Personnes (Connections with People)**

La base de données de LinkedIn doit pouvoir représenter des connexions mutuelles (réciproques, bidirectionnelles) entre les utilisateurs. Pas besoin de s'inquiéter des connexions unidirectionnelles où l'utilisateur A « suit » (follows) l'utilisateur B sans que l'utilisateur B « suive » l'utilisateur A.

**Connexions avec des Écoles (Connections with Schools)**

Un utilisateur doit pouvoir créer une affiliation avec une école donnée. Et de même, cette école doit pouvoir trouver ses anciens élèves. De plus, permettez à un utilisateur de définir :

- La date de début de leur affiliation (c'est-à-dire, quand ils ont commencé à fréquenter l'école) (start date of their affiliation)
- La date de fin de leur affiliation (c'est-à-dire, quand ils ont obtenu leur diplôme), le cas échéant (end date of their affiliation, if applicable)
- Le type de diplôme obtenu/poursuivi (par exemple, « BA », « MA », « PhD », etc.) (type of degree earned/pursued)

**Connexions avec des Entreprises (Connections with Companies)**

Un utilisateur doit pouvoir créer une affiliation avec une entreprise donnée. Et de même, cette entreprise doit pouvoir trouver ses employés actuels et passés. De plus, permettez à un utilisateur de définir :

- La date de début de leur affiliation (c'est-à-dire, la date à laquelle ils ont commencé à travailler avec l'entreprise) (start date of their affiliation)
- La date de fin de leur affiliation (c'est-à-dire, quand ils ont quitté l'entreprise), le cas échéant (end date of their affiliation, if applicable)

### Données d'Exemple (Sample Data)

Votre base de données doit pouvoir représenter…

- Un utilisateur, Claudine Gay, dont le nom d'utilisateur est « claudine » et le mot de passe est « password ».
- Un utilisateur, Reid Hoffman, dont le nom d'utilisateur est « reid » et le mot de passe est « password ».
- Une école, Harvard University, qui est une université située à Cambridge, Massachusetts, fondée en 1636.
- Une entreprise, LinkedIn, qui est une entreprise technologique dont le siège est à Sunnyvale, Californie.
- La connexion de Claudine Gay avec Harvard, poursuivant un doctorat du 1er janvier 1993 au 31 décembre 1998.
- La connexion de Reid Hoffman avec LinkedIn, avec le titre « CEO and Chairman », du 1er janvier 2003 au 1er février 2007.

### Conseils (Advice)

- Considérez toute la gamme des types pris en charge par MySQL, qui sont documentés dans le manuel de référence MySQL 8.0 à l'adresse dev.mysql.com/doc/refman/8.0/en/data-types.html.
- Considérez également les conseils du manuel de référence sur le choix du bon type pour une colonne, documentés à l'adresse dev.mysql.com/doc/refman/8.0/en/choosing-types.html.
    - Parmi les conseils de haut niveau, choisissez le type le plus précis pour votre cas d'utilisation. Par exemple, si vous savez qu'une colonne d'entiers ne stockera que des valeurs positives, vous devriez envisager de modifier le type d'entier avec `UNSIGNED` (par exemple, `INT UNSIGNED` ou `TINYINT UNSIGNED`) pour obtenir la plage maximale de votre type.

## Utilisation (Usage)

Pour utiliser `mysql`, vous devez d'abord démarrer un serveur MySQL, comme suit :

```bash
docker container run --name mysql -p 3306:3306 -v /workspaces/$RepositoryName:/mnt -e MYSQL_ROOT_PASSWORD=crimson -d mysql
```

Vous pouvez ensuite vous connecter au serveur avec :

```bash
mysql -h 127.0.0.1 -P 3306 -u root -p
```

Tapez crimson comme mot de passe.

Si c'est la première fois que vous vous connectez à votre serveur `mysql`, vous devrez créer une nouvelle base de données sur le serveur. À votre invite `mysql>`, essayez ce qui suit :

```sql
CREATE DATABASE `linkedin`;
```

Vous devriez voir quelque chose comme « Query OK, 1 row affected ». Ensuite, pour vous assurer que vos futures instructions SQL sont exécutées sur votre nouvelle base de données `linkedin`, exécutez ce qui suit :

```sql
USE `linkedin`;
```

Maintenant, rappelez-vous d'autres instructions MySQL du cours qui pourraient vous aider à naviguer ! Vous pouvez toujours taper `quit` pour fermer votre connexion à la base de données MySQL.

## Comment Tester (How to Test)

Bien que `check50` existe pour ce problème, seul vous pouvez vous assurer que votre base de données répond aux spécifications de la plateforme et qu'elle peut stocker les données d'exemple de manière efficace. Considérez si votre base de données est entièrement normalisée !

### Correction (Correctness)

```bash
check50 cs50/problems/2024/sql/sentimental/connect
```

### Dépannage (Troubleshooting)

Parfois, vous pouvez rencontrer cette erreur lorsque vous essayez de vous connecter à votre base de données MySQL après un redémarrage de Codespace :

```bash
ERROR 2003 (HY000): Can't connect to MySQL server on '127.0.0.1:3306' (111)
```

Vous pouvez exécuter la commande suivante pour redémarrer le conteneur mysql :

```bash
docker container start mysql
```
