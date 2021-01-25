# Projet NOSQL MSBGD 2020-2021
## Contributeurs Bertrand Coureaud - Dominique Jeancler - Roberto Nobrega - Thomas Trivellato 

## 1.Cahier des charges:

Le descriptif complet du sujet du projet est consultable sur http://andreiarion.github.io/projet2021.html

**Objectif** : 

Proposer un système de stockage distribué, résilient et performant sur AWS permettant de requêter sur la base de données GDELT les recherches suivantes : 

a.	afficher le nombre d’articles/évènements qui parlent de COVID qu’il y a eu pour chaque triplet (jour, pays de l’évènement, langue de l’article)

b.	pour un pays donné en paramètre, affichez les évènements qui y ont eu place triées par le nombre de mentions (tri décroissant); permettez une agrégation par jour/mois/année

c.	pour une source de donnés passée en paramètre (gkg.SourceCommonName) affichez les thèmes, personnes, lieux dont les articles de cette sources parlent ainsi que le le nombre d’articles et le ton moyen des articles (pour chaque thème/personne/lieu); permettez une agrégation par jour/mois/année.

d.	est-ce qu’on observe des patterns dans l’evolution qui pourraient nous permettre d’identifier la prochaine vague/pandemie

**Contraintes** :
*	Utiliser au moins 1 technologie vue en cours en expliquant les raisons de votre choix (SQL/Cassandra/MongoDB/Spark/Neo4j).
*	Le système doit pouvoir continuer après la panne sur un nœud
*	Pre-charger une année de données dans votre cluster
*	Utiliser AWS pour déployer le cluster.

**Livrables** :

Vous devrez fournir:
* une archive avec votre code source (ou un lien sur github…)
> https://github.com/PhileasFrog/Projet_GDELT/tree/main/Zeppelin_notebooks (deux notebooks : un pour la récupération des data, un pour ETL et query)
* une courte présentation de votre architecture, modélisation, les avantages et inconvénients, des choix de modélisation et d’architecture, volumétrie, limites et contraintes (max 10 slides de présentation)
> https://github.com/PhileasFrog/Projet_GDELT/tree/main/Presentation

## 2. Choix de l’architecture et justification

**Descriptif **:

Pour répondre aux contraintes, on propose une solution entièrement hébergée sur AWS. Stockage sur S3, traitement via un cluster Spark sur EMR et persistance de la sortie du traitement sur une base de donnée Cassandra installée sur des instances EC2. 

![schemaarchi](https://github.com/PhileasFrog/Projet_GDELT/blob/main/Screenshot/architecture.PNG)

Comme illustré le process se découpe en 3 étapes :

* un premier traitement Spark via notre cluster EMR qui récupère les fichiers Zip de GDELT et les stocke dans nos bucket S3 soit environ 500 Go de donnée pour un an
![s3snap](https://github.com/PhileasFrog/Projet_GDELT/blob/main/Screenshot/bucketbc.png)
* une phase ETL où le cluster Sark va lire les zips de S3, récupérer uniquement les informations d'intérêt pour les requêtes sous forme de dataframe et enfin écrire sur nos instances Cassandra EC2.
* une phase de lecture, où depuis le cluster Spark, le client peut interoger et visualiser les résultats de ses requêtes
![requetevisu](https://github.com/PhileasFrog/Projet_GDELT/blob/main/Screenshot/sqlsh.png)

**Justifications** :

Pour exécuter du traitement massif en parallèle, on a besoin d’un outil type Spark. On a donc choisi par un cluster EMR avec Spark et l’interface Zeppelin préinstallée. Zeppelin a l’avantage notamment d’offrir des outils intégrés de visualisation très pratiques. Un inconvénient rencontré lors du projet est la nécessité de reconfigurer notre interpreteur Zeppelin à chaque démarrage de cluster (installation de fichier jar et adresssage des instances Cassandra). Nous n'avons pas trouvé de solution pour sauvegarder ces paramètres contrairement à certains camarades.

Concernant, la gestion base de données, l'équipe a choisi Cassandra car c’est un outil très répandu et fiable et qui offre une bonne performance pour le passage à l’échelle. Par ailleurs parmi les outils noSQL vus en cours, l'équipe a pensé que cette solution s’adaptait à la problèmatique compte tenu de la nature de la donnée à écrire (structuré et typé via les dataframes) et d'un point de vue confort client Cassandra ayant une très bonne performance en lecture et un langage de requête familier. En utilisant des machines sur EC2 nous pouvons garder notre installation et base de donnes à moindre coût. Pour finir nous avons choisi à minima une installation avec 3 instances pour tolérer la panne d’un nœud comme exigé dnas le cahier des charges.

## 3. Création des tables pour répondre aux requêtes

Le détails des requêtes et résultats sont consultables dans la présentation ppt disponible dans https://github.com/PhileasFrog/Projet_GDELT/tree/main/Presentation.

Préambule compte tenu de l'énoncé nous avons considéré que le filtre COVID était appliqué sur l'ensemble des requêtes. Pour appliquer ce filtre nous nous sommes basés sur la colonne « Theme » des éléments gkg de GDELT. Ne trouvant que peu de thème avec la mention "COVID", nous avons pris la décision d'ajouter le terme "CORONAVIRUS" dans le filtre.

Le schéma illustre ci-dessous le processus menant à la création des dataframes qui nous permettent en final de créer des tables Cassandra spécifiques aux requêtes prédéfinies dans l’énoncé. Le filtre COVID étant commun, on retrouve ainsi le rôle du dataframe « gkg_all » qui va servir de filtre via jointure.

![schemadonnee](https://github.com/PhileasFrog/Projet_GDELT/blob/main/Screenshot/Schemadonnee.jpg)

### 3.1 Requête 1

Après avoir filtré sur la colonne « Theme » de « gkg » nous sélectionnons uniquement la colonne 4 (mentionid) qui correspond à l’identifiant unique du document considéré comme clef étrangère du jeu données « mention ». Nous sélectionnons dans « mention » l’information langue et l’identifiant de l’évènement pour faire la jointure avec les données « event » pour récupérer cette fois les informations de date et pays. Après opération d’agrégation nous obtenons un dataframe qu’on va définir comme table sur Cassandra.

### 3.2 Requête 2

Pour cette requête nous repartons du même dataframe post jointure entre event et mention de la query 1 mais ajoutons 3 colonnes correspondant au jour mois année pour faire les opérations d’agrégation demandées

### 3.3 Requête 3

Pour cette dernière requête nous nous concentrons maintenant uniquement sur les données gkg. Nous créons trois tables différentes pour faire le calcul du ton moyen en fonction du thème, de la personne ou du lieu.




 
