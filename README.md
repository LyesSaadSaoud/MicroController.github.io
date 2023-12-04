![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/00061c74-435c-4c19-9cd1-a23c64c0377a)# MicroController.github.io
# 1.1. TP1 : Prise en main de l’environnement de programmation du microcontrôleur
#  1.1.1. Objectif
  L'exercice du TP1 montre comment créer, éditer et construire des projets avec MPLAB® X
IDE. Ce laboratoire est une étape par étape à travers le développement du projet MPLAB X.
Un projet MPLAB X est créé et un fichier source C existant est ensuite ajouté au projet. Le
laboratoire continue avec l&#39;édition du fichier source et la construction réussie d&#39;un projet. Le
laboratoire comprend des exercices pour démontrer certaines fonctionnalités utiles de
manipulation de fichiers et de données de MPLAB X IDE
# 1.1.2. Création d&#39;un projet MPLAB X
## 1.1. Installation du logiciel MPLAB X
 • Télécharger MPLAB X depuis le site officiel :
     http://www.microchip.com/mplab/mplab-x-ide
 • Exécuter l’icône d’installation
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/ccd810b5-01f3-4e0e-92a7-3be0456e6a70)
Allez à l'emplacement où vous avez téléchargé le programme d'installation. Décom-pressez le fichier téléchargé et exécutez le programme d'installation :
MPLABX-vX.XX-windows-installer.exe.
 
Selon vos paramètres de sécurité Windows, vous pouvez obtenir une fenêtre demandant si vous êtes sûr que vous voulez exécuter ce programme. Répondez «Oui».
 •	Installer
Cliquez sur Suivant>.
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/7c778f7a-ac72-43ec-a549-01e5ee637065)
 •	Accord de licence
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/9568706d-d1b6-404a-a192-b15efbd9d1b1)
Cliquez sur le bouton radio en regard de "J'accepte l'accord".
Cliquez sur Suivant>.
 •	Répertoire d'installation
 Par défaut, MPLAB X IDE sera installé à C:\ Program Files (x86) \ Microchip \ MPLABX. Si vous préférez un répertoire différent, cliquez sur l'icône de dossier à droite de la zone de texte et sélectionnez l'emplacement d'installation souhaité.
Cliquez sur Suivant>.
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/d48e44e4-cfd4-4412-acb0-943127f62c19)
 •	Sélectionner des programmes
Si vous souhaitez installer MPLAB X IDE (environnement de développement intégré) ou MPLAB IPE (environnement de programmation intégré), cochez / décochez les cases ap-propriées. Généralement, vous devez installer les deux programmes.
Cliquez sur Suivant>.
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/61f32835-b3ed-4c73-917d-4134de960c33)
 •	Prêt à installer
Cliquez sur Suivant>.
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/0e9c70e2-cf61-487a-b18f-a446b81cd940)
 •	Installation
Attendez que le programme d'installation ait terminé l'installation de tous les composants de l'IDE.
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/2c821e55-2a04-439e-88af-b44133befe11)
 •	Permettre l'installation d'un pilote de périphérique
Les pilotes de périphériques USB doivent communiquer avec les outils de développement matériel de Microchip.
Cochez la case à côté de «Toujours faire confiance au logiciel de Microchip Technology» pour empêcher cette boîte de dialogue à l'avenir.
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/ff7af2cc-3f02-414c-8e74-8862b6d21b34)
 •	Achevée
Laissez la case cochée si vous souhaitez que votre navigateur Web soit ouvert sur la page de téléchargement du compilateur MPLAB XC de Microchip pour télécharger un compila-teur à utiliser avec MPLAB X IDE.
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/840030cd-f864-484f-803a-c863acb52e5f)
Décochez la case si vous avez déjà un compilateur ou si vous souhaitez en télécharger un plus tard.
Cliquez sur Terminer.
L'installation est terminée. Il devrait y avoir des icônes pour MPLAB X IDE, MPLAB IPE et le commutateur de pilote MPLAB sur le bureau. Il y aura également des lanceurs dans le menu Windows / Démarrer sous Tous les programmes ▶ Microchip ▶ MPLAB X IDE.
#1.1.3.	Notre premier projet 
 •	Création de projet
