# Projet NOSQL MSBGD 2020-2021
## Contributeurs Bertrand Coureaud - Dominique Jeancler - Thomas Trivellato - Roberto Nobrega

1.Objectif

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

2. Choix de l’architecture

**Stockage données brutes** : Pour répondre aux contraintes, on stocke l’ensemble des zips de l’année 2020 dans un bucket S3. Pour un an cela représente XXGo de données.
La récupération et le stockage a été obtenu via le notebook lienXX.

![test.png](https://github.com/PhileasFrog/Projet_GDELT/blob/main/test.PNG)

**Base de données NoSQL** : Ce projet nous place dans une problématique gestion et utilisation de base de données volumineuse peu structurée. Nous avons considèré que l’objectif était de proposer une architecture construire pour fournir une base de donnée NoSQL permettant au client d’effectuer rapidement de requêtes prédéfinies avec une faible latence. Nous avons choisi Cassandra comme gestion de base de données car outre sa fiabilité et son utilisation courant il présente les avantages suivants :
*	Facile d’installation
*	Résilient à la panne
*	Passage à l'échelle avec bonne performance 
*	Présence d'un langague de requêtage et d'un connecteur avec spark pour les requêtes.

![test.png](https://github.com/PhileasFrog/Projet_GDELT/blob/main/test.PNG)


 
