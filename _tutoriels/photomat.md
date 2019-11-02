---
titre: Fabriquer un photomat avec un Raspberry Pi
temps: 1 heure
difficulté: intermédiaire
age: 14 ans +
---

Le Raspberry Pi est un micro ordinateur qui coûte pas cher et qui est facile à utiliser pour apprendre à programmer et créer des projets d'électronique.

Dans ce tuto nous allons apprendre a installer et démarrer un Raspberry Pi pour ensuite lui brancher une camera, une imprimante et un bouton pour le transformer en photomaton.


## Liste du matériel

### Pour le photomat

- Un Raspberry Pi
  - Avec un alimentation 5 V et 2 à 3 A
  - Une Carte microSD (2Go minimum)
- Une Raspberry Pi Cam ou webcam
- Une Imprimante thermique
  - Les câbles ou fils nécessaire pour l'alimenter et communiquer avec (dépend du modèle)
  - Des rouleaux de papier thermique
- Un bouton poussoir

### Pour l'installation

- Un ordinateur avec une connexion internet
- Un lecteur de carte SD et un adaptateur microSD vers SD
- Un écran et un câble HDMI
- Un clavier et une souris
- Un câble ethernet ou un accès WiFi (pour le Raspberry Pi)
- Des câbles jumper ou du fil électrique


## Téléchargement du système d'exploitation

~ Il faut commencer par préparer le Raspberry Pi car il est vide et pas configuré. Sans système de logique (ou d'exploitation) la machine n'est qu'un tas de composants avec lesquelles il est difficile pour nous d'interagir.

Télécharger le système d'exploitation [Raspbian](https://www.raspberrypi.org/downloads/raspbian/) et un logiciel pour nous permettre de la graver sur notre carte microSD comme [Etcher](https://www.balena.io/etcher/) sur votre ordinateur.

Insérer la carte microSD dans votre ordinateur et utiliser Etcher (ou un autre logiciel) pour graver Raspbian dessus. La carte microSD devrait être détectée automatiquement (mais vérifier quand même), il suffit juste de lui indiquer où se trouve le fichier `.zip` de Raspbian téléchargé.

![gif etcher flashage carte microSD]()


## Démarrage et configuration du Raspberry Pi

Config...

Langue, timezone...

![image config demarrage raspberry pi]()

```
raspi-config
```

optionnel mais recommandé :

sous "options d'internationalisation" configurer le clavier pour convenir à vos besoins. Si les saisis sont étranges et inattendues, le soucis provient sûrement de là.

changer le mot de passe

une fois ceci fait on peut redémarrer le Raspberry Pi pour attaque le reste

```
reboot
```

Choisir l'utilisateur "pi" et entrer le mot de passe que vous avez choisi. Si besoin, celui par défaut est "raspberry".

[//]: # texte de transition ?

## Installation de la caméra

![image branchement module camera et webcam]()

En fonction de si on a une webcam USB ou un module caméra pour Raspberry Pi cette étape change légèrement. Mais l'idée reste la même, on va apprendre à prendre des photos avec l'interface en ligne de commande.

Si on a une webcam USB il faut installer le packet `fswebcam` afin de pouvoir demander au Raspberry de prendre des photos avec.

```
sudo apt-get install fswebcam
```

Pour le module caméra Raspberry Pi la commande `raspistill` est préinstallé sur Raspbian et donc ne nécessite pas l'installation de packets supplémentaires.


## Test de la caméra

Maintenant que tout est installé on va prendre des photos pour s'assurer que ça marche bien.

Pour le module caméra Raspberry Pi c'est :

```
raspistill -vf -hf -o photo.jpg
```

Et avec une webcam USB c'est :

```
fswebcam photo.jpg
```

![image exemple de photo]()

Notre image que l'on a nomé photo.jpg se retrouve alors dans le repertoire `/home/pi/` (celui sur lequel on retourne quand on clique sur la petite maison).

![image photo dans dossier pi]()


## Approfondissement des commandes

`/vers/repertoire/photo.jpg` : pour enregistrer la photo dans un repertoire précis


### raspistill

description `-vf` et `-hf` : vertical flip et horizontal flip (sinon photo = à l'envers)


### fswebcam

`--no-banner` : pour ne pas avoir la bannière en bas avec la date etc..

`-r 1280x720` : pour définir la résolution (doit être en adéquation avec le format de la webcam sinon sa résolution sera modifiée au rapport de forme le plus proche)

exemple :
pour prendre une photo à la webcam et la sauvegarder sans la bannière dans *mondossier* il faut écrire :
```
fswebcam --nobanner /mondossier/photo.jpg
```


## Installation de l'imprimante

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

![image branchement imprimante]()

-- savoir le baud rate de l'imprimante --

pour ajouter l'imprimante et la choisir par défaut :

```
sudo lpadmin -p ZJ-58 -E -v serial:/dev/ttyAMA0?baud=9600 -m zjiang/ZJ-58.ppd
sudo lpoptions -d ZJ-58
```

changer la valeur de "baud" en fonction l'imprimante, 9600 ou 19200.

Pour une imprimante USB, changer le nom de l'imprimante à `/dev/ttyUSB0`

Pour toute autres imprimantes utiliser le nom `/dev/ttyAMA0` ou `/dev/serial0`


## Test de l'imprimante

[//]: # insérer tests ici

![image test impression]()


## Le photomat (scripts)

Maintenant que la camera et l'imprimante fonctionnent on va rassembler les deux pour créer notre photomat.

création d'un script pour automatiser la prise de photo + impression

```
#!/bin/bash
# prendre photo
# imprimer ici.....
```

lui donner le nom qu'on veut (ex: `monscript.sh`)

![image création de script]()

modifier les droits pour le rendre executable (cli + gui)

![image fichier executable]()

puis pour l'executer pour voir si ça marche :

```
./monscript.sh
```

maintenant ajout de bouton et autres options, lancement du script au boot et finitions.

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
*Ce qui est écrit après un `#` n'est pas exécuté par l'ordinateur, on appelle ça un commentaire*

Il faut faire en sorte que le script de la camera démarre tout seul quand le Raspberry démarre.

```
sudo nano /etc/rc.local
```

Juste avant la dernière ligne "exit 0" écrire la ligne suivante :

```
sh /home/monscript.sh
```

Sauvegarder et quitter l'editeur de texte nano.


fin

on peut le débrancher de l'ecran et fabriquer une boite pour contenir notre photomat.

![photo exemple de montage photomat]
