---
layout: post
title: Installer Shinken pour GLPI
modified:
categories: [Tuto]
description: comment installer Shinken
tags: [tutoriel, shinken, monitoring, nagios]
image:
  feature:
  credit:
  creditlink:
comments:
share:
date: 2016-03-08T10:00:05+01:00
---

# Introduction

Dans ce tutoriel, nous allons voir comment installer [Shinken](http://www.shinken-monitoring.org/index.php) sur un serveur Linux. En soit l'installation de Shinken n'est vraiment pas difficile et peut être faites de plusieurs manières. La [Documentation](http://shinken.readthedocs.org/en/latest/index.html) est vraiment très bien faites et agréable à lire. Toutefois, ce tutoriel va montrer l'installation via les sources et notamment la façon dont on peut configurer Shinken avec un serveur GLPI et le plugin [glpi_monitoring](https://github.com/ddurieux/glpi_monitoring).

# Les différentes installations de Shinken

Si vous êtes allé faire un tour sur la documentation de Shinken, vous avez pu voir qu'il existe 3 manière différentes : via [Pip](https://pip.pypa.io/en/stable/) (l'installateur de modules de Python) qui est souvent à jour, via les paquets qui ne sont parfois pas à jour et enfin via les sources du [dépôt Shinken](https://github.com/naparuba/shinken).

Livbre à vous d'installer Shinken comme bon vous semble. Mais le **plus important** est de n'utiliser qu'**une seule manière** pour l'installation et les mises à jour de Shinken ! En aucun cas vous devez mélanger plusieurs type d'installations sous peine de voir votre serveur planter, avoir des bugs ou ne plus marcher du tout !

# Prérequis

Shinken est écrit en Python, il aura donc besoin d'une version de Python pour fonctionner (version 2.6 ou supérieur et si possible 2.7 pour de meilleurs performances). Il aura aussi besoin de [python-pycurl](http://pycurl.io/) et de [setuptools](https://pypi.python.org/pypi/setuptools/).

Voici les différentes étapes pour ces dépendances, en sachant que ce tutoriel a été fait sur un Ubuntu Server 14.04.4 LTS :

## Installer Python

Pour installer Python, rien de plus simple :

```bash
sudo apt-get update
sudo apt-get install python
```

En général, sur Ubuntu Server, Python 2.7 est déjà installé par défaut. Il est possible que sur d'autres OS il ne soit pas disponible via les dépôts de la distribution. Renseignez vous alors sur le site de votre distribution.

### Installer PyCurl

Pour installer PyCurl, il suffit de l'installer via les paquets :

```bash
sudo apt-get install python-pycurl
```

### Installer setuptools

Pour installer SetupTools, la démarche est la même :

```bash
sudo apt-get install python-setuptools
```

Voilà, vous avez normalement toutes les dépendances requises pour l'installation de Shinken.

# Récupération des sources de Shinken

Pour récupérer les sources de Shinken et surtout la dernière _release_ rendez vous sur le [dépôt de Shinken](https://github.com/naparuba/shinken) et téléchargez-la :


```bash
sudo apt-get install -y git
cd ~
git clone https://github.com/naparuba/shinken.git 
cd shinken
# Vous pouvez voir les différentes release en tapant "git tag -l"
git checkout 2.4.2
```

Voilà, votre dossier Shinken est prêt, vous allez pouvoir lancer l'installation.

> Vous pouvez bien évidemment la télécharger avec `wget` ou via une interface graphique, mais je préfère cette méthode, car elle permet de garder une version de Shinken à jour pour votre serveur. Avec Git vous n'aurez qu'à "tirer" les nouvelles version et vous remettre sur un `tag` plus récent.
 
# Installer Shinken

Vous allez maintenant pouvoir lancer l'installation de Shinken avec Python. Mais avant cela, Shinken aura besoin d'un utilisateur `shinken` pour fonctionner. Crééz-le avant de lancer le setup :

```bash
adduser shinken
```

Une fois cela fait, vous pouvez la commande d'installation :

```bash
cd ~/shinken
sudo python setup.py install
```

Normalement l'installation devrait se dérouler sans problème. Vous aurez quelques `warning` au début pour signaler qu'il n'est pas sous Windows, etc... mais surtout à la fin de l'installation vous aurez les lignes suivantes :

```bash
Finished processing dependencies for Shinken==2.4.2
Changing owner of /etc/shinken to shinken:shinken
Changing owner of /var/run/shinken to shinken:shinken
Changing owner of /var/log/shinken to shinken:shinken
Changing owner of /var/lib/shinken/ to shinken:shinken
Changing owner of /var/lib/shinken/libexec to shinken:shinken
Changing owner of /usr/bin/shinken-receiver to shinken:shinken
Changing owner of /usr/bin/shinken-discovery to shinken:shinken
Changing owner of /usr/bin/shinken-arbiter to shinken:shinken
Changing owner of /usr/bin/shinken-poller to shinken:shinken
Changing owner of /usr/bin/shinken-broker to shinken:shinken
Changing owner of /usr/bin/shinken-scheduler to shinken:shinken
Changing owner of /usr/bin/shinken to shinken:shinken
Changing owner of /usr/bin/shinken-reactionner to shinken:shinken
Notice: for better performances for the daemons communication, you should install the python-cherrypy3 lib
Shinken setup done
```

Comme vous pouvez le voir, Shinken a donné les droits à l'utilisateur `shinken` que vous avez créé au préalable, sur les dossiers dont il a besoin. Vous n'avez donc rien à faire de ce côté-ci.

Il vous signale aussi que les `démons` (`daemons` en anglais) de Shinken seront plus performants si vous installer la librairie `python-cherrypy3`. Cette dépendance est optionnelle donc libre à vous de l'installer ou pas :

```bash
sudo apt-get install -y python-cherrypy3
```

Voilà, Shinken est prêt à fonctionner. Vous pouvez l'activer et le démarrer :

```bash
sudo update-rc.d shinken defaults
sudo service shinken start
```

Vous devriez avoir la sortie suivante :

```bash
Starting scheduler: 
   ...done.
Starting poller: 
   ...done.
Starting reactionner: 
   ...done.
Starting broker: 
   ...done.
Starting receiver: 
   ...done.
Starting arbiter: 
   ...done.
```

Félicitation, votre Shinken est maintenant opérationnel et va pouvoir monitorer les différents serveurs de votre parc. Mais pour cela, il va falloir installer quelques plugins de Shinken et notamment une interface Web.

# Installer l'interface Web

Pour que vous puissiez voir si vos hôtes sont bien répertoriés et leurs états, une interface Web est disponible pour Shinken. Pour l'installer il va falloir que vous mettiez un peu plus les mains dans le "cambouis". Et donc dans les fichiers de configurations de Shinken.

Pour cela, connectez-vous en tant qu'utilisateur Shinken :

```bash
sudo su - shinken
```

Et initialisez la CLI de Shinken pour générer le fichier ini contenant les chemins vers les différents répertoires de configuration :

```bash
shinken --init
```

Vous devriez avoir la sortie suivante :

```bash
Creating ini section paths
Creating ini section shinken.io
Saving the new configuration file /home/shinken/.shinken.ini
```

Maintenant vous pouvez installer les modules requis.

* L'interface WebUI : `shinken install webui`
* Le module d'identification : `shinken install auth-cfg-password`
* Le module pour stocker les données des utilisateurs : `shinken install sqlitedb`

En tapant chaque commande vous devriez avoir la sortie :

```bash
Grabbing : module_name
OK module_name
```

Une fois les modules installés, vous allez devoir indiquer à Shinken que vous voulez les utiliser, dans ses fichiers de configuration.

Éditez le fichier `/etc/shinken/brokers/broker-master.cfg` et trouvez la ligne non commentée où il y a `modules` pour y ajouter **webui** :

```conf
# - webui                   = Shinken Web interface
# - glpidb                  = Save data in GLPI MySQL database
modules    webui

# Enable https or not
```

Éditez ensuite le fichier `/etc/shinken/modules/webui.cfg` de la même manière pour y ajouter **auth-cfg-password** et **SQLitedb** :

```conf
# - SQLitedb              = Save user preferences to a SQLite database
modules     auth-cfg-pasword,SQLitedb
## Advanced Options
```

Une fois toutes ces manipulations effectuées, vous pouvez aller changer le mot de passe par défaut du compte `admin` dans le fichier `/etc/shinken/contacts/admin.cfg` :

```conf
define contact{
    use             generic-contact
    contact_name    admin
    email           shinken@localhost
    pager           0600000000   ; contact phone number
    password        your_password
    is_admin        1
    expert          1
}
```

Enregistrez et quittez.

Vous pouvez vérifier vos configurations en tapant la commande suivante (en étant utilisateur `sudo` ou `root`) :

```bash
sudo service shinken check
```

Redémarrez les services Shinken pour prendre en compte les nouvelles configurations :

```bash
sudo service shinken restart
```

Voilà, votre interface Web est configurée. Si tous les démons de Shinken ont bien été redémarrés, vous devriez voir la page suivante en vous rendant sur `http://ip_serveur:7767` :

<figure>
    <img src="/images/shinken/shinken_webui.png" alt="">
    <figcaption>Shinken - Écran de connexion</figcaption>
</figure>

