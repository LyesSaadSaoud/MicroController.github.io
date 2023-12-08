# 1.7.	TP7 : Signal modulé en largeur d’impulsion (PWM) 
Dans ce TP, nous nous concentrerons sur les sorties de puissance. Le microcontrôleur ou le port du microprocesseur ne fournit qu'une quantité limitée de courant : environ 20 mA dans le cas de PIC, et encore moins pour des ports de microprocesseur standard. Par con-séquent, si nous voulons conduire un périphérique de sortie qui a besoin de plus de cou-rant que cela, un certain type d'amplificateur ou de commutateur de courant est néces-saire.
Le circuit de ce TP peut être utilisé comme variateur de vitesse des moteurs à courant con-tenu et comme un gradateur de lumière. 
Le moteur à courant continu, comme nous avons vu dans le module « électronique de puissance, Master 1, semestre 1 », a un conducteur rectangulaire, représentant les enrou-lements d'induit, placés dans un champ magnétique. Le champ peut être fourni par des aimants permanents dans de petits moteurs, ou des enroulements de champ dans de plus grands. Un courant est passé à travers le conducteur, ce qui provoque un champ magné-tique cylindrique autour de lui. Ceci interagit avec le champ magnétique planaire fourni par les aimants de champ ou les enroulements, provoquant une force tangentielle sur le rotor, qui fournit le couple du moteur. Pour permettre la rotation, le courant est fourni à l'induit par l'intermédiaire d'anneaux et de brosses. Afin de maintenir le couple dans la même direction, le courant doit être inversé à chaque demi-tour, de sorte que la bague col-lectrice est divisée pour former un collecteur.
 # 1.7.1.	Interconnexion du moteur
Comme discuté ci-dessus, la fonction de base d'un moteur est de convertir le courant d'en-trée électrique en puissance mécanique de sortie (couple). Tous utilisent des bobines élec-tromagnétiques pour fournir cette conversion, et ont besoin de commutateurs de courant ou d'amplificateurs pour les exploiter à partir d'un MCU. Une méthode simple de com-mande de moteurs à courant alternatif consiste à utiliser un relais comme commutateur. Un autre est d'utiliser un triac pour contrôler le courant, comme indiqué ci-dessus, mais en pratique il y a quelques problèmes délicats associés à la commande de charges inductives avec des thyristors et triacs qui nécessitent la référence à des textes spécialisés. Les moteurs triphasés nécessitent, en termes simples, que chaque phase soit commandée par un disposi-tif séparé, mais simultanément, c'est-à-dire trois relais ou triacs exploités par le même con-trôleur.
Une interface typique de commande de petits moteurs (un moteur à courant continu) est représentée sur la figure 7.11. Le moteur et la lampe peuvent être actionnés en appuyant sur le « bouton démarrage moteur », la LED bleue sera allumée. Le switch SW1 est utilisé pour choisir entre le moteur et la lampe. La vitesse du moteur et l’intensité de la lumière peuvent être varies en utilisant les deux boutons « Vitesse du moteur ». L’algorithme du programme de contrôle est donné à la figure 7.12 et le code source dans le programme TP7.asm. Le fonctionnement de chaque interface sera expliqué à tour de rôle.

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/92131ec4-45d1-4d35-a028-854d5e59a95c)

                                                  Fig. 7.11.

 # 1.7.2.	Contrôle de vitesse avec PWM
Le moteur à courant continu est commandé à partir de la sortie PWM du PIC, via un FET VN66 de puissance. Cela a un courant de fonctionnement d'environ 1 A maximum, don-nant une puissance d'entrée maximale du moteur de 12 W à la tension de service de 12 V. Les caractéristiques du moteur peuvent être réglées dans la simulation, donc une résistance minimale du moteur d'environ 10 Ω Serait approprié, comme le FET lui-même a une résis-tance avant d'environ 1 Ω.
Le VN66 est un dispositif pratique à utiliser car il fonctionne aux tensions de grille de ni-veau TTL. C'est-à-dire que 0 V le met hors tension, + 5 V le met en marche (seuil d'environ 1 V). Il a une impédance d'entrée très élevée, ainsi la fiabilité est améliorée en ajoutant la résistance de shunt à la porte, pour améliorer l'immunité de bruit. La diode à travers le moteur est nécessaire pour couper la fore électromotrice de la charge inductive. Lorsque le système est démarré, le moteur DC commence à tourner, une sortie PWM par défaut est générée avec un rapport 50% de marge / espace. Le MSR (mark space ratio) peut alors être augmenté et diminué à l'aide des boutons haut / bas. Notez que le logiciel doit vérifier chaque fois que le MSR est modifié pour la valeur maximum (FF) ou minimum (00), pour empêcher le retournement et la suppression de la valeur PWM.

MOTEUR Courant contenu

Démarrer moteur CC par bouton poussoir, en utilisant P16F877 (4MHz)

Programme principal

