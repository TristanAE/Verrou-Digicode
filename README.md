# Verrou-Digicode

Verrou par digicode activant un actionneur linéaire.


# I-	Conception électronique

a)	Le Motorshield

Pour ce projet, une carte Arduino avec un Motorshield sont utilisés. Le Motorshield lié à la carte est doté de plusieurs ponts en H lui permettant de prendre en charge 4 moteurs.

Le Motorshield utilisé vient donc s’emboiter sur la carte Arduino et les deux sont ensuite placés à l’intérieur d’une boite IPhone qui est attachée sur la porte à l’extérieur de la chambre.

![Image1](https://user-images.githubusercontent.com/92324336/139721845-fbbda448-a35c-4a8e-94ab-cb61105cec80.jpg)

 
b)	L’actionneur linéaire

L’actionneur linéaire va permettre de fermer la porte comme un verrou : celui-ci se trouve donc du coté intérieur de la chambre à l’opposé du boitier. L’actionneur étant connecté au Motorshield, un trou est fait dans le boitier pour que ses fils puissent sortir, passer par la fente de la serrure et arriver de l’autre côté.
Pour faire fonctionner l’actionneur linéaire, les 5V issus de l’Arduino ne suffise pas : il faut donc rajouter une batterie externe dans le compartiment external motor power du schéma ci-dessus.
La batterie utilisée est un support permettant de placer 4 piles 1.5V soit d’avoir 6V. La différence est faible mais la tension d’entrée de l’actionneur marquée 12V fonctionne aussi avec 6V pour le travail demandé. La batterie vient se coller sur la face du botier.
L’actionneur linéaire est connecté au premier emplacement des moteurs car on doit pouvoir contrôler son mouvement d’allongement et de rétractation.


c)	Le Keypad

Le Keypad utilisé est un 4×4 avec 8 pins à connecter à l’Arduino. Cependant avec la présence du Motorshield, plus que 6 pins analogiques sont disponibles et nous devons obligatoirement les souder pour les rajouter.

![Image2](https://user-images.githubusercontent.com/92324336/139721882-de53c350-70a3-4f73-bdaa-bba8e765d101.jpg)

 
Etant donné que nous ne pouvons utiliser que 6 pins sur 8 nous devons sacrifier une ligne et une colonne du Keypad qui ne pourront pas être connectées.
La ligne supprimée est celle des symboles et la colonne est celle des lettres.

![Image3](https://user-images.githubusercontent.com/92324336/139721977-a61fb674-9a8a-491a-9093-0ce1aa1e3ebe.png)

 

Selon le schéma ci-dessus nous pouvons supposer que R4 est la dernière ligne et C4 la dernière colonne.
Nous connectons donc les pins du Motorshield de la manière suivante :



A0	R1

A1	R2

A2	R3

A3	C1

A4	C2

A5	C3

Le Keypad vient se coller sur la face du boitier.

# II-	Conception informatique 

a)	Le Keypad

1)	Initialisation du Keypad

Pour pouvoir utiliser le Keypad, une bibliotèque #include <Keypad.h> est nécessaire. 

Nous devons ensuite déclarer deux variables pour le nombre de lignes et de colonnes utilisées sur le Keypad soit 3 dans notre cas mais 4 en temps normal.

Nous mettons ensuite en place un tableau donnant les valeurs qui seront lues quand on cliquera sur le Keypad. Puisque nous avons supprimé une ligne et une colonne, il nous reste :

char hexaKeys[ROWS][COLS] = {
  {'1', '2', '3'},
  {'4', '5', '6'},
  {'7', '8', '9'}
};

Dans le loop:

La carte est en attente d’un clic sur le Keypad avant de lancer les instructions.

2)	La bibliothèque Password

Pour pouvoir mettre en place un mot de passe, il est possible de leur faire sans bibliotèque ajoutée mais nous utiliserons #include <Password.h> pour être plus efficace.

On indique au programme quel est le mot de passe choisi.

Pour tester si le code est le bon, nous utiliserons un switch :

Si le bouton est 1 alors on vérifie si le mot de passe enregistré est le bon.
Si le bouton est autre chose, on ajoute le nombre appuyé dans le password. A chaque clic sur le Keypad, les nombres s’ajoutent au password. 

Une fois celui-ci terminé d’être écrit, en appuyant sur 1, le mot de passe est vérifié.

b)	L’actionneur linéaire

Pour utiliser ce motorshield, la bibliotèque #include <AFMotor.h >est nécessaire :

Les pins de la vitesse et de la direction de l’emplacement 1 des moteurs sont  respectivement 5 et 0.

On met dès le départ la vitesse du moteur 1 au maximum et on déploie l’actionneur linéaire pour fermer.
Si le mot de passe est correct alors on rétracte l’actionneur

Remarque :
Un unique emplacement de moteur nous sera utile pour déployer un actionneur linéaire.
Il aurait été beaucoup plus judicieux d’utiliser un Motorshield à un seul emplacement de moteur pour gagner de la place et économiser un module. Ce choix n’a pas été fait en raison du manque de connaissances au moment de la réalisation du projet.
Le système de la Nodemcu + Motorshield, plus petit avec Wifi et seulement 2 moteurs n’a pas fonctionné car la fonction utilisée plus tard waitforkey obligeait la carte à se réinitialiser en permanence du fait du manque d’activité.

![Image4 (1)](https://user-images.githubusercontent.com/92324336/139722311-309ce73f-6b7e-4022-828f-737cbf0f8b2b.gif)

 
