---
layout: responsive
titre: Fabriquer un photomaton avec un Raspberry Pi
---

# Fabriquer un photomaton avec un Raspberry Pi

[!] - rédiger rapidement - à revérifier et à corriger - [!]

[[source]](https://learn.adafruit.com/instant-camera-using-raspberry-pi-and-thermal-printer/system-setup)



Afin de fabriquer un photomation il faut plusieurs éléments : de quoi prendre des photos, de quoi les imprimer et de quoi déclencher le tout.

Pour faire cela tout allons utiliser une webcam ou une camera Pi, une imprimante thermique et un bouton. Mais le plus important, pour relier tout ça ensemble, nous allons utiliser un Raspberry Pi.



#### Les composants :

- Un Raspberry Pi
  - Avec un alimentation 5 V et 2 à 3 A
  - Une Carte microSD (2Go minimum)
- Une Raspberry Pi Cam ou webcam
- Une Imprimante thermique
  - Les cables ou fils necessaire pour l'alimenter et communiquer avec (dépend du modèle)
  - Des rouleaux de papier thermique
- Et un bouton poussoir



##### Pour la mise en place vous aurez besoin :

- D'un ordinateur avec une connexion internet
- D'un lecteur de carte SD et un adaptateur microSD vers SD
- Un écran et un cable HDMI
- Un Clavier et une souris
- Un cable ethernet ou un accès WiFi (pour le Raspberry Pi, si son modèle le permet)
- Des cables jumper ou du fil électrique


Maitenant qu'on a tout il ne reste plus qu'a s'y mettre et construire notre photomaton.


## Préparation et configuration du Raspberry Pi

::10 à 20 minutes::

On va commencer par préparer notre Raspberry Pi car il est vide et pas configurer pour l'usage que l'on va en faire.

#### 	Installation du système d'exploitation sur carte SD

Sans système de logique ou d'exploitation la machine n'est rien qu'un tas de composants avec lesquelles il est difficile d'interagir. Alors on va télécharger le système d'exploitation Raspbian et un utilitaire pour nous permettre de la graver sur notre carte microSD.

Lien vers Raspbian

Lien vers Etcher

#### 	Configuration de Raspbian

Config....

Langue, timezone etc...

`raspi-config`

optionnel mais recommandé :

sous "options d'internationalisation" configurer le clavier pour convenir à vos besoins. Si les saisis sont étranges et inattendues, le soucis provient surement de là.

changer le mot de passe



une fois ceci fait on peut redémarrer le Raspberry Pi pour attaque le reste

```
reboot
```

Choisir l'utilisateur "pi" avec le mot de passe que vous avez choisi ou celui par défaut "raspberry"



..petit texte de transition ?...

---

## La camera

::10 minutes::

En fonction de si on a une webcam USB ou un module caméra pour Raspberry Pi cette étape change légèrement. Mais l'idée reste la même, on va apprendre à prendre des photos avec l'interface en ligne de commande !

#### 	Installation des packets

Si on a une webcam USB il faut installer le packet `fswebcam` afin de pouvoir demander au Raspberry de prendre des photos avec.

```
sudo apt-get install fswebcam
```

La commande `raspistill` est préinstallé sur Raspbian et donc ne nécessite pas l'installation de packets supplémentaires.




#### 	Test de la caméra

Maintenant que tout est installé on va prendre des photos pour s'assurer que ça marche bien.

Pour le module caméra Raspberry Pi c'est :

```
raspistill -vf -hf -o photo.jpg
```

Et avec une webcam USB c'est :

```
fswebcam photo.jpg
```

Notre image que l'on a nomé photo.jpg se retrouve alors dans le repertoire `/home/pi/` (celui sur lequel on retourne quand on clique sur la petite maison).

#### 	Approfondissement des commandes

/vers/repertoire/photo.jpg : pour enregistrer la photo dans un repertoire précis

##### raspistill

description `-vf` et `-hf` : vertical flip et horizontal flip (sinon photo = à l'envers)



##### fswebcam

`--no-banner` : pour ne pas avoir la bannière en bas avec la date etc..

`-r 1280x720` : pour définir la résolution (doit être en adequation avec le format de la webcam sinon sa résolution sera modifiée au rapport de forme le plus proche)

exemple :
pour prendre une photo à la webcam et la sauvegarder sans la bannière dans *mondossier* il faut écrire :
```
fswebcam --nobanner /mondossier/photo.jpg
```
---

## L'imprimante

::20 minutes::



#### 	Installation des packets

téléchargement des packets généraux dont le paquet CUPS (Common UNIX Printing System) qui gère l'interface avec l'imprimante ainsi que quelques autres outils.

```
sudo apt-get update
sudo apt-get install git cups wiringpi build-essential libcups2-dev libcupsimage2-dev
```

téléchargement du filtre CUPS qui permet de traduire les images bitmap au format natif des imprimantes thermiques

```
cd
git clone https://github.com/adafruit/zj-58
cd zj-58
make
sudo ./install
```



#### 	Configuration de l'imprimante par defaut

-- savoir le baud rate de l'imprimante --

pour ajouter l'imprimante et la choisir par défaut :

```
sudo lpadmin -p ZJ-58 -E -v serial:/dev/ttyAMA0?baud=9600 -m zjiang/ZJ-58.ppd
sudo lpoptions -d ZJ-58
```

changer la valeur de "baud" en fonction l'imprimante, 9600 ou 19200.

Pour une imprimante USB, changer le nom de l'imprimante à `/dev/ttyUSB0`

Pour toute autres imprimantes utiliser le nom `/dev/ttyAMA0` ou `/dev/serial0`





#### 	Essais d'impression de texte et d'images

---

## Photomat'

#### 	Création d'un script

pour automatiser la prise de photo + impression

```
#!/bin/bash
# prendre photo
# imprimer ici.....
```

lui donner le nom qu'on veut (ex: `monscript.sh`)

modifier les droits pour le rendre executable

puis pour l'executer `./monscript.sh`

#### 	Amélioration du script

ajout de bouton et autres options, lancement du script au boot et finitions.



```
#!/bin/bash

BOUTON=16

# initialisation du GPIO
gpio -g mode  $BOUTON up

while :
do
	# si [le bouton est appuyé]; alors
	if [ $(gpio -g read $BOUTON) -eq 0 ]; then
    		#insérer premier script ici#
    fi
done
```





---







##### Configuration du démarrage automatique

Nous allons faire en sorte que le script de la camera démarre tout seul quand le Raspberry démarre.

```
sudo nano /etc/rc.local
```

Juste avant la dernière ligne "exit 0" écrire la ligne suivante :

```
sh /home/monscript.sh
```

Sauvegarder et quitter l'editeur de texte nano.
