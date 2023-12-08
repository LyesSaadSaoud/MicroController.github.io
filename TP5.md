# 1.5.	TP5 : Affichage alphanumérique et programmation d’un afficheur LCD

L’afficheur LCD (Liquid Crystal Display) est maintenant un choix très courant pour les affichages graphiques et alphanumériques. Ceux-ci vont de petits types numériques mono-chromes à 7 segments tels que ceux utilisés dans les multimètres numériques (typiquement 3 ½ chiffres, lecture maximale 1.999) à de grands écrans couleur et haute résolution qui peuvent afficher une vidéo complète. Ici, nous nous concentrerons sur le petit type alpha-numérique monochrome qui affiche des caractères alphabétiques, numériques et symbo-liques à partir du jeu de caractères ASCII standard. Ce type peut également afficher des graphiques à basse résolution, mais nous nous en tiendrons à des exemples simples. Un circuit est illustré à la figure 7.9. L'affichage est un LM016L standard qui affiche 2 lignes de 16 caractères (16 x 2). Chaque caractère est de 5 à 8 pixels, ce qui en fait 80 à 16 pixels dans l'ensemble. Dans le programme de démonstration, un message fixe est affiché sur la ligne 1, affichant une chaine de caractère « I.Bio M1(S2). La deuxième ligne se termine par un caractère qui compte de 0 à 9 et se répète, pour montrer un affichage variable. L'écran reçoit des codes ASCII pour chaque caractère aux entrées de données (D0-D7). Les don-nées sont présentées aux entrées d'affichage par le MCU, et verrouillées par impulsion de l'entrée E (Enable). La ligne RW (lecture / écriture) peut être associée à un niveau faible (mode écriture), car l'écran LCD ne reçoit que des données.
L'entrée RS (Register Select) permet d'envoyer des commandes à l'écran. RS = 0 sélectionne le mode de commande, le mode de données RS = 1. L'écran lui-même contient un contrô-leur ; la puce standard dans ce type d'affichage est le Hitachi HD44780. Il doit être initiali-sé en fonction des données et des options d'affichage requises.
Dans cet exemple, les données sont envoyées en mode 4 bits. Le code à 8 bits pour chaque caractère ASCII est envoyé en deux moitiés ; la partie haute de quarte bits (nibble haut), puis la partie basse de quarte bits (nibble bas). Cela permet d'économiser les broches d'E / S et permet à l'écran LCD d'être piloté en utilisant seulement 6 lignes d'un seul port, tout en rendant le logiciel un peu plus complexe.
Le jeu de commandes du contrôleur d'affichage est indiqué dans le Tableau 7.2. Si nous nous tournons maintenant vers le programme de démonstration, nous pouvons interpréter les commandes envoyées lors de la séquence d'initialisation d'affichage, en comparant les codes hexadécimaux aux commandes binaires. On peut voir que l'affichage doit initiale-ment être réglé sur le mode de fonctionnement par défaut, avant de sélectionner le mode requis (4 bits, 2 lignes) et de réinitialisation. Notez que les commandes sont différenciées par le nombre de zéros en tête (Tableau 7.3).
Nous allons analyser le programme TP5.asm en détail car il contient un certain nombre de fonctionnalités que nous verrons à nouveau. 

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/b396820b-7c34-483d-98a4-3b8b73725bb9)

                                               Fig. 7.9.

Le programme comporte trois processus principaux :
• Message fixe de la ligne de sortie 1 "  I.Bio. M1(S2) "
• Ligne de sortie 2 message fixe 'VARIABLE : '
• Nombre de variables de sortie 0-9 à la ligne 2, position 12
Le programme principal (dernier dans la liste des codes sources) est très court, comprenant :
• Initialiser le microcontrôleur et l'écran LCD
• Sortie de messages fixes
• Nombre de sorties

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/da3f67e4-3f8a-466a-9cc7-724d92fbfd2b)

                                              Tableau 7.2.

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/6620d495-7610-447c-b5f0-0af140b0ba9b)

                                              Tableau 7.3.

Afin de faciliter cela, le pseudocode est donné comme suit :
Projet d’affichage sur LCD
            Programme pour démontrer la sortie fixe et variable de carac-tères alphanumériques à 16x2 LCD (simulation uniquement)
MATÉRIEL
            Fichier de simulation ISIS TP5.DSN
            CPU          16F877A
                   Horloge = XT 4MHz
            LCD          Données = RD4 - RD7
                   RS = RD1
                   E = RD2
                   RW = 0
Procédure
            Initialiser
                   Sortie LCD = Port D
                   Attendre 100 ms pour que l'écran LCD démarre
                   LCD: données 4 bits, 2 lignes, curseur automatique
                   Réinitialiser LCD
Affichage de la ligne de message 1
                   Réinitialiser le pointeur de table
                   RÉPÉTER
                   Obtenir le code suivant
                   Envoyer un code ASCII
                   Jusqu'à 16 caractères terminés
Affichage de la ligne de message 2
                   Positionner le curseur
                   Réinitialiser le pointeur de table
                   RÉPÉTER
                    Obtenir le code suivant
                    Envoyer un code ASCII
                    Jusqu'à 11 caractères
Comptage d'incrémentation d'affichage
                   RÉPÉTER
                     Nombre de sets = 0
                        BOUCLE
                            Calculer ASCII
                            Envoyer un code ASCII
                            Comptage d'incrément
                            Réinitialiser le curseur
                            Delay 250ms
                            Jusqu'à Count = 9
                   TOUJOURS
Envoyer un code ASCII
                   Masque à faible morsure
                   Sortie grignotage élevé
                   Impulsion
                   Attendre 1 ms
                   Swap nibbles
                   Sortie faible grignotage
                   Impulsion
                   Attendre 1 ms
                   Revenir

