---
layout: post
title:  Installation et configuration de mon environnement de développement"
date:   2013-12-06 22:00:00
---

1. Installation de Brew

ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go/install)"

ça va installer xcode-select s’il n’est pas installé

brew doctor

2. installation de PHP 5.5

https://github.com/josegonzalez/homebrew-php

brew tap homebrew/dupes
brew tap josegonzalez/homebrew-php

brew install php55
brew install php55-intl
brew install php55-xdebug

3. installation de Mysql

Téléchargement du DMG depuis le site de mysql : http://dev.mysql.com/downloads/mysql/

installation du fichier pkg, du prefpan et du startup pkg

4. installation de compass

sudo gem install compasse

5. Installation de Jekyll pour mon blog

sudo gem install jekyll


6. installation de POW
https://github.com/37signals/pow/wiki/Running-Pow-with-Apache

$ curl get.pow.cx/uninstall.sh | sh #if you have pow installed
$ echo 'export POW_DST_PORT=88' >> ~/.powconfig
$ sudo curl https://gist.github.com/soupmatt/1058580/raw/zzz_pow.conf -o /private/etc/apache2/other/zzz_pow.conf
$ sudo apachectl restart
$ curl get.pow.cx | sh

7. installation de oh-my-zsh

curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
...

8. Configuration de iterm2 (theme + font + font size)

Police Menlo en 14pt
Scrollback lines : Unlimited scrollback
Theme : iterm custom qui se trouve dans mon dossier download

9. Configuration de git
 include fichier gitconfig et gitignore global

10. configuration apache2
	activation de php dans apache

	LoadModule php5_module /usr/local/opt/php55/libexec/apache2/libphp5.so

	Lancement d'apache avec mon utilisateur
	
	User Benoit
	Group staff

	Changement du serverName

	ServerName localhost:80

	Activation du fichier de vhost en décommentant la ligne ci-dessous
	Include /private/etc/apache2/extra/httpd-vhosts.conf

	Création du fichier dans /etc/apache2/user pour avoir acces à http://localhost/~Benoit/

		<Directory>
		    Options Indexes MultiViews
		    AllowOverride All
		    Order allow,deny
		    Allow from all
		</Directory>

11. configuration de php (php.ini)

Modification de la timezone
date.timezone = Europe/Paris

12. installation globale de composer
brew install composer

13. Installation de phpcs avec composer
composer global require 'squizlabs/php_codesniffer=*'

