---
layout: post
title: Dokuwiki (1ère partie)
modified:
categories: [Tuto]
description: Comment installer Dokuwiki
tags: [turoriel, dokuwiki]
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

Qu'est-ce qu'un wiki ? Si vous ne connaissez pas ce genre d'outil, rendez-vous sur [Wikipédia : Wiki](https://fr.wikipedia.org/wiki/Wiki) pour en apprendre un peu plus. 

Dokuwiki est simple à installer et à utiliser. De plus il est possible de faire beaucoup de choses avec [Dokuwiki](https://www.dokuwiki.org/) car le développement de vos propres plugins est très facile.

## A savoir
L'intérêt d'un tel site, c'est qu'il vous permet d'établir une documentation interne complète, que ce soit pour votre entreprise ou toute autre organisation. 

Ce tutoriel est normalement compatible avec n'importe quel debian-like (Debian, Ubuntu, Mint...). Les commandes devront donc être adaptées si vous utilisez d'autres distributions Linux.

## Prérequis

 * Une distribution Linux
 * Un serveur Web : [Apache](http://www.apache.org/httpd) ou [nginx](http://nginx.org/) par exemple.
{% highlight bash %}
sudo apt-get install apache2
{% endhighlight %}
 * Vous devez avoir Php d'installé :
{% highlight bash %}
sudo apt-get install php5
{% endhighlight %}

C'est tout.

## Installation des sources

Sur certaines distributions, il est tout à fait possible d'installer Dokuwiki en une seule commande :
{% highlight bash %}
sudo apt-get install dokuwiki
{% endhighlight %}

Mais je vous déconseille de faire ça. Autant installer votre serveur là ou vous voulez et l'installation par paquet installe dans plusieurs dossier. Autant tout centraliser pour faciliter les futures mises à jour.

Rendez-vous sur la [page de téléchargement](http://download.dokuwiki.org/) de Dokuwiki et copiez le lien de la version "Stable". 

Ouvrez un terminal et collez le lien copié :
{% highlight bash %}
cd /var/www/
wget http://download.dokuwiki.org/src/dokuwiki/dokuwiki-stable.tgz
{% endhighlight %}

Décompressez l'archive téléchargée et renommez la :
{% highlight bash %}
tar -xzvf dokuwiki/dokuwiki-stable.tgz
mv dokuwiki-DATE-VERSION monwiki
{% endhighlight %}

Donnez les droits à votre utilisateur (www-data pour Apache) :
{% highlight bash %}
sudo chown -R www-data:www-data /var/www/monwiki
{% endhighlight %}

Voilà le dossier de votre serveur est prêt.

## Serveur web

Il va falloir maintenant aller dire à Apache de nous faire tourner tout ça. Pour cela rajoutez un fichier de configuration pour votre Wiki dans le dssier de votre serveur web.

{% highlight bash %}
sudo vi /etc/apache2/sites-available/monwiki.conf
// Le dossier pour nginx est sensiblement identique
{% endhighlight %}

Et renseigner le selon votre configuration :

{% highlight apacheconf %}
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
{% endhighlight %}

Rechargez votre serveur web :

{% highlight apacheconf %}
sudo service apache2 reload
{% endhighlight %}

Ouvrez votre explorateur et entrez l'adresse suivante en remplaçant "ip_serveur" par votre ip :

{% highlight php %}
http://ip_serveur/mowiki/install.php
{% endhighlight %}

Vous devriez voir la page d'installation de Dokuwiki.

Si vous ne voyez rien ou que vous avez une page d'erreur, relisez ce tutoriel. Sinon vérifiez les logs d'Apache.