Le programme est divisé en blocs fonctionnels en conséquence. Notez que les étiquettes de registre standard sont définies en incluant le fichier standard P16F877.INC, qui contient une liste de toutes les étiquettes pour les SFR et les bits de contrôle, par exemple, PORTD, STATUS, Z. Cela est plus commode que de les déclarer dans les fichiers et chaque inclusion fournit par Microchip définit un ensemble standard d'étiquettes que tous les program-meurs peuvent utiliser.
Pour envoyer les données et les commandes à l'afficheur, les données de sortie sont initia-lement masquées de sorte que seul le nibble élevé est envoyé. Les bits bas sont effacés. Ce-pendant, étant donné que les bits bas contrôlent l'affichage (RS et E), ceux-ci doivent être configurés après que les données ont été sorties dans les bits hauts de port. En particulier, un bit d'indicateur RS est mis en place dans un registre factice 'Select' pour indiquer si la sortie courante est une commande ou des données, et copié à RD1 après la configuration des données.
Après chaque sortie, une temporisation de 1 ms est exécutée pour permettre au contrôleur ACL de traiter l'entrée et de l'afficher. Une boucle de synchronisation exacte (Onems) est réalisée en remplissant la boucle de retard à 4 cycles avec un NOP et en l'exécutant 249 fois. Avec les instructions supplémentaires et les sauts de sous-programme, le délai est exactement 250x4= 1000 μs. Ceci est ensuite utilisé par une autre boucle (Xms) pour obte-nir des retards en millisecondes entières. Il est également utilisé pour générer une impul-sion de 1 ms en E pour verrouiller les données et les commandes dans le port d'entrée du contrôleur LCD.
Les messages fixes sont générés à partir de tables de données de programme utilisant ADDWF PCL pour sauter dans la table en utilisant le nombre en W et RETLW pour re-tourner les codes ASCII. L'assembleur génère le code ASCII en réponse aux guillemets simples qui entourent le caractère. Pour récupérer le code requis, le pointeur de table est ajouté au compteur de programme en haut de la table. Le pointeur de table est également vérifié à chaque fois pour voir la fin de la table a été atteinte.
La variable de sortie est juste un comptage de 0 à 9, mais pour obtenir le code ASCII cor-respondant, 30h doit être ajouté, car l'ASCII pour 0 est 30h, pour 1 est 31h et ainsi de suite. Le tableau de codes ASCII est présenté dans le tableau 7.4. Un délai de 250 ms est exécuté entre chaque sortie pour rendre le compte visible.
On note que j’ai utilisé deux méthodes pour déclarer les tableaux : 1) pour la ligne 1, on utilise la directive dt    

Ligne1  ADDWF	PCL		; Modifier le compteur de programme
	dt   "  I.Bio. M1(S2) ",0
Cette déclaration est équivalente à :
Ligne1  ADDWF	PCL		; Modifier le compteur de programme
          retlw     ‘ ’ 
          retlw     ‘ ’
          retlw     ‘I’   
          retlw     ‘.’ 
          retlw     ‘B’ 
          retlw     ‘i’ 
          retlw     ‘o’ 
          retlw     ‘.’ 
          retlw     ‘ ’ 
          retlw     ‘M’ 
          retlw     ‘1’ 
          retlw     ‘(’ 
          retlw     ‘S’ 
          retlw     ‘2’ 
          retlw     ‘)’ 
          retlw     ‘ ’ 

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/ec602485-2929-4c84-8968-5797aab460ef)

                                              Tableau 7.3.

Le premier ensemble de caractères se compose de lettres majuscules et minuscules, les nombres et la plupart des autres caractères trouvés sur un clavier standard. Il s'agit de 95 caractères, représentés par des codes de 3210 à 12610 inclus. Avec un code à 8 bits, 256 caractères sont possibles, donc les autres sont utilisés pour des jeux de caractères spéciaux, par exemple des caractères accentués, grec, arabe ou chinois. Le jeu de caractères peut être programmé dans la RAM d'affichage comme requis, ou commandé comme une mise en œuvre ROM. Vous pouvez obtenir d'autres détails à partir de la fiche de données d'affi-chage (datasheet).

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/7a3578e5-c04e-4271-9756-f6cadafdb20c)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/85dee94c-cfe3-40ab-9590-a2db2ac24c57)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/7440807b-fcef-44b5-82ee-2b1e0849a12d)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/ff186aa4-9c7a-47ed-8ce2-0d7777f25c97)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/31e0339b-6e95-401f-993a-cea953840a71)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/f2c66224-9c60-4a03-a96a-e0431999f936)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/9d4a62ca-6ab0-47cd-8726-812a2e0f62b2)

1.	Réaliser le circuit de la figure 7.9, en utilisant le logiciel Proteus.
2.	Comme a été montré dans les TPs précédant. Éditer et compiler le code en assem-bleur TP5.asm.
3.	Double clic sur le PIC16F877 pour charger le fichier Project_5.X.production.hex , pour mon cas : C:/Users/Lyes/MPLABXProjects/Project_5.X/dist/default/production/Project_5.X.production.hex
4.	Exécuter le circuit dans Proteus, le message I.Bio. M1(S2) sera affiché sur la première ligne et Variable : 0 à 9 sur la ligne 2.
5.	Modifier le code pour afficher votre « nom » sur la première ligne et pour la deu-xième ligne : 1 .  « Var. Hex. : 0 à F », puis 2. Var. : 00 à 99.
6.	 Modifier le code et le circuit pour ajouter un bouton poussoir. L’affichage aura le même principe saut le comptage sera incrémenté en utilisant le bouton-poussoir (astuce : utiliser une temporisation pour enlever les rebondissement).













