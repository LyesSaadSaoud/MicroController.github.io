# 1.6.	TP6 : Acquisition de grandeur non électrique 
De nombreuses applications de commande nécessitent la mesure de variables analogiques, telles que la tension, la température, la pression, la vitesse et ainsi de suite. Comme a été monté dans le cours (chapitre 5), le PIC16F877 contient des convertisseurs analogiques numériques (CAN) qui peuvent travailler en mode 8 bits (256 niveaux) ou bien en mode 10 bits (1024 niveaux).
 Le CAN est commandé à partir des registres de fonctions spéciales ADCON0 et ADCON1, et peut générer une interruption périphérique si nécessaire. La sortie du con-vertisseur est stockée dans ADRESH (résultat de conversion analogique / numérique, octet élevé) et ADRESL (octet bas). Le traitement pour un résultat de 8 bits est plus simple, ce qui sera décrit dans ce TP. 
 # 1.6.1.	Circuit de conversion 8 bits
De nombreux capteurs qui doivent être connectés à une entrée analogique de microcontrô-leur ont un signal de sortie plutôt petit. En outre, il ne peut être disponible qu'en sortie de tension différentielle, c'est-à-dire entre deux points avec une grande tension de mode commun.
Les jauges de contrainte sont habituellement connectées de cette façon. Ils pourraient me-surer de petits changements dans la forme d'une pièce mécanique en contrainte, telle qu'une barre dans une flèche de grue. Cette jauge est très utilisée pour mesurer les signaux électrocardiogramme (ECG). Le capteur comprend typiquement quatre résistances sen-sibles à la déformation connectées en un montage en pont, de sorte qu'une variation de leur résistance due à l'étirement est délivrée en tant que petite tension différentielle, typi-quement dans la plage 0-10 mV (pour plus d’informations sur les jauges, vous pouvez consulter le support du cours de circuits de conditionnement -Master 1-semèstre 1 écrit par moi-même).
Un amplificateur sensible est nécessaire, avec un gain élevé et une haute résistance d'en-trée, ce qui réduit le courant extrait du capteur et donc les erreurs. Un amplificateur non inverseur monophasé a une résistance d'entrée élevée, mais n'a pas d'entrées différen-tielles. En outre, s'il est configuré pour un gain élevé, avec une valeur élevée pour RFF, le courant de réaction est faible, et donc l'amplificateur est plus sensible aux erreurs de bruit et de décalage.
La solution est un amplificateur d'instrumentation, qui combine les caractéristiques re-quises (Figure 7.10). Il se compose de deux étages : l'amplificateur de différence principale et deux étages d'entrée à haute impédance. Afin de voir quelques-unes des limites, notre amplificateur opérateur standard unique, LM324, a été utilisé, mais la performance peut être améliorée en sélectionnant un ampli-op de spécification plus élevé ou en achetant l'amplificateur d'instrumentation comme un ensemble spécial.
Le gain de l'amplificateur est fixé par le rapport de la chaîne de résistance de réaction con-nectée entre les sorties des étages d'entrée, à partir de la relation
G = 1 + 2R2/R1 , d’où R2 = 10 kΩ and R1 = 202 Ω
G = 1 + 20000/202 = 100
Cela fournit le gain requis. La tension différentielle maximale d'entrée est de 10 mV, le dif-férentiel de sortie est de 1,00 V. Ceci est ensuite transmis à l'étage de sortie différentiel, qui a un gain unitaire, dont le rôle est de fournir une sortie à une seule sortie (c'est-à-dire me-surée par rapport à 0 V).
Le 16F877 dispose de huit entrées analogiques disponibles en RA0, RA1, RA2, RA3, RA5, RE0, RE1 et RE2. Ceux-ci ont d'autres étiquettes AN0-AN7 pour cette fonction. RA2 et RA3 peuvent être utilisés comme entrées de tension de référence, en définissant les valeurs minimale et maximale pour la plage de tension mesurée. Ces entrées sont par défaut en mode analogique, de sorte que le registre ADCON1 doit être initialisé explicitement pour utiliser ces broches pour l'entrée ou la sortie numérique.
L'entrée de tension de test à RA0 (entrée analogique AN0) est obtenue directement de la sortie de l’amplificateur U2 : B. Une tension de référence est fournie à RA3 (AN3), qui dé-finit la tension maximale à convertir, et donc le facteur de conversion requis dans le logi-ciel. La valeur minimale par défaut est 0 V. La diode Zener de 2,7 V fournit une tension de référence constante. Elle est alimentée via une résistance de limitation de courant, de sorte que le zener fonctionne au courant spécifié pour une stabilité de tension optimale. 
Celui-ci est ensuite divisé à travers le pot de tension de référence RV1 et une résistance fixe de 10k. La gamme à travers le pot est d'environ 2,7-2,4 V, et est ajusté pour 2,56 V, ce qui donne un facteur de conversion pratique. L'écran LCD est connecté au port D pour fonc-tionner en mode 4 bits et afficher la tension, comme décrit au TP N°2.

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/4697837d-347d-4cd6-a2bb-090ba43a2fb8)

                                            Fig. 7.10.
 # 1.6.2.	Programme de conversion 8 bits
