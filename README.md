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

- /etc : `Stocke des fichiers divers de configuration système.`
- /bin et /usr/bin : `Contient des fichiers executables (binaries), /bin pour des fichiers faisant partie du noyau de l'OS, /usr/bin les executables pour les utilisateurs de cette OS (system wide).`
- /sbin et /usr/sbin : `Similaire à /bin et /usr/bin, mais pour des executables nécessitant la permission root.`
- /home : `Répertoire personnel d'un utilisateur.`
- /var : `Stocke des données variables (qui peuvent/vont changer).`
- /var/log : `Stocke des logs d'applications.`
- /var/lib : `Stocke des données importantes et persistantes (mais qui peuvent changer) de programmes.`

## Où est le pilote ?

Comment retrouver, sous GNU/Linux :

- La liste des pilotes chargées en mémoire ? `lsmod`
- La liste des fichiers binaires _(modules)_ qui les fournissent ? `modinfo [modulename]` 
- Forcer leur chargement et leur déchargement ? `sudo modprobe [modulename]` (pour le chargement) `sudo modprobe -r [modulename]` (pour le déchargement) 
- Les messages qu'ils ont émis ? `dmesg`

## MS Windows

Comment fonctionne en pratique WSL _(Windows Subsystem for Linux)_ ? Que permet-il de faire ? Quels protocoles utilise-t-il ?

~~~~
En pratique, WSL2 (la dernière version) fait tourner un noyau Linux dans une machine virtuelle légère et gère aussi la communication entre cette VM et le reste de l'environnement Windows. Ceci permet d'installer des distributions Linux, d'utiliser des services, serveurs et outils graphiques Linux localement sur Windows, d'executer des scripts Bash et d'accéder aux fichiers Windows depuis Linux.
Les protocoles utilisés pour la communications entre les deux sont 9P, VSOCK, TCP/IP (NAT) et RDP.
~~~~

## On commence

Vous venez d'installer un système GNU/Linux que faites-vous pour en sécuriser les accès via SSH ?

### Réponse

J'installe openssh
~~~~
sudo apt install openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh
~~~~
J'ouvre le fichier de configuration SSH.
~~~~
sudo vim /etc/ssh/sshd_config
~~~~
Je désactive la connexion root: `PermitRootLogin no`
Je force la connexion par clef `PasswordAuthentication no` `PubkeyAuthentication yes`


## Alerte !

Vous vous connectez à un système GNU/Linux, le client ssh vous averti qu'il a un problème de reconnaissance de sa clef publique
d'hôte. Que faites-vous ? Pourquoi ?

### Réponse

1. Je vérifie l'empreinte du serveur et confirme que le changement est normal car cela pourrait être une tentative d'attaque.
2. Si le changement est normal, je supprime l'entrée précédente `ssh-keygen -R nom_du_serveur` et je me reconnecte en acceptant la nouvelle clé.
3. Sinon, je fais pars du problème/je cherche la cause du changement.


## Oh les gourmands !

Comment identifier rapidement, avec une seule ligne de commande, les utilisateurs qui consomment le plus d'espace de stockage.

_N.B._ Vous pouvez supposer que les répertoires personnels sont tous des sous-répertoires de `/home` 