Ouvrir MPLAB X IDE
Fermez tous les projets ouverts dans MPLAB X IDE en cliquant avec le bouton droit de la souris sur le nom du projet et en sélectionnant Fermer ou en allant dans Fichier et en sélec-tionnant Fermer tous les projets.
Lorsque MPLAB® X démarre, il ouvre le dernier projet travaillé. Pour éviter toute confusion, ce laboratoire vous demande de fermer tout projet ouvert
 •	Début de la création du projet
Cliquez sur l'icône Nouveau projet  pour lancer le processus de création du projet
 •	Nouvelle fenêtre de projet
Sélectionnez « Microchip Embedded » (“Microchip Embedded”) puis « Standalone Pro-ject» (“Standalone Project”.).
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/106ae8a1-55ed-43bb-8ca7-21e95e9c8926)
 •	Sélection du processeur
Sélectionnez «Mid-range 8-bits (PIC10/12/16/MCP)» dans le menu déroulant «Famille», puis sélectionnez «PIC16F877» dans le menu «Périphérique».
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/2fe94e2b-5971-4cd0-a51a-e6fbd732325d)
Nous utilisons le PIC16F877 comme microcontrôleur pour ce TP.
Cliquez sur Next   
 •	Sélection du matériel
Sélectionnez "Simulateur" sous Outils matériels lorsque vous êtes invité à sélectionner un outil.
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/571c41da-0276-4a39-b564-a3dd1fc491e9)
Cliquez sur Next  
 •	Sélection du compilateur
Sélectionnez une version installée du compilateur mpasm(v5.xx)
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/82f47220-4c40-46ed-8fb0-caad214798f2)
Cliquez sur Next  
 •	Nom du projet et sélection du dossier
Cliquez sur le bouton Parcourir et accédez au dossier que vous désirez, ou bien laisser le dossier par défaut.
Sur la ligne Projet location, taper le nom du projet (par exemple : Projet_1).
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/9643ffb6-6614-4528-b613-cb04480cce0d)
Remarque MPLAB® X remplissant la ligne du dossier de projet ;
Cliquez sur Finish  
Toutes nos félicitations ! Vous venez de créer un projet MPLAB® X
Nous allons maintenant ajouter des fichiers de code source au projet.
 •	Éditer le fichier source 
Appuyez en utilisant le bouton doit sur « source files », -> New -> pic_8b_simple.asm.
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/8c11d581-40a3-4669-9d5a-82d11a1cb203)
Dans la fenêtre New pic_8b_simple.asm, taper le nom de fichier « TP1 »
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/5be5d3ca-c6ab-4dc4-86a6-b086dfbfef68)
 •	Ajout de fichiers au projet
Cliquez avec le bouton droit sur le dossier Fichiers source dans la fenêtre du projet
Sélectionnez Ajouter un élément existant
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/393d07b2-decc-48e0-a5df-4df8cafcf290)
 •	Ouvrir l'éditeur
Double-cliquez sur le fichier TP1.asm pour ouvrir l'éditeur. Effacer ce qui existe et taper le code suivant :
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/20cea1e1-e9b2-40cc-9b11-749c923bd873)
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/7a5243cf-f711-40f7-a7df-19b78a10b981)
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/73802601-ad3d-403f-b75b-47eeb5e190ad)
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/026cb387-b7ce-44bb-ad6f-d7bcaa95de73)
C'est un programme très simple conçu pour renforcer la confiance dans votre capacité à construire un projet et à l'exécuter en MPLAB® X.
 •	Construire le projet 
