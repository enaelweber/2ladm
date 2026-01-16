# Examen Administration système 16 janvier 2026

Attendu : une url menant à un fichier README.md (format _markdown_) sur une plateforme GIT publique _(codeberg, gitlab, github, ...)_.
reprenant ce document en le complétant.

## Les deux familles

Quelles sont les principales distributions de la famille Debian, de la famille Red Hat, et les autres ?

Comment pourriez-vous décrire, succintement, leurs positionnements respectifs ? Les différences au niveau des outils d'admnistration ?

~~~~
La famille Debian a comme principales distributions une qui est éponyme, ainsi que d'autres très connues comme Ubuntu et Linux Mint. Ils se focalisent principalement sur le concept de logiciels libres: "We want to create a free operating system, freely available for everyone." Les outils administratifs principaux de cette famille sont apt/dpkg (gestion de paquets) et GNOME.

La famille Red Hat a comme principales distributions Red Hat Linux, son successeur spirituel Fedora, et CentOS. Ils se focalisent principalement sur la stabilité. Les outils administratifs principaux de cette famille sont dnf/rpm (gestion de paquets) et Cockpit.

Une autre distribution populaire est openSUSE. Elle se focalise sur accorder aux utilisateurs le plus de choix possible et la transparence. Les outils administratifs principaux de cette famille sont zypper/rpg (gestion de paquets) et YaST.
~~~~

## Back to basics 

Quelle est la différence fondamentale entre du code qui tourne en mode kernel/superviseur et en mode utilisateur ?

~~~~
Le mode utilisateur est restreint en ce qu'il peut faire, notamment quand à son accès au CPU et à la mémoire. Tout code tournant en mode utilisateur passe quand même par le Kernel pour être exécuté, un peu comme s'il était surveillé par celui-ci.
Du code tournant en mode Kernel a essentiellement tous les droits possibles. Il peut tout effectuer.
~~~~

## Qui est où ?

Indiquez, en quelques mots, pour ces répertoires, ce qu'ils sont supposés contenir :

- /etc: 
- /bin et /usr/bin
- /sbin et /usr/sbin
- /home
- /var
- /var/log
- /var/lib

## Où est le pilote ?

Comment retrouver, sous GNU/Linux :

- La liste des pilotes chargées en mémoire ?
- La liste des fichiers binaires _(modules)_ qui les fournissent ?
- Forcer leur chargement et leur déchargement ?
- Les messages qu'ils ont émis ?

## MS Windows

Comment fonctionne en pratique WSL _(Windows Subsystem for Linux)_ ? Que permet-il de faire ? Quels protocoles utilise-t-il ?

## On commence

Vous venez d'installer un système GNU/Linux que faites-vous pour en sécuriser les accès via SSH ?

## Alerte !

Vous vous connectez à un système GNU/Linux, le client ssh vous averti qu'il a un problème de reconnaissance de sa clef publique
d'hôte. Que faites-vous ? Pourquoi ?

## Oh les gourmands !

Comment identifier rapidement, avec une seule ligne de commande, les utilisateurs qui consomment le plus d'espace de stockage.

_N.B._ Vous pouvez supposer que les répertoires personnels sont tous des sous-répertoires de `/home` 

## On va aider les devs...

Une équipe de développeur.se.s vous demande de préparer un système GNU/Linux pour du développent système (C, make, etc.)

Que faites-vous en pratique en tant qu'administrateur.trice pour commencer ?

- Sur une distribution de la famille Debian
- Sur une distribution de la famille Red Hat

## Qui fait quoi ?

Comment pourriez-vous retrouver la liste des dernières connexions d'utilisateurs à un système GNU/Linux, autant à distance que localement ?

Comment identifier les sessions où un.e utilisateur.trice à utilisé _sudo_ pour avoir les droit d'administration ?

## Post mortem

Le système que vous administrez est tombé cette nuit. Comment récupérer des informations sur ce qui s'est passé ?

## On surveille...

Sur un serveur GNU/Linux qui démarre via _systemd_, un service nommé _bidule_, comment :

- Savoir si le service est lancé au démarrage automatiquement
- Savoir s'il est en cours de fonctionnement
- Faire en sorte qu'il le soit
- Examiner les messages qu'il émet
- Le redémarrer
- Déterminer de quel paquet (Debian ou Red Hat) il provient

## Zut

Vous devez reprendre la main, en tant qu'administrateur, d'un système GNU/Linux mais vous n'avez aucun mot de passe à votre disposition, ni _root_ ni utilisateur. Que pouvez-vous faire ?

## C'est la faute à Rémy

Un service réseau http(s) ne fonctionne plus. Vous avez accès à ce système comme administrateur.trice. Quels outils utilisez-vous pour diagnostiquer le problème, indiquez précisément ce que chaque outil permet de vérifier.

## Au charbon !

On vous demande d'installer un serveur de base de données _Elastic_ (autrefois nommé _Elastic Search_) sur un système Debian (ou autre si vous préférez)

Décrivez votre recherche documentaire, la méthode que vous sélectionnez, les commandes que vous exécutez et les fichiers de configuration impliqués.

## Le Web

Vous avez la charge de gérer un serveur Web sous Debian GNU/Linux, pour héberger plusieurs sites Web (hébergement mutualisé).

- Quels paquets installez-vous ?
- Comment organisez sous le stockage des différents contenus ?
- Certains sites ont besoin de PHP, comment l'installez-vous ?
- Comment pouvez-vous organiser l'enregistrement séparé des visites des différents sites ?
- Quel(s) outil(s) peuvent vous permettre de fournir une vison des visites de chaque site à chaque Webmaster ?


