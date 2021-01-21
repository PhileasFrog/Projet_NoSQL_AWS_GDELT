# Projet NOSQL MSBGD 2020-2021
## Contributeurs Bertrand Coureaud - Dominique Jeancler - Thomas Trivellato - Roberto Nobrega

## 1.Objectif

Proposer un système de stockage distribué, résilient et performant sur AWS permettant de requêter sur la base de données GDELT les recherches suivantes : 
a.	afficher le nombre d’articles/évènements qui parlent de COVID qu’il y a eu pour chaque triplet (jour, pays de l’évènement, langue de l’article)
b.	pour un pays donné en paramètre, affichez les évènements qui y ont eu place triées par le nombre de mentions (tri décroissant); permettez une agrégation par jour/mois/année
c.	pour une source de donnés passée en paramètre (gkg.SourceCommonName) affichez les thèmes, personnes, lieux dont les articles de cette sources parlent ainsi que le le nombre d’articles et le ton moyen des articles (pour chaque thème/personne/lieu); permettez une agrégation par jour/mois/année.
d.	est-ce qu’on observe des patterns dans l’evolution qui pourraient nous permettre d’identifier la prochaine vague/pandemie

Contraintes :
*	Utiliser au moins 1 technologie vue en cours en expliquant les raisons de votre choix (SQL/Cassandra/MongoDB/Spark/Neo4j).
*	Le système doit pouvoir continuer après la panne sur un nœud
*	Pre-charger une année de données dans votre cluster
*	Utiliser AWS pour déployer le cluster.

## 2. Choix de l’architecture

**Stockage données brutes** : Pour répondre aux contraintes, on stocke l’ensemble des zips de l’année 2020 dans un bucket S3. Pour un an cela représente 500Go de données.
La récupération et le stockage a été obtenu via le notebook lienXX.

![test.png](https://github.com/PhileasFrog/Projet_GDELT/blob/main/bucketbc.png)

**Base de données NoSQL** : Ce projet nous place dans une problématique gestion et utilisation de base de données volumineuse peu structurée. Nous avons considèré que l’objectif était donc de proposer une architecture fournissant une base de donnée NoSQL permettant au client d’effectuer rapidement de requêtes prédéfinies avec une faible latence. Nous avons choisi Cassandra avec 3 noeuds (pour résister à la panne d'un noeud) car outre son utilisation courante et sa fiabilité présente les avantages suivants :
*	Facile d’installation
*	Résilient à la panne
*	Passage à l'échelle avec bonne performance
* Typage fort
*	Présence d'un langague de requêtage et d'un connecteur avec spark pour les requêtes.

*Spécification* : EC2 3 instances "t2.micro"

![test.png](https://github.com/PhileasFrog/Projet_GDELT/blob/main/test.PNG)

**ETL** : Pour effectuer l'étape ETL (extraction analyser et chargement) sur les données bruts sotckées sur S3 et avant chargement sous forme de table persistantes sur notre cluster Cassandra, nous avons utilisé un cluster EMR avec le framework Spark et Zeppelin préinstallé. Spark permet d'accélérer les traitement par utilisation de la RAM et d'enchainer différentes phase de traitement à l'inverse de MapReduce. Nous sommes passés par EMR pour éviter d'avoir à réinstaller nous même Spark et Zeppelin.

*Spécification* : EMR "m4.large" Spark 2.3.2 Zeppelin 0.8.0

IMAGE DE L'ARCHITECTURE

## 3. Création des tables pour répondre aux requêtes

* Préambule compte tenu de l'énoncé nous avons considérer que le filtre COVID était appliqué sur l'ensemble des requêtes. Par ailleurs ne trouvant que peu de thème avec la mention "COVID", nous avons pris la décision d'ajouter le terme "CORONAVIRUS" dans le filtre. Le notebook Zeppelin pour accéder à la phase ETL et requête est accessible sur ce lien XXX.

### 3.1 Requête 1

### 3.2 Requête 1

### 3.3 Requête 1

## 4. Peformances

## 5. Conclusions voies d'améliorations




 
