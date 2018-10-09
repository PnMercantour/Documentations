# VirtualBox #

## Partage du presse-papier ##

Il est possible dans VirtualBox de partager le presse-papier entre la machine hôte et la machine virtuelle. Cette manip permet également à la machine virtuelle d'adapter sa taille de fenêtre à l'écran.

Pour cela, il faut installer les paquets:

	apt update
	apt install linux-headers-amd64 gcc make perl
	
Puis, via l'onglet `Périphériques`, cliquer sur `Insérer l'image CD des Additions Invités...`. Ensuite, ouvrir un terminal dans le CD `VBox_GAs_5.2.12` puis passer en `root`. Et exécuter:

	sh ./VBoxLinuxAdditions.run
	
Redémarrez la machine virtuelle.

Pour finir, dans l'onglet `Périphériques`, `Presse-papier partagé`, activer `Bidirectionnel`.

### Remarque ###

Si votre VM vient d'être installée, faire `apt update` devrait vous renvoyer une erreur. Pour la corriger, faite en `root`:

	nano /etc/apt/source.list
	
Et commentez la ligne `deb cdrom:[Debian GNU/Linux 9.4.0 _Stretch_ - Official amd64 xfce-CD Binary-1 20180310-11:21]/ stretch main`.

## Connexion à sa VM en local ##

Il est possible d'utiliser sa VM comme d'un serveur (Lizmap par exemple) et de l'atteindre via sa machine hôte et sans utiliser le réseau internet externe.

Pour cela, dans la `Configuration` de la VM, onglet `Réseau`, il faut basculer le `Mode d'accès réseau` sur `Réseau privé hôte`.

Une fois la VM démarré, il faut récupérer l'adresse IP de la machine en exécutant par exemple:

	/sbin/ip -4 addr
	
Et récupérer l'adresse IP en `192.168.xxx.xxx`.
	
Dans le navigateur de sa machine hôte, il sera alors possible d'accéder au site web contenu sur la VM. Pour Lizmap par exemple, `192.168.xxx.xxx/lm`.

A noter : A la suite de cette manip, la VM ne pourra plus accéder à internet. Il faudra rebasculer sur la configuration réseau `NAT` pour reconnecter la VM à internet.

## Utilisation d'un dossier partagé hôte/invité ##

Afin de pouvoir utiliser des fichiers de sa machine hôte (Windows dans notre cas) vers la machine invité (notre VM Debian pour nous), il faut configurer un `Dossier partagé`.

Pour cela, avant de démarrer notre VM, dans ses `Configuration`, onglet `Dossiers partagés`, il faut ajouter un `Dossiers permanents` avec `Montage autoamtique` et `Configuration permanente`.

Une fois notre VM démarré, dans un terminal en `root` ou via `sudo` :

	usermod -a -G vboxsf mon_utilisateur
	mount -t vboxsf monDossierPartage /media/sf_monDossierPartage

Dans les `Périphériques` de votre machine invité, vous aurez alors un nouveau dossier qui sera synchronisé avec votre machine hôte.