~~~~
du -sh /home/* 2>/dev/null | sort -hr
~~~~

## On va aider les devs...

Une équipe de développeur.se.s vous demande de préparer un système GNU/Linux pour du développent système (C, make, etc.)

Que faites-vous en pratique en tant qu'administrateur.trice pour commencer ?

- Sur une distribution de la famille Debian
- Sur une distribution de la famille Red Hat

### Distribution Debian

~~~~
sudo apt update
sudo apt upgrade 
sudo apt install build-essential git vim cmake
sudo groupadd dev -g 5001
sudo useradd -m -c "Dev1" -G dev dev1
sudo useradd -m -c "Dev2" -G dev dev2
sudo passwd dev1 
sudo passwd dev2
sudo chsh -s /bin/bash dev1
sudo chsh -s /bin/bash dev2
sudo mkdir /srv/devteam
sudo chgrp dev /srv/devteam
sudo chmod g=rwx /srv/devteam
sudo chmod g+s /srv/devteam
~~~~

### Distribution Red Hat

~~~~
sudo dnf update
sudo dnf groupinstall "Development Tools"
sudo dnf install git vim cmake
sudo groupadd -g 5001 dev
sudo useradd -m -c "Dev1" -G dev -s /bin/bash dev1
sudo useradd -m -c "Dev2" -G dev -s /bin/bash dev2
sudo passwd dev1
sudo passwd dev2
sudo chsh -s /bin/bash dev1
sudo chsh -s /bin/bash dev2
sudo mkdir /srv/devteam
sudo chgrp dev /srv/devteam
sudo chmod g=rwx /srv/devteam
sudo chmod g+s /srv/devteam
~~~~

## Qui fait quoi ?

Comment pourriez-vous retrouver la liste des dernières connexions d'utilisateurs à un système GNU/Linux, autant à distance que localement ?

~~~~
last
~~~~

Comment identifier les sessions où un.e utilisateur.trice à utilisé _sudo_ pour avoir les droit d'administration ?

~~~~
journalctl _COMM=sudo
~~~~

## Post mortem

Le système que vous administrez est tombé cette nuit. Comment récupérer des informations sur ce qui s'est passé ?

~~~~
last reboot
journalctl -b -1
journalctl -k -b -1
less /var/log/syslog
systemctl --failed
journalctl -u service -b -1
df -h
~~~~

## On surveille...

Sur un serveur GNU/Linux qui démarre via _systemd_, un service nommé _bidule_, comment :

- Savoir si le service est lancé au démarrage automatiquement : `systemctl is-enabled bidule`
- Savoir s'il est en cours de fonctionnement : `systemctl is-active bidule`
- Faire en sorte qu'il le soit : `sudo systemctl start bidule`
- Examiner les messages qu'il émet : `journalctl -u bidule`
- Le redémarrer : `sudo systemctl restart bidule`
- Déterminer de quel paquet (Debian ou Red Hat) il provient : `dpkg -S /usr/bin/bidule` (Debian) `rpm -qf /usr/sbin/bidule` (Red Hat)

## Zut

Vous devez reprendre la main, en tant qu'administrateur, d'un système GNU/Linux mais vous n'avez aucun mot de passe à votre disposition, ni _root_ ni utilisateur. Que pouvez-vous faire ?

~~~~
Redémarre
On interrompt GRUB en appuyant sur une touche
On sélectionne l'entrée de menu qu'on veut démarrer (la première)
On appuie sur 'e' (edit)
On se déplace jusqu'à la ligne "linux ..."
On ajoute à la fin de la ligne init=/bin/bash et on boot cette configuration (ctrl+X ou F10)
On remonte la racine en écriture : "mount -o remount,rw /"
On répare : par exemple : passwd root
On ne peut rebooter de façon normale : "mount -o remount,ro /"
Extinction/reboot hardware
~~~~

## C'est la faute à Rémy

Un service réseau http(s) ne fonctionne plus. Vous avez accès à ce système comme administrateur.trice. Quels outils utilisez-vous pour diagnostiquer le problème, indiquez précisément ce que chaque outil permet de vérifier.

## Au charbon !

On vous demande d'installer un serveur de base de données _Elastic_ (autrefois nommé _Elastic Search_) sur un système Debian (ou autre si vous préférez)

Décrivez votre recherche documentaire, la méthode que vous sélectionnez, les commandes que vous exécutez et les fichiers de configuration impliqués.

### Réponse

Je commence par regarder la documentation a : https://www.elastic.co/guide/en/elasticsearch/reference/current/deb.html
Et je suis les étapes 1 à 1
~~~~
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
~~~~

Je décide d'installer à partir du répertoire APT
~~~~
sudo apt-get install apt-transport-https
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/9.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-9.x.list
sudo apt-get update && sudo apt-get install elasticsearch
~~~~

## Le Web

Vous avez la charge de gérer un serveur Web sous Debian GNU/Linux, pour héberger plusieurs sites Web (hébergement mutualisé).

- Quels paquets installez-vous ? 
- - `apache2, apache2-utils`
- Comment organisez sous le stockage des différents contenus ? 
- - `Dans /var/www/ des répertoires nomdusite.com/ pour chaque site, et dans chaque répertoire, un répertoire public_html/ et un répertoire logs/`
- Certains sites ont besoin de PHP, comment l'installez-vous ? 
- - `sudo apt install php libapache2-mod-php php-cli`
- - `sudo a2enmod php8.4`
- - `sudo systemctl restart apache2`
- Comment pouvez-vous organiser l'enregistrement séparé des visites des différents sites ?
- - `Set up un fichier virtual host par site dans /etc/apache2/sites-available/`
- Quel(s) outil(s) peuvent vous permettre de fournir une vison des visites de chaque site à chaque Webmaster ?
- - `AWStats, Webalizer`

