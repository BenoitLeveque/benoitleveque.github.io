---
layout: post
title:  Installation et configuration de mon environnement de développement
date:   2014-01-01 20:00:00
---

Cet article reprend dans les grandes lignes les différents outils que j'ai installés sur mon poste de développement, mais également les différentes ressources que j'ai pu utiliser pour personnaliser mon environnement de développement.

Mon environnement de développement est un environnement plutôt classique de développeur PHP / MySQL sous Mac OS X.
Je vais décrire ci-dessous toutes les étapes que j'ai effectuées pour avoir un environnement qui me correspond.

Tout d'abord PHP, sous Mac, on a plusieurs possibilités pour installer PHP, [MacPorts][macports], les binaires fournies par [Liip][liip_php], ou en utilisant [Homebrew][brew]. Ici j'ai décidé d'utiliser [Homebrew][brew] pour des raisons de cohérence avec tous les autres outils que je vais installer plus bas.

### Installation de Brew

Pour installer brew, rien de plus simple on suit ce qui est indiqué sur la documentation (ligne de code ci-dessous à coller dans votre terminal).

``` bash
ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go/install)"
```

Si vous n'avez pas encore installé xcode sur votre Mac, l'installer de brew va vous demander de l'installer ``xcode-select``.

A la fin de l'installation de brew, il va vous demander de lancer la commande ci-dessous avant d'installer des packages.

``` bash
brew doctor
```

### Installation de PHP 5.5

Pour installer PHP 5.5 j'ai suivi la procédure d'installation fourni par sur le [dépot git d'homebrew-php][homebrew_php]. j'ai simplement installé en plus les extensions ``php55-intl`` et ``php55-xdebug``

``` bash
brew tap homebrew/dupes
brew tap josegonzalez/homebrew-php

brew install php55
brew install php55-intl
brew install php55-xdebug
```

### Configuration de PHP

Par défaut, la version de php fournie par brew n'est pas complétement configuré, il faut donc aller toucher dans le fichier php.ini qui se trouve ici : ``/usr/local/etc/php/5.5/php.ini``

``` ini
date.timezone = Europe/Paris
```

On doit également configurer xdebug pour pouvoir l'utiliser sans problème avec Symfony2, il faut pour cela rajouter dans le fichier de configuration de xdebug ``/usr/local/etc/php/5.5/conf.d/ext-xdebug.ini`` les lignes ci-dessous.

``` ini
xdebug.cli_color=1
xdebug.remote_enable=on
xdebug.remote_handler=dbgp
xdebug.remote_host=localhost
xdebug.remote_port=9000
xdebug.max_nesting_level=200
```

### Installation de Mysql

Pour installer MySQL j'aurais simplement pu faire un ``brew install mysql`` mais j'aurais quand même dû télécharger le dmg de mysql pour récupérer le fichier prefpane sur le site de mysql, j'ai donc choisi de prendre directement le dmg sur le site de [MySQL][mysql].

Une fois télécharger la bonne version, j'ai pris la version ``Mac OS X 10.7 (x86, 64-bit), DMG Archive`` vous trouverez dans le dmg trois fichiers, le premier fichier correspond à MySQL, le second, un fichier prefpane qui permet d'ajouter comme son nom l'indiquait une entrée MySQL dans les ``Préférences système``. Le dernier fichier permet quant à lui de lancer MySQL au démarrage de votre Mac.

### Configuration d'apache

Apache est installé par défaut sur Mac OS X, il n'est simplement pas lancé. Vous pouvez le lancer avec la commande ci-dessous.

``` bash
sudo apachectl start
```

Au niveau configuration, il faut faire quelque modification simple du fichier httpd.conf.

Tout d'abord il faut activer la version de PHP que l'on vient d'installer, pour cela il faut remplacer la ligne qui commence par ``LoadModule php5_module`` par la ligne ci-dessous.

``` 
LoadModule php5_module /usr/local/opt/php55/libexec/apache2/libphp5.so
```

Par défaut apache lance des warnings concernant un problème avec le ``ServerName``. il faut simplement changer la ligne ``ServerName`` qui doit être commentée par la ligne ci-dessous.

```
ServerName localhost:80
```

Il ne reste plus qu'à activer les vhosts et vous aurez un apache fonctionnel pour faire du développement Symfony.

```
Include /private/etc/apache2/extra/httpd-vhosts.conf
```

Comme j'ai souvent des problèmes de cache avec mes applications Symfony2, j'ai également changé l'utilisateur utilisé par apache. Si vous n'avez pas de problème de cache vous pouvez sauter cette étape.

```
User Benoit
Group staff
```

### Manipulation du fichier hosts.

Si comme moi vous n'aimez pas aller toucher au fichier hosts, vous pouvez installer un petit utilitaire bien sympa, utilitaire utilisé par un certain nombre de développeurs ruby.

L'utilitaire POW créé par 37Signals permet de prendre en charge toutes les adresses ``*.dev`` pour vous.
Vous trouverez la procédure d'installation sur le [github de 37Signals][37Signals_pow]

``` bash
curl get.pow.cx/uninstall.sh | sh #if you have pow installed
echo 'export POW_DST_PORT=88' >> ~/.powconfig
sudo curl https://gist.github.com/soupmatt/1058580/raw/zzz_pow.conf -o /private/etc/apache2/other/zzz_pow.conf
sudo apachectl restart
curl get.pow.cx | sh
```

[macports]: https://trac.macports.org/wiki/howto/MAMP
[liip_php]: http://php-osx.liip.ch/
[brew]: http://brew.sh/
[homebrew_php]: https://github.com/josegonzalez/homebrew-php#installation
[mysql]: http://dev.mysql.com/downloads/mysql/
[37Signals_pow]: https://github.com/37signals/pow/wiki/Running-Pow-with-Apache

