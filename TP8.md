# 1.8.	TP8 : Système d'acquisition de données biomédicales 
Dans ce TP, le signal électrocardiographique ECG, qui est un signal très important dans le domaine biomédical, est manipulé pour détecter les intervalles entre deux Rs (intervalle RR) successives. L’intervalle mesurée va nous permettre de calculer le nombre de batte-ment cardiaque et l’arythmie cardiaque. Puisque on va travailler avec le logiciel de simula-tion Proteus, les données d’un signal ECG doivent être arrangées avant qui peuvent être utilisées. Après avoir des données conditionnées, un circuit électronique est utilisé pour détecter les passages des Rs. Ces passages doivent être introduites au PIC16F877 pour cal-culer et afficher le temps entre RR.  
 # 1.8.1.	Un peu de théorie sur le diagnostic du signal ECG :
Il y a plusieurs paramètres qui peuvent être utilisés pour le diagnostic du comportement du cœur. L’intervalle QT et la durée entre deux ondes R (R-R) présentent des exemples de ces paramètres (voir figure 7.12).  L'intervalle QT est le temps écoulé depuis le début du complexe QRS, représentant la dépolarisation ventriculaire, jusqu'à la fin de l'onde T, ré-sultant de la repolarisation ventriculaire. L'intervalle QT normal est controversé, et plu-sieurs durées normales ont été rapportées. En général, l'intervalle QT normal est inférieur à 400 à 440 millisecondes (ms), ou 0,4 à 0,44 secondes. Les femmes ont un intervalle QT plus long que les hommes. Une fréquence cardiaque plus faible entraîne également un intervalle QT plus long.
Une façon rapide de distinguer un intervalle QT prolongé consiste à examiner si l'onde T se termine au-delà du point intermédiaire entre l'intervalle RR. Si l'onde T se termine au-delà de la moitié de l'intervalle RR, elle est prolongée. En fonction des effets de la fréquence cardiaque, l'intervalle QT corrigé (QTc) est fréquemment utilisé. Le QTc est considéré comme prolongé si plus de 450 ms chez les mâles et 470 ms chez les femelles. Il est calculé en utilisant la formule de Bezett, décrite ci-dessous :

QTc=(Intervalle QT)/√RR

QTc : intervalle corrigé de QT.
QT : intervalle entre le début de l’onde Q et la fin de l’onde T.
RR : Temps entre deux ondes R successives.
Il faut noter qu’il existe autres méthodes pour estimer la valeur de QT et il n’est pas claire qui est la plus efficace. 

La formule de Bazett est la plus utilisée en raison de sa simplicité. Il sur-corrige à des fré-quences cardiaques> 100 bpm et sous-corrige à des fréquences cardiaques <60 bpm, mais fournit une correction adéquate pour les fréquences cardiaques allant de 60 à 100 bpm.
À des fréquences cardiaques en dehors de la plage de 60 à 100 bpm, les corrections Frede-ricia ou Framingham sont plus précises et devraient être utilisées à la place.
Si un ECG est fortuitement capturé alors que la fréquence cardiaque du patient est de 60 bpm, l'intervalle QT absolu doit être utilisé à la place.
La prolongation de l'intervalle QT peut provenir de multiples médicaments, d'anomalies électrolytiques - hypocalcémie, hypomagnésémie et hypokaliémie - et de certains états pa-thologiques, y compris l'hémorragie intracrânienne.

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/0b8fb1f5-0754-4662-aa72-46edc2cd4eac)

Fig. 7.12

La fréquence cardiaque instantanée peut être calculée à partir du temps entre deux com-plexes QRS quelconques. L'inconvénient de cette méthode est que la fréquence cardiaque calculée peut être assez différente de l'impulsion mesurée même chez une personne nor-male en raison des variations de la fréquence cardiaque associée à la respiration (l'aryth-mie sinusale). Bien que le calcul de l'intervalle RR soit assez facile, en dérivant la fréquence cardiaque à partir de cela nécessite quelques étapes supplémentaires.
Fc=60/RR
1.	Pour la figure 7.13, calculer les valeurs de QT, RR, QTc et Fc.

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/41e77421-0644-425c-a1b3-aa75290967d6)
Fig. 7.13.

 # 1.8.2.	Manipulation :
Dans cette manipulation, on va utiliser un signal ECG réel que je l’ai conditionné pour que il sera manipulable par notre circuit. Le signal réel téléchargé depuis le site internet https://www.physionet.org/ a été passé par les changement suivants :
•	Normalisé entre 0 et 1 V.
•	Converti en format audio avec une fréquence d’échantillonnage de 1.5KHz.
Le signal obtenu va être appelé directement en utilisant l’outil « Generators » du Proteus.
Puisque le signal d’origine a été modifié, une correction doit être utilisée pour avoir les ca-ractéristiques originales. Le temps obtenu soit par l’oscilloscope soit sur l’afficheur doit être multiplié par le coefficient C = 12.1.  
Dans le circuit de la figure 7.14, on a trois amplificateurs opérationnels à usage général (TL084). Le premier est un sommateur qui fait la somme entre le signal ECG et -0.81V pour laisser seulement les pics des ondes R supérieur à 0. La sortie du premier Ampli Op va être amplifiée 10 fois et inversée par le second Ampli Op. Le troisième Ampli Op est compara-teur par rapport à zéro, qui nous donne 5 V si la valeur d’entrée est supérieure à zéro et -5V si inférieur. Pour éliminer les valeurs inférieures à zéro (-5V), la diode 1N4148 est utili-sée (pour plus de détails, consulté la brochure du module circuits de conditionnement, S1, M1).
La sortie du circuit de conditionnement va être introduite à l’entre RC2/CCP1 du micro-contrôleur, dont la fonction CCP1 de la broche va être utilisée.  Le mode de fonctionne-ment pour Timer1 est le mode de capture. Ceci permet à la valeur de compteur dans le registre de minuterie 16 bits (TMR1H + TMR1L) d'être capturée à mid-comptage. La cap-ture est déclenchée par l'entrée de l'état de changement RC2. Un pré-scaler peut être inclus entre l'entrée et l'activation de capture de sorte que la capture ne soit déclenchée qu'à la 4ème ou 16ème impulsion à l'entrée, réduisant ainsi le taux de capture. Cette méthode est utilisée ici pour mesurer la période d'une forme d'onde d'impulsion, qui est alimentée en RC2, l'entrée du module CCP1. Le module est configuré pour générer une interruption lorsque l'entrée passe de haut en bas, une fois par cycle. L'horloge d'instruction de minute-rie est en continu et le comptage atteint est mémorisé dans la paire de registres CCP1 à chaque interruption et le comptage est ensuite redémarré.

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/4fbdb554-edf3-43dd-bcdb-5f0f3302ac96)