Le programme de fonctionnement (TP6.asm) est donné ci-dessous. Le port de sortie et les registres de commande du CAN sont initialisés dans le premier bloc, avec le fichier d'affi-chage LCD fournissant l'initialisation d'affichage et les routines de pilote. La boucle princi-pale contient des appels de sous-programme pour lire l'entrée du CAN, convertir de bi-naire en BCD et l'afficher. La routine pour lire le CAN définit le bit GO/ DONE et les in-terroge jusqu'à ce qu'il soit effacé à la fin de la conversion.
Le résultat 8 bits de ADRESH est converti en trois chiffres BCD par l'algorithme de sous-traction décrit précédemment. L'entrée à pleine échelle est 255, qui s'affiche sous 2,55 V.
1.	Réaliser le circuit de la figure 7.10, en utilisant le logiciel Proteus.
2.	Comme a été montré dans les TPs précédant. Éditer et compiler le code en assem-bleur TP6.asm.
3.	Double clic sur le PIC16F877 pour charger le fichier Project_6.X.production.hex , pour mon cas : C:/Users/Lyes/MPLABXProjects/Project_6.X/dist/default/production/Project_6.X.production.hex
4.	Exécuter le circuit dans Proteus, le mV mètre doit afficher des valeurs entre 0.02 à 24.9 mV et l’afficheur doit afficher 00.5 à 24.8 mV. 
5.	Expliquer le circuit en détail (plus de détails peuvent être trouver dans le support de cours circuits de conditionnement).
6.	Le capteur de pression est très utilisé en monde réel, par exemple dans le stéthos-cope et les stations météorologiques. On veut mesurer la pression atmosphérique, pour cela le capteur MPX4115A de Motorola est un bon choix. Il est alimenté par 5V et fournit et émet entre ~ 0,25V et ~ 4,75V en fonction de la pression détectée à température ambiante (25 ° C). Le dispositif fournit une sortie linéaire basée sur la pression. Lorsque la pression augmente, la tension de sortie du capteur augmente aussi avec ~ 0,25V représente une pression <15 kPa par rapport au vide et ~ 4,75V représente> 115 kPa.
Noter que : 1 atmosphère de pression au niveau de la mer est égale à 101,325 Pa ou 101 kPa. D’après le datasheet, on peut voir que la réponse de sortie typique d'un capteur de pression MPX4115A, inférieur à 15KPa et supérieur à 115KPa, la tension ne change pas. Le MPX4115A est donc un capteur idéal pour les applica-tions du baromètre ou altimètre à microcontrôleur. 

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/a9912ac3-66b5-49ca-bdea-a70ce92b0e1b)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/c760867e-bedf-42d7-89a1-907d8a6c6e20)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/0f6c620b-3f65-473a-a925-877bcfcb929b)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/a2deb9ad-a464-458a-9a76-baffea4447d2)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/271a7d65-2063-4753-8def-617620d8363a)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/d15e4502-d9f7-4498-a194-38fdcf5c79e8)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/8b05c6df-fbde-4b49-a6bc-d52d4f8d8bb9)

