# Projet NOSQL MSBGD 2020-2021
## Contributeurs Bertrand Coureaud - Dominique Jeancler - Roberto Nobrega - Thomas Trivellato 

## 1.Cahier des charges:

Le descriptif complet du projet est consultable sur http://andreiarion.github.io/projet2021.html

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
* une courte présentation de votre architecture, modélisation, les avantages et inconvénients, des choix de modélisation et d’architecture, volumétrie, limites et contraintes (max 10 slides de présentation)

## 2. Choix de l’architecture et justification

*Descriptif :*

Pour répondre aux contraintes, on propose une solution entièrement hébergée sur AWS. Stockage sur S3, traitement via un cluster Spark sur EMR et persistance de la sortie du traitement sur une base de donnée Cassandra installée sur des instances EC2. 

![test.png](https://github.com/PhileasFrog/Projet_GDELT/blob/main/bucketbc.png)

Comme illustré le process se découpe en 3 étapes :

* un premier traitement Spark via notre cluster EMR qui récupère les fichiers Zip de GDELT et les stocke dans nos bucket S3 soit environ 500 Go de donnée pour un an
![test.png](https://github.com/PhileasFrog/Projet_GDELT/blob/main/bucketbc.png)
* une phase ETL où le cluster Sark va lire les zips de S3, récupérer uniquement les informations d'intérêt pour les requêtes sous forme de dataframe et enfin écrire sur nos instances Cassandra EC2.
* une phase de lecture, où depuis le cluster Spark, le client peut interoger et visualiser les résultats de ses requêtes

La source de la phase de récupération de données est disponible ici tandis que la phase ETL et requête et intégré dans le même notebook ici.

*Justifications* :

Pour exécuter du traitement massif en parallèle, on a besoin d’un outil type Spark. On a donc choisi par un cluster EMR avec Spark et l’interface Zeppelin préinstallée. Zeppelin a l’avantage notamment d’offrir des outils intégrés de visualisation très pratiques. Un inconvénient rencontré lors du projet est la nécessité de reconfigurer notre interpreteur Zeppelin à chaque démarrage de cluster (installation de fichier jar et adresssage des instances Cassandra). Nous n'avons pas trouvé de solution pour sauvegarder ces paramètres contrairement à certains camarades.

Concernant, la gestion base de données, l'équipe a choisi Cassandra car c’est un outil très répandu et fiable et qui offre une bonne performance pour le passage à l’échelle. Par ailleurs parmi les outils noSQL vus en cours, l'équipe a pensé que cette solution s’adaptait à la problèmatique compte tenu de la nature de la donnée à écrire (structuré et typé via les dataframes) et d'un point de vue confort client Cassandra ayant une très bonne performance en lecture et un langage de requête familier. En utilisant des machines sur EC2 nous pouvons garder notre installation et base de donnes à moindre coût. Pour finir nous avons choisi à minima une installation avec 3 instances pour tolérer la panne d’un nœud comme exigé dnas le cahier des charges.

## 3. Création des tables pour répondre aux requêtes

Préambule compte tenu de l'énoncé nous avons considérer que le filtre COVID était appliqué sur l'ensemble des requêtes. Par ailleurs ne trouvant que peu de thème avec la mention "COVID", nous avons pris la décision d'ajouter le terme "CORONAVIRUS" dans le filtre. Ayant ce filtre commun nous avons donc effectuer une première étape  

### 3.1 Requête 1

### 3.2 Requête 2

### 3.3 Requête 3



 
