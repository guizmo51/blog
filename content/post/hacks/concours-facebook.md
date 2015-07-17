+++
date = "2015-04-30T13:55:17+02:00"
draft = false
title = "concours facebook"
img = "images/5546724718_7788ef2d42_b.jpg"
category =  "hacks"
categories = ["hacks"]
+++

La page facebook d'une radio organisait un concours, le principe est simple : être le premier à commenter une publication spécifique entre 13h et 17h. Le 1er utilisateur à avoir répondu gagne le lot du concours. Ce dernier s'étalait sur une semaine entière.

Pourquoi avoir fait ce mini hack ? Tout simplement pour prouver que sur ce genre de concours il est idiot de proposer un système de participation de ce type sachant que des solutions commerciales permettant de tricher existent. je me suis donc lancé dans la réalisation d'un outil avec pour objectif de remporter ce concours.


Dès le début le but est d'automatiser tout le système afin de ne pas avoir à rafraichir sa page toutes les 10 secondes et faire l'action de commenter. Pendant ce temps je peux vacquer sereinnement à mes occupations quotidiennes. L'autre raison de l'automatisation est que dans ce genre de concours en 10 secondes tu peux déjà être 15eme. 

Avant de débuter il faut découper le problème en deux parties afin de travailler efficacement sur ces deux parties indépendantes :
+ détection du nouveau contenu
+ soumission du commentaire.

A moins d'utiliser une base de mots clés ou un dictionnaire il est impossible d'identifier à l'avance le post à commenter. J'ai donc décidé de commenter automatiquement tous les nouveaux postes publiés dans le créneau défini (13h - 17h): une unique fois lors de la détection.

Tout est lié à Facebook : je vais donc utiliser leur SDK et leur API qui bien qu'elle soit mal documentée restera plus efficace sans devoir ré-inventer la roue.

Afin de pouvoir utiliser ce SDK I il faut au préalable avoir un compte ''déveloper" puis déclarer son application (nom, urls autorisées à appeler l'API , droits nécessaires..). Dans un premier temps l'application n'est visible que par vous, afin de la rendre publique il faut la soumettre aux reviewers de Facebook. Dans mon cas il n'était pas certain que l'application soit autorisée de plus il fallait un temps certain que je n'avais pas.

## Détection de nouveau contenu
Le principe de la détection de nouveau contenu est assez simple : récupérer le flux des posts, puis voir si le plus récent a changé. L'API facebook retourne cela sous forme d'un tableau d'objet qui est déjà trié. Il suffit de stocker cet identifiant du contenu le plus récent en base de données ou fichier texte. Ensuite toutes les X secondes, dans mon cas paramètré à 4 secondes je requêtais l'API Facebook grâce au SDK pour avoir les contenus de la page de la radio. J'extrayais l'identifiant du dernier post et le comparais avec celui enregistré dans mon fichier texte. Si celui-ci avait changé alors bingo je dois déclencher la publication du commentaire.

## Publication du commentaire:
Pendant plusieurs heures j'ai essayé d'utiliser le SDK Facebook afin de publier le commentaire et tout garder en PHP dans un objectif d'économie de temps.
 Il s'est avéré que cela était très compliqué probablement causé par la non-approbation de l'application. En effet hormis pour le compte developper et certains droits chaque application doit être approuvée ce qui prend du temps.

 J'ai donc abandonné la solution PHP pour publier le commentaire à travers l'API Facebook. 
 Dans un second temps comme la détection fonctionnait très bien l'application affichait directement la publication il n'y avait plus qu'à commenter. J'estimais toutefois que cela fut une perte de temps.

Enfin en désespoir de cause j'ai observé la requête Ajax jouée dans le navigateur, n'y croyant pas j'ai fait rejouer cette requête. Quelle ne fut pas ma surprise quand j'ai vu que ce replay avait permis de poster un nouveau commentaire.  Cette requête Ajax prenait en paramètre l'id de la la publication à commenter, le commentaire, la date et un autre paramètre.

J'ai donc décidé d'utiliser cette requête ajax pour publier le commentaire.  

Voici ce que j'ai décidé (qui va être l'architecture finale) : 
Une extension chrome créée pour l'occasion va appeller le script PHP de détection de nouveaux contenus. Si ce script retourne qu'il a détecté un nouveau contenu alors l'extension chrome va déclencher la requête AJAX. 

Je parle ici pour la premiere fois d'extension Chrome, c'est quelque chose de très simple à utiliser qui permet d'injecter du Javascript sur une page. 
En terme de fichiers il s'agit d'un dossier avec un manifest et des scripts. 

En quelques lignes de JavaScript j'ai implémenté et mis en oeuvre la détection de nouveau contenu et la publication du commentaire.

## Résultats
Ce projet n'a été opérationnel qu'à partir du jeudi, en effet les jours précèdents je me suis heurté à des soucis propres à la page Facebook : le contenu du jeu était créé chaque matin puis dépublié, le script n'avait alors pas détecté la publication. 
Le jeudi quelqu'un avait été plus rapide que moi, d'où l'importance des technologies 