Initialiser

        Port C = Sorties, moteurs à courant continu
        
        Port E = Entrées numériques, bouton-poussoir : 
        
                 Démarrage, Haut, Bas
                 
                 Taux PWM = 4kHz
                 
Attendre le bouton 'Bouton démarrage moteur'

        Sélectionnez le mode PWM, 50% MSR
        
RÉPÉTER

        CALL Moteur
        
TOUJOURS

Sous-programme Moteur

        Si Bouton 'Up' enfoncée
        
            Vitesse sera incrémentée à moins que le maximum
            
        Si Bouton "Bas" enfoncé
        
            Vitesse sera décrémentée à moins que minimum
            
REVENIR


                                          Fig. 7.12.

Le module PWM travaille avec le timer 2, et au niveau de Tcy de la façon suivante :
- Vous entrez votre valeur de débordement dans PR2
- À chaque fois que TMR2 déborde de PR2 à 0x00, la pin CCPx passe au niveau 1.
Comme le temps Tc est le temps séparant 2 montées successives du signal (ou 2 descentes), nous avons défini Tc. Pour rappel :
TC = (PR2 + 1) * Tcy * prédiviseur
Ou encore
Tc = (PR2 + 1) * 4 * Tosc * prédiviseur
En effet, un cycle d’instruction vaut 4 cycles d’oscillateur.
Reste donc à définir l’emplacement de redescente de notre signal pour obtenir notre signal complet.
Ce qu’il nous faudrait, c’est un deuxième registre « PR2 », qui nous indiquerait, au mo-ment où TMR2 atteindrait cette valeur, que le signal doit retomber à « 0 ». C’est ce qui est implémenté, en gros, dans notre PIC.
On va commencer par le principe général, qui va être comme suit :
On suppose que le registre CCP est initialisé à zéro. Le Timer 2 compte 
- TMR2 arrive à la valeur du registre PR2
- Au cycle suivant, TMR2 repasse à « 0 », CCPx passe à « 1 »
- TMR2 arrive à la seconde valeur de consigne, CCPx passe à « 0 »
- TMR2 arrive à la valeur de PR2
- Au cycle suivant, TMR2 = 0, CCPx vaut « 1 »
- TMR2 arrive à la seconde valeur de consigne, CCPx passe à « 0 »
- Et ainsi de suite…
Par exemple, avec un PIC cadencé à 20MHz, et une fréquence de votre signal PWM de 78,125 KHz :
Le temps Tc vaut : 1/78,125 Khz = 12,8 μs
Tc = (PR2 + 1) * Prédiviseur * 4 * Tosc
Tosc = 1/20MHz = 50 ns
PR2 = (TC / (prédiviseur * 4 * Tosc) )– 1
PR2 = (12,8μs / 200ns) – 1 = 64 – 1 = 63 (avec prédiviseur = 1)
→ On placera 63 dans notre registre PR2
Il sera alors possible d’ajuster notre rapport cyclique sur des valeurs comprises entre
B’00000000,00’ et B ‘00111111,11 », soit 256 valeurs différentes, donc une résolution sur 8 bits, ou encore une marge d’erreur de 0,4%.
En somme, la résolution vaut 1 / ((PR2+1) * 4), et est donc en toute logique multipliée par 4 par rapport à une comparaison sans utiliser de fractions (sur 8 bits).
Comme il faut intervenir sur des valeurs de consigne de 10 bits, l’écriture ne pourra pas se faire en une seule fois, et donc pourra provoquer une valeur temporaire indésirable. 
Mais, de nouveau, Microchip nous fournit une solution, en utilisant un registre intermé-diaire qui servira de valeur de comparaison, et qui sera chargé au moment du déborde-ment de TMR2.
La procédure exacte est finalement la suivante :
- Le Timer 2 compte : on imagine que le signal CCP vaut actuellement « 0 »
- TMR2 arrive à la valeur de PR2
- Au cycle suivant, TMR2 repasse à « 0 », CCPx passe à « 1 »
- En même temps, la valeur programmée comme consigne par l’utilisateur est copiée dans le registre final de comparaison.
- TMR2 arrive à la valeur de la copie de la seconde valeur de consigne, CCPx passe à « 0 »
- TMR2 arrive à la valeur de PR2
- Au cycle suivant, TMR2 = 0, CCPx vaut « 1 », et ainsi de suite
Vous en déduirez que toute modification de la valeur de comparaison, et donc de la durée de Th ne sera effective qu’à partir du cycle suivant celui actuellement en cours.
On conclut cette partie fortement théorique en résumant les formules qui seront utiles.
Tout d’abord le calcul de PR2, qui fixe le temps total de cycle Tc :
PR2 = (TC / (prédiviseur * 4 * Tosc) ) – 1
Ensuite, la formule qui lie les 10 bits de notre second comparateur, sachant que ces 10 bits (COMPAR) expriment un multiple de quarts de cycles d’instructions, donc un multiple de temps d’oscillateur, avec le temps du signal à l’état haut (Th).
Th = COMPAR * prédiviseur * Tosc.
COMPAR = Th / (prédiviseur * Tosc)
On peut également dire, en fonction du rapport cyclique (Rc), que :
Th = Tc * Rc
Il faut encore rappeler que pour comparer TMR2 avec COMPAR codé sur 10 bits, il faut que TMR2 soit également codé sur 10 bits. Il faut donc compléter TMR2 avec 2 bits qui représentent les quarts de cycle d’instruction.
En fait, tout se passe très simplement. Si vous choisissez un prédiviseur de 1, TMR2 sera complété, en interne, par le numéro (de 0 à 3) du temps Tosc en cours.
Si, vous utilisez un prédiviseur de 4, ce nombre sera complété par le nombre d’événements déjà comptabilisés par le prédiviseur (de 0 à 3).
Si, par contre, vous utilisez un prédiviseur de 16, ce nombre sera complété par le nombre de multiples de 4 événements déjà comptabilisés par le prédiviseur.
De cette façon, TMR2 sera bien complété avec 2 bits qui représentent des quarts de valeur. Inutile en fait de vous en préoccuper, c’est l’électronique interne qui s’en charge, mais, au moins, comme ça, vous le savez.
 # 1.7.3.	Les registres utilisés
