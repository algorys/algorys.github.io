---
layout: page
title: A propos
image:
  feature: about.jpg
  credit: 
  creditlink:
comments: false
modified: 2015-11-20
---

# Jour après jour le boulot d'Administrateur/Intégrateur tu feras...

## Une Histoire

Après avoir fait une formation de Technicien Superieur en Réseau Informatique et Télécom, j'ai du monter une boucle de gestion de projet et d'intégration logicielle (et c'est pas encore fini...) pour une entreprise.

Et je vais essayer de partager mes connaissances sur ce site. Des serveurs que j'ai pu monter jusqu'au plugins utilisés ou au configurations mises en place.

_NB : ma boite créé des logiciels._

## Les Propos

Ce blog est destiné avant tout aux intégrateurs et aux administrateurs qui cherchent à réaliser une boucle complète d’intégration et de gestion de projets. Il aura pour but de documenter l’installation et la configuration de serveurs linux, ainsi que toute information tournant autour.

## Les Réjouissances

Pour faire un logiciel, il y a forcément du code informatique. J'ai donc choisi d'utiliser Git pour la gestion et le versionnement du code source et [Gitlab](https://gitlab.com) pour héberger les dépôts. Pour que le code puisse être repris par tous, et surtout documenté, j'utilise [Doxygen](http://www.doxygen.org/). [Redmine](http://www.redmine.org/) s'est très vite imposé comme choix pour la gestion de projet. Non seulement il permet de communiquer avec Gitlab (et donc de lier les `issues` avec les commits) mais c'est aussi une véritable usine à gaz puissante et complète.

En association avec tout ça, il me fallait un serveur pour pouvoir automatiser les compilations des sources sur différents serveurs. J'ai donc choisi [Jenkins](https://jenkins-ci.org) pour l'automatisation. Ce serveur en Java peut automatiser quasimmment toutes les tâches que peut faire un Sysadmin ou un Intégrateur. Il peut les éxécuter aussi bien sur un serveur Windows que sur un serveur Linux et fonctionne très bien avec Git.

Au final, il est donc possible de déclencher des "jobs" de Jenkins en "poussant" les sources du code vers Gitlab, et avoir les résultats sur Redmine et par mail.

Par la suite, j'ai monté en parallèle une messagerie interne avec [Prosody](https://prosody.im/) et le protocole XMPP. Il est aussi possible d'afficher les résultats des compilations sur un salon dédié.

Et enfin, j'ai choisi [Dokuwiki](https://www.dokuwiki.org/) pour créer un Wiki interne et pouvoir déposer toute la documentation sur celui-ci.

Je rajouterais également les autres serveurs qui ne font pas partie de cette boucle d'intégration logicielle, comme [Puppet](https://puppetlabs.com/) qui permet de faire de l'automation.

#### Créateur du thème

<div markdown="0"> <a href="https://github.com/mmistakes/hpstr-jekyll-theme" class="btn btn-success">Download HPSTR</a></div>
