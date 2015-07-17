+++
date = "2015-04-01T13:57:26+02:00"
draft = false
title = "boostadopt"
img = "images/LOGO-boostadopt.jpg"
category = "retour d'experience"
categories = ["retour-dexperience"]
+++

-AUM
-PHP avant, mobie
-Problematique 
- Back
- Front
- Chrome extension

Pendant de nombreuses années et depuis l'ouverture du site j'ai été un utilisateur du site de rencontre <a href="http://www.adopteunmec.com">AdopteUnMec.com</a> (ce n'est plus le cas maintenant ;-) ). Ce site a un concept assez particulier lui ayant permis de se démarquer : les hommes possèdent un nombre de charmes limité (un charme est semblable à un poke), la personne ayant reçu ce charme est notifiée de ce charme. Ensuite elle peut autoriser, si le profil émetteur du charme lui plait, l'autoriser à lui écrire des messages. Suite à ça la conversation peut enfin s'engager. 

AdopteUnMec a eu un business model assez mouvant depuis sa création puisqu'à l'origine gratuit il est devenu de plus en plus payant. 

Je n'ai jamais voulu payer pour avoir accès à ce genre de site et j'avais la chance d'avoir un compte crée au début du site et ayant donc un accès gratuit à certaines fonctionnalités. 

Un jour  (en 2010) je me suis rendu par un test tout simple (création d'un profil "femmes") que les profils d'hommes ayant un compte gratuit remontaient beaucoup moins dans les résultats de recherches. Je n'ai toutefois pas réussi à déterminer l'algorithme en place derrière. Afin d'optmiser mes chances sur ce site je suis parti dans une autre théorie : si je visite 100 profils et que ma fiche est intéressante alors un pourcentage (15% - ) va venir la lire, et parmis ces 15% une faible proportion va m'autoriser à lui écrire. Au final sur 100 filles, 3 ou 4 vont m'autoriser à leur écrire. Très intéressé par les études et stats je souhaitais mettre un système d'analyse et de remontée de ses stats mais je n'en ai pas eu le temps.

L'autre idée qui m'est venue en tête : au lieu de visiter manuellement ces profils ce qui est une tâche rébarbative et longue et si je faisais un programme faisant le travail à ma place ? Je me suis donc lancé dans la réalisation d'un script visitant automatiquement en peu de temps des profils définis selon plusieurs paramètres.

Initialement mon script faisait un travail classique : il simulait une connexion via le formulaire HTMl puis visitait les profils en chargant les pages HTML des profils. Après un temps d'utilisation je me suis rendu compte de plusieurs choses :
- les profils HTML étaient longs à charger, normal c'est une page HTML donc plusieurs centaines de KiloOctet à chaque fois.
- l'accès au site AdopteUnMec à travers la version "desktop" était restreinte entre 18h et 1h du matin, cela réduisait le temps d'utilisation potentiel du script. 

Suite à ça je me suis rendu compte que l'application Android d'AdopteUnMec ne subissait pas de blocage entre 18h et 1H du matin (toujours pour un même compte qui lui est bloqué sur la version "desktop" du site). La conclusion est simple si l'application Android arrive à visiter cela alors mon script pourra le faire. 

J'ai alors ouvert Wireshark qui est un utilitaire qui permet de capturer les packets passant sur le réseau. Pour vulgariser : ce logiciel permet de voir en partie les données transitant entre les ordinateurs du réseau observé. Pendant ce temps j'ai utilisé l'application Android sur tous les cas possibles : connexion, déconnexion, recherches, visites, charmes, envois de messages. Le but était d'obtenir toutes les urls appellées et les paramètres (les données que l'on donne à ces urls afin d'obtenir ce que l'on souhaite : l'identifiant du profil, les paramètres de recherches , etc).

Une fois toutes ces informations récupérées j'ai pris le temps de reconstruire tout mon script autour de cette API : au final les requêtes sont devenues plus rapides. En effet l'API retourne du JSON qui est donc rapidement exploitable par le serveur PHP alors qu'auparavant il fallait parser du HTML.

Une fois cela effectué tout était presque règlé : il y avait seulement des améliorations visant à ne pas être blacklisté par AdopteUnMec. Initialement cela était basé sur la fréquence des visites (si le temps était inférieur à 2 ou 3 secondes le compte pouvait être bloqué).

Puis vint un jour où j'ai voulu réaliser et monétiser une webapp à destination de tous les utilisateurs d'AdopteunMec. Le but de cette application est d'avoir une interface propre et épurée permettant de lancer automatiquement des visites sur un type de profil déterminé par la recherche. J'ai nommé le fait de visiter rapidement plusieurs dizaines de profils déterminées "Booster". Suite à cela j'ai appelé mon application "BoostAdopt.com". Ma recherche de nom de domaine a été très rapide mais une fois effectuée je me suis aperçu qu'existait déja le nom de domaine "boostAdopte.com". Ce dernier était désactivé avant d'acheter le nom que j'avais choisis.

Avant de foncer tête baissée j'ai réfléchi à toutes les problématiques que j'aurai à affronter sur ce projet.

Le premier problème fut le suivant : alors que j'enregistrais automatiquement les profils visités au bout de 100 000 hits (visites) l'IP du serveur appellant a été blacklistée. En clair mon serveur n'avait plus accès aux serveurs d'AdopteUnMec. même si un utilisateur ne va pas faire 100 000 d'un coup , admettons que 1000 utilisateurs fasse 100 visites et le service devient bloqué.
 Sur le papier il devint donc dur de proposer un service qui pourrait s'arrêter du jour au lendemain. Une solution aurait pu être de changer d'IP une fois chacune d'entre elle blacklistée mais cela aurait été coûteux et aurait rendu le site peu fiable qualitativement parlant. Imaginez qu'un utilisateur paie, lance le booster et se retrouve bloqué car le serveur vient d'être blacklisté : ça ne le fait pas. 

 Pour schématiser l'architecture précèdemment citée : mon serveur appellait les serveurs d'AdopteUnMec. 

 Quelques jours plus tard me vint une idée un peu plus folle: que ça soit l'IP propre à chaque utilisateur qui appelle le serveur AdopteUnMec. Cela a beaucoup d'avantages : je n'ai pas de changement d'IP à faire et le service reste fiable. 
 L'inconvénient majeur est qu'il faut donner le code "métier" du côté du navigateur donc facilement imitable et donc le business deviendrait réduit ou inopérant. En effet n'importe qui pourrait rréutiliser le code, le mettre à disposition gratuitement d'autres utilisateurs. 



