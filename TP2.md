# 1.2.	TP2 : Utilisation de l’interface parallèle en mode en-trée 
  Dans ce TP on va connecter un clavier matriciel au microcontrôleur PIC16F877. Un cla-vier est tout simplement un tableau de boutons poussoirs connectés en rangées et colonnes, de sorte que chacun peut être testé pour la fermeture avec le nombre minimum de con-nexions (Figure 7.5). Il y a 12 touches sur un clavier de téléphone de type (0-9, #, *), dispo-sées dans une matrice 3x4. Les colonnes sont étiquetées 1, 2, 3 et les lignes A, B, C, D. Si nous supposons que toutes les lignes et les colonnes sont initialement hautes, une frappe de touche peut être détectée en mettant chaque rangée à tour de rôle et en vérifiant chaque colonne pour Un zéro.
  Dans le circuit clavier de la figure 7.6, les 7 broches du clavier sont connectées au port D. Les bits 4-7 sont initialisés en tant que sorties et les bits 0-2 utilisés comme entrées. Ces broches d'entrée sont tirées vers le haut vers la logique 1. Les lignes de sortie sont égale-ment initialement réglées sur 1. Si un 0 est maintenant sorti sur la ligne A, il n'y a aucun effet sur les entrées à moins qu'un bouton dans la rangée A soit pressé. Si elles sont véri-fiées à tour de rôle pour un 0, un bouton de cette ligne qui est pressé peut-être identifié comme une combinaison spécifique de bits de sortie et d'entrée.
Une façon simple d'obtenir ce résultat est d'incrémenter un nombre de touches testées lors-que chacune est cochée, de sorte que lorsqu'un bouton est détecté, le balayage du clavier est terminé avec le numéro de clé courant dans le compteur. Cela fonctionne parce que les nombres (non-zéro) sur le clavier, disposés dans l'ordre :
Ligne A= 1, 2, 3
Ligne B= 4, 5, 6
Ligne C= 7, 8, 9
Ligne D= *, 0, #
Suivant ce système, le symbole étoile est représenté par un comptage de 10 (0Ah), de zéro par 11 (0Bh) et de hachage par 12 (0C).

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/caa88d36-8e9b-4037-a438-6b53f4f4cd66)

                                              Fig. 7.5.

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/bb694e99-7422-4be9-a67e-454ab5f2b622)

                                              Fig. 7.6.

   L'opération de lecture du clavier (TP2) parcourt les boutons de cette manière, incrémen-tant un comptage de touches, et quitte la routine de balayage lorsqu'un bouton est détecté, avec le nombre de touches correspondant enregistré. Si aucun bouton n'est pressé, il se répète. Le programme affiche alors le numéro de bouton sur un affichage à 7 segments, avec des symboles arbitraires représentant l'étoile et le hachage.
Affichage 7 segments à LED 
  L'affichage 7 segments à LED standard dans l'application du clavier se compose de seg-ments lumineux agencés pour montrer des symboles numériques lorsqu'ils sont allumés dans la combinaison appropriée. Chaque segment est entraîné séparément du port C par l'intermédiaire d'une résistance de limitation de courant. Les numéros 0 à 9 peuvent être affichés, mais pour une gamme complète de caractères alphanumériques, d'autres seg-ments (par exemple LED étoile) ou un affichage matriciel sont plus polyvalents. Les codes 0-9, * et # sont indiqués dans le tableau 7.1.

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/492a2b16-475d-4482-b3f3-c77e70b2208d)

                                              Tableau 7.1.

  Les segments sont étiquetés a-g et sont supposés fonctionner en haut actif (1 = ON). Le code binaire requis doit alors être calculé pour chaque caractère à afficher, en fonction de l'ordre dans lequel les sorties sont connectées aux segments. Dans ce cas, le bit 1 = a, pas-sant au bit 7 = g, avec le bit 0 non utilisé.
Le hachage est affiché en tant que 'H' et l'étoile en trois barres horizontales (S est déjà utili-sé pour 5). Comme seulement 7 bits sont nécessaires, le LSB est supposé être 0 lors de la conversion en hexadécimal. Dans tous les cas, il est préférable de placer le code binaire dans le programme.
Les codes pour d'autres types d'affichage peuvent être calculés de la même manière.
  La sortie PIC fournit suffisamment de courant pour conduire une LED (contrairement aux sorties logiques standard), mais pour un élément d'affichage nécessitant plus de 20 mA pour fonctionner, un entraînement de courant supplémentaire doit être ajouté au matériel, habituellement sous la forme d'un transistor bipolaire. Cette interface sera traitée plus tard. Une alternative à l'affichage normal à 7 segments est un module décimal codé binaire (BCD) ; Ceci reçoit l'entrée BCD et affiche le numéro correspondant. En BCD : 0 = 00002, 1 = 00012 et ainsi de suite à 9 = 10012, et donc ne nécessite que 4 entrées (plus une borne commune).
  Ces affichages sont habituellement fournis avec une borne commune, connectée aux anodes ou cathodes des LED. Un affichage haut actif aura une cathode commune et des anodes individuelles, un type actif bas (ON = 0) aura une anode commune.
1.	Dans Proteus, réaliser le montage de la figure 7.6.
2.	Comme a été montré dans le TP1. Éditer et compiler le code en assembleur TP2.asm
3.	Double clic sur le PIC16F877 pour charger le fichier Project_2.X.production.hex , pour mon cas : C:/Users/Lyes/MPLABXProjects/Project_2.X/dist/default/production/Project_1.X.production.hex
4.	Appuyer sur tous les boutons du clavier et vérifier que chaque chiffre appuyé sera affiché.
5.	Modifier le code de telle sorte l’afficheur sera éteint à chaque fois le bouton soit relâché. 
6.	Modifier le code et le circuit pour que le PORTB va afficher le double du chiffre appuyé. 

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/ff346e73-310c-42b5-bbd5-302b532073e1)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/bae2264e-5856-4c21-9899-2c2c4bd1fdd3)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/43361fec-1910-4b91-8ddf-48732e41876a)