Fig. 7.14.
Le programme principal lui-même convertit en continu le contenu 16 bits de CCP1 en BCD à 5 chiffres et affiche le résultat sur l'écran LCD. Il peut attendre l'interruption sui-vante dans une boucle de ralenti, ce programme met en évidence que le MCU peut conti-nuer le traitement pendant que la minuterie compte. Le binaire est converti en BCD en utilisant une simple boucle de soustraction pour chaque chiffre, de dix milliers à dix (voir la série d’exercice N°2). Le reste à la fin est la valeur de l'unité. L'écran LCD 16x2 est con-necté et piloté comme décrit au TP5. Le comptage maximum absolu est de 65536 µs (comp-tage 16 bits), et la durée minimale est limitée par le temps nécessaire pour réinitialiser l'interruption et recommencer le comptage (moins de 20 μs). Le code TP8.asm a été réalisé pour couvrir toute la plage audio, 15 Hz-50 kHz, ce que signifier qu’on peut l’utiliser pour la même tache avec un temps entre les im-pulsions de 20 μs à 65536 µs (figures 7.14-7.16).  

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/5db74c61-ca4b-4abc-8cd4-8b8e400140cd)

Fig. 7.15.

Algorithme ECG

      Mesurer la période d'entrée de l'onde d'impulsion et afficher P16F877 (4MHz),
      
      Source de signal audio, LCD 16x2 
      
Programme principale

      Initialiser
      
      PortD = Sorties LCD
      
      Mode de capture et interruption
      
Initialiser LCD

      Activer l'interruption de capture
      
      Répéter
      
         Convertir le nombre de 16 bits en 5 chiffres BCD
         
         Affichage d'une période d'onde carrée en entrée
         
      Toujours
      
Sous routines

Convertir le nombre de 16 bits en 5 chiffres BCD

      Charger le nombre de 16 bits
      
      Effacer les registres
      
        Tentes, Thous, Hunds, Tens, Ones
        
      Répéter
      
        Soustraire 10000 du nombre
        
      Jusqu'à Tentes sera négatif
      
        Restaurer les tentes et le reste
        
      Répéter
      
        Soustraire 1000 du reste
        
      Jusqu'à Thous sera négatif
      
        Restore Thous et le reste
        
      Répéter
      
        Soustraire 100 du reste
        
      Jusqu'à Hunds sera négatif
      
        Restaurer les Hunds et le reste
        
      Répéter
      
        Soustraire 10 du reste
        
      Jusqu'à Tens sera négatif
      
        Restaurer les dizaines et stocker le reste
        
Affichage d'une période entre les ondes R 

        Affichage 'T ='
        
        Suppression des zéros principaux
        
        Afficher les chiffres en ASCII
        
        Afficher 'us'
        
ROUTINE DE SERVICE D’INTERRUPTION

        Effacer Timer 1 pour comptage
        
        Indicateur d'interruption de remise à zéro

Fig. 7.16.

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/86f826a6-8e36-4cca-b4fc-a8b2f044b235)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/c034736a-ae2b-4566-8cf9-bf627d836793)
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/ee696ef9-ccd1-45e2-994e-9e4101b638ee)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/be4c3d71-10be-4659-978c-56e9475d3356)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/06d97a72-3532-4f09-a3f9-6765d095879b)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/3dc6c9a4-4d58-4400-bffe-a0b4ff86253b)
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/07b08f6e-7e29-4a2d-bf22-bcc7d40f7160)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/c678495d-f80b-4853-9e3c-780d06687854)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/be0c42e7-0d4b-407c-af78-34c3dcb74419)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/b2b54ddb-94b3-4324-b58d-797866754e40)
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/052080ae-498b-40e8-bee5-ad38c655f506)

1.	Réaliser le circuit de la figure 7.14, en utilisant le logiciel Proteus.
2.	Éditer et compiler le code en assembleur TP8.asm.
3.	Double clic sur le PIC16F877 pour charger le fichier Project_8.X.production.hex , pour mon cas : C:/Users/Lyes/MPLABXProjects/Project_8.X/dist/default/production/Project_8.X.production.hex
4.	Exécuter le circuit dans Proteus, vous aurez ce que donné par la figure 7.17.

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/5740a22f-6194-41ba-b0ec-1eb1ae4dc96e)

Fig. 7.17.

5.	Sur l’oscilloscope, mesurer les valeurs de QT, RR, et calculer les valeurs de QTc et Fc.
6.	Calculer QTc et Fc en utilisant QT de l’oscilloscope et RR de l’afficheur LCD.
7.	Comparer les valeurs obtenues en question 1 de la théorie et celles obtenues d’après les questions 2 et 3 de la manipulation.   












