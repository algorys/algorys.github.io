---
layout: post
title: Dokuwiki (1ère partie)
modified:
categories: [Tuto]
description: tutoriel Dokuwiki serveur debian
tags: [turoriel, dokuwiki, wiki]
image:
  feature:
  credit:
  creditlink:
comments:
share:
date: 2015-11-21T22:05:05+01:00
---

# Installation du serveur

## Pourquoi Dokuwiki ?

Si vous ne connaissez pas ce genre d'outil, rendez-vous sur [Wikipédia : Wiki](https://fr.wikipedia.org/wiki/Wiki) pour en apprendre un peu plus. 

[Dokuwiki](https://www.dokuwiki.org/) est simple à installer et à utiliser. De plus il est possible de faire beaucoup de choses avec, car le développement de vos propres plugins est assez facile pour peu que vous ayez quelques notions de programmation.

## A savoir
L'intérêt d'un tel site, c'est qu'il vous permet d'établir une documentation interne complète, que ce soit pour votre entreprise ou toute autre organisation, jeu, univers, etc... 

Ce tutoriel est normalement compatible avec n'importe quel debian-like (Debian, Ubuntu, Mint...). Les commandes devront donc être adaptées si vous utilisez d'autres distributions Linux.

## Prérequis

 * Une distribution Linux
 * Un serveur Web : [Apache](http://www.apache.org/httpd) ou [nginx](http://nginx.org/) par exemple.
```bash
sudo apt-get install apache2
```
 * Vous devez avoir Php d'installé :
```bash
sudo apt-get install php5
```

C'est tout.

## Installation des sources

Sur certaines distributions, il est tout à fait possible d'installer Dokuwiki en une seule commande : *sudo apt-get install dokuwiki et suivez le tutoriel de votre distribution*.

>**Cependant, je vous déconseille de faire ça.** 

L'installation par paquet installe dans plusieurs dossier. Pour Dokuwiki autant tout mettre dans le même dossier pour faciliter les futures mises à jour. Vous allez voir c'est très simple et autant installer votre serveur là ou vous voulez ! 

Sinon, rendez-vous sur la [page de téléchargement](http://download.dokuwiki.org/) de Dokuwiki et copiez le lien de la version "Stable". 

>Ouvrez un terminal et collez le lien copié :
```bash
cd /var/www/
sudo wget http://download.dokuwiki.org/src/dokuwiki/dokuwiki-stable.tgz
```

Décompressez l'archive téléchargée et renommez la :
```bash
sudo tar -xzvf dokuwiki-stable.tgz
sudo mv dokuwiki-DATE-VERSION monwiki
sudo rm dokuwiki-stable.tgz
```

Donnez les droits à votre utilisateur (www-data pour Apache) :
```bash
sudo chown -R www-data:www-data data conf
```

Voilà le dossier de votre serveur est prêt.

## Serveur web

Il va falloir maintenant aller dire à Apache de nous faire tourner tout ça. 

>Rajoutez un fichier de configuration pour votre Wiki dans le dossier de votre serveur web.

```bash
sudo vi /etc/apache2/sites-available/monwiki.conf
```

Et renseigner le selon votre configuration :

```apache
<VirtualHost *:80>
    # Détails du serveur
    ServerName monwiki.domaine.com
    ServerAlias monalias
    
    ServerAdmin admin@mail.com

    # Options
    DocumentRoot /var/www/monwiki
    <Directory /var/www/monwiki>
    Options Indexes FollowSymLinks MultiViews
      Order allow,deny
      allow from all
    </Directory>

    # Logs
    ErrorLog ${APACHE_LOG_DIR}/wiki_error.log
    CustomLog ${APACHE_LOG_DIR}/wiki_access.log combined

</VirtualHost>
```

>Rechargez votre serveur web :

```apache
sudo a2ensite monwiki.conf
sudo service apache2 reload
```

Ouvrez votre explorateur et entrez l'adresse suivante en remplaçant "ip_serveur" par votre ip :

```php
http://ip_serveur/install.php
```

Vous devriez voir la page d'installation de Dokuwiki.

Si vous ne voyez rien ou que vous avez une page d'erreur, relisez ce tutoriel. Sinon vérifiez les logs d'Apache.