Nous voici débarrassé de la théorie pure, passons maintenant à un peu de concret. Nous avons besoin de plusieurs registres pour programmer toutes ces valeurs. En fait, nous les avons déjà tous rencontrés, ne reste donc qu’à expliquer leur rôle dans ce mode particulier. Nous avons besoin d’une valeur de débordement pour notre timer 2, cette valeur se trouve, comme je l’ai déjà dit dans le registre PR2. C’est donc une valeur sur 8 bits. La va-leur de la seconde comparaison (celle qui fait passer la sortie de 1 à 0) est une valeur de 8 bits complétée de 2 bits fractionnaires.
Le nombre entier sera inscrit dans le registre CCPRxL. Les 2 bits fractionnaires qui complè-tent ce nombre sont les bits CCPxX et CCPxY du registre CCPxCON.
Comme nous l’avons vu, ce nombre de 10 bits sera copié en interne par le PIC® vers un autre registre, qui sera le registre CCPRxH complété de 2 bits internes non accessibles. No-tez qu’en mode « PWM », il vous est impossible d’écrire dans ce registre. Vous n’avez donc pas, en fait, à vous en préoccuper, le PIC® s’en charge.
Pour lancer le mode « PWM », nous devons procéder aux initialisations suivantes :
1) On initialise PR2 en fonction de la durée totale du cycle (Tc) :
PR2 = (TC / (prédiviseur * 4 * Tosc) – 1
2) On calcule la valeur de comparaison DBC (fractionnaire) suivant la formule :
DCB = Th / (prédiviseur * Tosc)
On place les bits 9 à 2 dans CCPRxL (valeur entière), les bits 1 et 0 (fraction) étant posi-tionnés dans CCPxX et CCPxY du registre CCPxCON.
3) On place la pin CCPx en sortie en configurant TRISC
4) On lance le timer 2 en programmant son prédiviseur
5) On configure CCPxCON pour travailler en mode « PWM ».
Il faut noter que les 4 derniers bits de CCPxCON doivent être B’11xx » pour travailler en mode PWM. Les bits 1 et 0 (xx) sont donc sans importance. Il semble qu’un bug existe dans certaines versions de PIC à ce niveau si vous utilisez simultanément les deux modules CCP. Dans ce cas, utilisez des valeurs différentes pour b1/b0 dans chaque registre, par exemple en utilisant B’1111’ pour CCP1CON et B’1100’ pour CCP2CON.
1.	Réaliser le circuit de la figure 7.11, en utilisant le logiciel Proteus.
2.	Éditer et compiler le code en assembleur TP7.asm.
3.	Double clic sur le PIC16F877 pour charger le fichier Project_7.X.production.hex , pour mon cas : C:/Users/Lyes/MPLABXProjects/Project_7.X/dist/default/production/Project_7.X.production.hex
4.	Exécuter le circuit dans Proteus, en appuyant sur le bouton « démarrage moteur » la LED doit être allumée et le moteur doit tourner avec une vitesse 790 tr/min. 
5.	Varier la vitesse du moteur en utilisant les boutons « Up » et « Down ».  Tracer la tension aux bornes du moteur pour les cas : minimum, medium et maximum. 
6.	Activer la lampe en utilisant le switch SW1 et refaire la question 5.

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/5f9a96f0-e4b7-46a8-a69d-a455def2dc14)
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/59a31f88-f144-43df-a181-973f2a8028e2)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/1de24c6f-5d6e-4988-95a5-2bd50391e8b2)
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/ec583076-ba5f-4cad-bbd1-5f938593982f)