Cliquez sur l'icône Nettoyer et construire pour créer le projet
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/07a680b4-c5ee-4749-a9e3-3b67059a0bbc)
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/89c9a252-a23f-4c74-a188-a5ec87153f75)
#1.1.4.	Réalisation du circuit électronique sur Proteus 
Comme on peut le voir, ISIS permet également de présenter facilement des diagrammes de circuit à un standard professionnel.
Une capture d'écran de la capture schématique ISIS et de l'environnement de simulation interactif est illustrée à la Figure 7.1. La fenêtre d'édition du schéma principal est accom-pagnée d'une fenêtre d'aperçu montrant le dessin entier et une fenêtre de sélection d'objet, qui contient normalement une liste de composants. Toutefois, il affiche également des listes d'autres périphériques disponibles pour une utilisation dans la fenêtre d'édition lorsque des modes spécifiques sont sélectionnés.
La fenêtre d'édition principale comprend un contour de la feuille, qui montre le bord de la zone de dessin, dans lequel les composants doivent être placés. Le bouton Composant est normalement sélectionné par défaut dans la barre d'outils Mode. Avec ce mode sélection-né, les composants sont récupérés pour être placés sur le schéma en appuyant sur le bou-ton P (pick devices) et en sélectionnant la catégorie de composants requise. Le type d'appa-reil individuel peut alors être choisi dans une liste (Figure 7.2).
Les composants sont catégorisés comme des microprocesseurs (comprend des microcontrô-leurs), des résistances, des condensateurs, etc. avec des variantes dans chacun d'eux. Les sous-catégories peuvent être sélectionnées. Les composants interactifs, tels que les boutons poussoirs sont regroupés dans la bibliothèque ACTIVE.
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/72224018-b628-492f-83db-39531e370ae4)
                                                     Fig. 7.1.
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/e241d8c3-f7f4-4614-8106-fb45f1e69032)
                                                     Fig. 7.2.
Un circuit pour démontrer le fonctionnement du programme TP1 est représenté à la figure 7.3. 
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/10aea7e6-2d49-4b39-86be-fc80604b9374)
                                                     Fig. 7.3.
Le microcontrôleur est un PIC 16F877, notre dispositif de référence ; D'autres puces PIC seront décrites plus loin. Une diode électroluminescente LED est connecté au port B (RB0), avec un bouton poussoir sur RD0. Il faut noter que pour les simulations, le circuit d'hor-loge externe ne contrôle pas la fréquence de fonctionnement du PIC. Ceci doit être défini dans la boîte de dialogue des propriétés pour le composant MCU (voir ci-dessous). De même, l'entrée MCLR (Master Clear) ne doit pas être connectée pour que le programme s'exécute en mode simulation, alors que cela est essentiel dans le circuit réel.
Le port n'a pas besoin d'être initialisé pour l'entrée, car c'est également la condition par défaut. D'autre part, les sorties doivent être initialisées par le programme MCU en char-geant le registre de direction de données avec des zéros. Les sorties PIC peuvent générale-ment fournir jusqu'à 25 mA, ce qui est suffisant pour allumer les LED sans aucun pilote supplémentaire. 100 Ω limite le courant dans la LED à environ 20 mA.
Lors de la création d'une nouvelle application, un dossier approprié doit être prévu pour contenir les fichiers de projet, car le fichier de conception du schéma sera accompagné par plusieurs fichiers associés au programme joint (code source, code hexadécimal, fichier de liste, etc.).
1.	Réaliser le circuit de la figure 7.3, en utilisant le logiciel Proteus.
2.	Double clic sur le PIC16F877, la fenêtre de la figure 7.4 doit s’apparaitre.
3.	Appuyer sur l’icône « Program file » et charger le fichier Pro-ject_1.X.production.hex , pour mon cas : C:/Users/Lyes/MPLABXProjects/Project_1.X/dist/default/production/Project_1.X.production.hex
4.	Appuyer sur le bouton poussoir, la LED doit s’allumer. 
5.	Modifier le code et le circuit pour que une autre LED en plus reliée à la pin RB1 soit éteint quand la première LED est allumée et vis-versa.   
6.	Modifier le code et le circuit pour que 7 autres LEDs en plus reliées au PORTB. Les LEDs seront allumées 4 par 4 en alternance (paire et impaire).   
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/355a5e6f-9a90-47db-bcca-b5c2276057a7)
                                                     Fig. 7.4






