Il est très facile d'interagir le MPX4115A avec un PIC, si vous utilisez le capteur à 8 broches, reliez la broche 2 à + 5V, la broche 3 à la masse et la sortie est sur la broche 4 (branchez à la broche analogique RA0 du PIC). Laisser le reste sans rap-port. Si vous utilisez le capteur à 6 broches, la broche 1 est la sortie (brancher à la broche analogique RA0 du PIC), la broche 2 à la masse et la broche 3 à + 5V. Lais-ser le reste non connecté. 
Modifier le circuit de la figure 7.10 (déconnecter la sortie de l’ampli Op U2 :B et relier la proche  du MPX4115A que vous avez utilisé au RA0). Modifier le code pour qu’il affiche la pression générée par le capteur. 
7.	On veut maintenant mesurer la température. Il existe de nombreux capteurs de température disponibles sur le marché. Mais le capteur de température LM35 est utilisé dans ce projet. Il est le moins cher et on peut facilement le trouver sur le marché. Il existe de nombreux autres avantages de LM35 comme :
-	Il est plus efficace que la thermistance
-	Il est constitué de circuit intégré, donc pas de risque d'endommager les circuits internes.
-	Il tire du courant uniquement en micro-ampères.
Seule une alimentation de 5 volts est nécessaire pour LM35 et il n'est pas nécessaire de cir-cuit supplémentaire pour le faire fonctionner. 
Le capteur de température LM35 convertit la température en sa valeur de tension analo-gique proportionnelle. LM35 est un appareil à trois terminaux. Les broches numéro un et trois correspondent à une alimentation en tension de 5 volts. La broche 2 est la sortie de tension analogique par rapport à la valeur de la température. La relation entre la tempéra-ture mesurée et la tension de sortie analogique est :
                                                               1°C = 10 mV
Par conséquent, pour chaque augmentation de 1 degré de température, il y aura un incré-ment de 10 mV en tension de sortie du capteur LM35. 
Utiliser le circuit (Fig. 7.10) et le code TP6.asm pour mesurer la température. 
8.	Le dernier point demander est de mesurer l’intensité de la lumière.  La science derrière la mesure de lumière à l'aide de LDR est très simple. On va utiliser quelques formules de base que l'intensité dépend de la distance aussi. Donc on n’a pas en mesure de trouver une formule parfaite pour trouver l'intensité lumi-neuse précise en cas de LDR (résistance à la relation d'intensité). On va donc utili-ser le changement de résistance maximum et minimum pour faire une estimation de l'intensité de la Lumière. Cela aidera dans l'application LDR de base. La for-mule est à l'intensité minimum LDR a Résistance maximale offerte qui est d'envi-ron 990kΩ qu’on va mesurer dans Proteus en utilisant des sondes de courant et de tension. Résistance maximale signifie une lumière minimale (approche de l'inten-sité lumineuse nulle). Maintenant, pour trouver l'intensité en pourcentage, on a cette formule :
"I = (Résistance de LDR (à la lumière courante) / Résistance Maximum) x 100"
Ceci donne le pourcentage mais la diminution de résistance avec l'augmentation dans l'intensité ainsi, on va employer des mathématiques simples :

"I = 100 - I (Intensité mesurée à partir de la formule ci-dessus)"

Pour réaliser cette tâche, on va relier une résistance fixe (R = 10kΩ) avec un LDR. Cela crée un circuit diviseur de potentiel. On va relier ensuite le point commun de LDR et R3 au canal RA0 (AN0) pour mesurer la tension potentielle du diviseur.
Modifier le circuit (Fig. 7.10) et le code (TP6.asm) pour réaliser cette tâche.








