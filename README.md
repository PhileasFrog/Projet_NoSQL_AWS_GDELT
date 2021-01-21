# Projet NOSQL MSBGD 2020-2021
## Contributeurs Bertrand Coureaud - Dominique Jeancler - Thomas Trivellato - Roberto Nobrega

1.Objectif

Proposer un système de stockage distribué, résilient et performant sur AWS permettant de requêter sur la base de données GDELT les informations traitant d'information contenant le terme COVID : 
* le nombre d’articles/évènements qu’il y a eu pour chaque triplet (jour, pays de l’évènement, langue de l’article).
* pour un pays donné en paramètre, affichez les évènements qui y ont eu place triées par le nombre de mentions (tri décroissant); permettez une agrégation par jour/mois/année
* pour une source de donnés passée en paramètre (gkg.SourceCommonName) affichez les thèmes, personnes, lieux dont les articles de cette sources parlent ainsi que le le nombre d’articles et le ton moyen des articles (pour chaque thème/personne/lieu); permettez une agrégation par jour/mois/année.
* dresser la cartographie des relations entre les pays d’après le ton des articles : pour chaque paire (pays1, pays2), calculer le nombre d’article, le ton moyen (aggrégations sur Année/Mois/Jour, filtrage par pays ou carré de coordonnées)


![text](test.png) : insere une image à l’url.png
