# TP3 : Commande d’une LED clignotante
  Dans cet exercice nous voulons faire clignoter notre LED (figure 7.7) à une fréquence de 1Hz (1 clignotement par seconde). 
Nous allons de ce fait créer un programme de la forme :
debut
J’allume la LED
J’attends ½ seconde
J’éteins la LED
J’attends ½ seconde
Je retourne au début

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/3bd1d3bb-0a37-4f64-a1fd-28ef902f4d71)

                                            Fig. 7.7.
                                            
  Pour obtenir un clignotement à 1Hz il faut temporiser à 500ms et non à 1 s. Nous voyons que nous utilisons 2 fois la temporisation de ½ seconde. Nous créerons donc un sous-programme que nous appellerons « tempo ». Notre programme principal aura donc l’aspect suivant :
start
     bsf LED    ; allumer la LED
     call tempo ; appeler la tempo de 0.5s
     bcf LED    ; éteindre LED
     call tempo ; appeler la tempo de 0.5s
     goto start ; boucler

On aurait pu écrire également (en utilisant nos macros)
start
     LEDON      ; allumer la LED
     call tempo ; appeler la tempo de 0.5s
     LEDOFF     ; éteindre LED
     call tempo ; appeler la tempo de 0.5s
     goto start ; boucler

  Choisissez la méthode que vous préférez. L’important, c’est d’avoir compris les deux mé-thodes. Je vous rappelle cependant que la seconde méthode est plus propre et plus aisée à maintenir, même si ça semble demander un léger effort supplémentaire au codage initial.
Nous n’avons pas encore vu le timer0, ni les interruptions. Le but ici est de vous faire comprendre le fonctionnement du 16F84. Pour réaliser une tempo, il suffit dans notre cas de faire perdre du temps au 16F84 entre chaque inversion de la LED.
Nous devons donc perdre approximativement 0.5s. Les secondes ne sont pas appropriées pour les PIC®, qui travaillent à une vitesse beaucoup plus élevée. Nous utiliserons donc des unités à l’échelle de temps du PIC®.
  Cette notion « d’échelle de temps » est très importante en programmation, il faut toujours essayer de vous représenter les évènements qui arrivent comme si vous étiez un PIC. At-tendre 1 seconde équivaut alors à une éternité, et certains évènements qui vous semblaient négligeables deviennent alors parfaitement perceptibles, nous en reparlerons.
Notre PIC16F877 est cadencé à la fréquence de notre quartz, soit 20MHz. Or, le PIC exé-cute un cycle d’instruction tous les 4 cycles de l’horloge principale. Le PIC exécutera donc (20.000.000/4) = 5 millions cycles d’instruction par seconde. Il existe également d’autres modèles de PIC 8 bits capables de travailler avec des vitesses encore bien plus importantes.
La plupart des instructions (hormis les sauts) s’exécutent en 1 cycle d’instructions (4 tops d’horloge), ce qui vous donne approximativement 5 millions d’instructions par seconde. Vous verrez parfois la dénomination MIPS. Ceci signifie Million d’Instructions Par Se-conde. Notre PIC® avec ce quartz a donc une puissance de traitement de près de 5MIPS.
Chaque cycle d’instruction dure donc 0.2 microseconde (μs). Voilà donc l’unité pour tra-vailler avec notre PIC®.
  Donc, notre tempo de 0.5s est donc une tempo de 500000 microsecondes. Autrement dit, nous devons « perdre pour rien » 2500000 cycles d’instruction dans notre routine de tem-porisation. Vous voyez donc que votre PIC®, pour notre application, va passer son temps à ne rien faire d’utile : 1 instruction utile (allumer ou éteindre la led) pour 2500000 instruc-tions destinées à lui faire perdre son temps.
  La première idée qui vient à l’esprit est de réaliser une boucle qui va incrémenter ou dé-crémenter une variable.
On commence donc par déclarer notre variable (cmpt1) dans la zone de RAM. Il y a plu-sieurs méthodes pour réaliser cette tâche, soit en utilisant la directive CBLOCK ou bien UDATA. Les deux méthodes sont équivalentes de point de vue de code source généré, mais elle se déferrent pour le débogage. On peut voir les contenus des variables mémoire si on utilise la directive UDATA.  
  À titre d’exemple, prenant cette sous-routine de temporisation et on va calculer la durée occupée :
tempo
       clrf cmpt1        ; 1 cycle
boucle1
       decfsz cmpt1 , f  ; 1 cycle si on ne saute pas, 2 si 
                         ; on saute. Donc, on ne sautera 
                         ; pas 255 fois et on sautera 1 fois
       goto boucle1      ; 2 cycles multiplié par 255 passages
       return            ; 2 cycles.
  Nous l’avons vu, un temps est équivalant à un nombre de cycles d’instructions. Nous pouvons donc mesurer du temps en comptabilisant les cycles d’instructions exécutés.
Le temps total est donc de :
2 cycles pour l’appel de la sous-routine (call tempo)
1 cycle pour le reste de la variable
257 cycles pour les 256 décrémentations
510 cycles pour les 255 goto
2 cycles pour le return.
Soit un total de 772 cycles. On est loin des 2500000 cycles nécessaires. Pour la suite des calculs, nous allons négliger les 2 cycles du call et les 2 cycles du return (comparés aux
2500000 cycles, c’est effectivement dérisoire dans notre application).
Bon, nous allons allonger notre routine, en réalisant une seconde boucle qui va forcer la première boucle à s’exécuter 256 fois. Commençons par déclarer une nouvelle variable cmpt2 :
cmpt1 : 1 ; compteur de boucles 1
cmpt2 : 1 ; compteur de boucles 2

Ecrivons donc les 2 boucles imbriquées :
tempo
       clrf cmpt2        ; effacer compteur2
boucle2
       clrf cmpt1        ; effacer compteur1
boucle1
       decfsz cmpt1 , f  ; décrémenter compteur1
       goto boucle1      ; si pas 0, boucler
       decfsz cmpt2 , f  ; si 0, décrémenter compteur 2
       goto boucle2      ; si cmpt2 pas 0, recommencer boucle1
       return            ; retour de la sous-routine
  Vous voyez que notre première boucle est toujours là, mais au lieu d’effectuer le return une fois terminée, nous recommençons la boucle tant que cmpt2 ne devient également pas nul. Nous allons donc exécuter 256 fois notre boucle1.
Quelle est la temporisation obtenue ? Calculons approximativement :
Durée de la boucle 1 : 257 cycles + 510 cycles + 1 cycle (clrf cmpt1) = 768 cycles. Or cette boucle va être exécutée 256 fois, donc 768*256 = 196608 cycles, auquel il convient d’ajouter les quelques cycles d’initialisation et instructions de la boucle elle-même. Or, nous désirons 2500000 cycles. Nous devrons donc utiliser cette double-boucle (2500000/196608) = 12,72 fois. Nous ne savons pas faire de demi boucle. Nous effectuerons donc 12 boucles. Il nous manque 140704 cycles. Pour réaliser cette nouvelle temporisation, on va utiliser boucle1 183 fois (183*768=140544). 
1.	Réaliser le circuit de la figure 7.7, en utilisant le logiciel Proteus.
2.	Comme a été montré dans les TPs précédant. Éditer et compiler le code en assem-bleur TP3.asm.
3.	Double clic sur le PIC16F877 pour charger le fichier Project_3.X.production.hex , pour mon cas : C:/Users/Lyes/MPLABXProjects/Project_3.X/dist/default/production/Project_3.X.production.hex
4.	Exécuter le circuit dans Proteus, la LED doit clignoter avec une cadence de 1 s. 
5.	La temporisation (tempo et tempo2) que nous avons utilisé et approximativement égale 0.5 s, calculer la durée exacte. 
6.	Modifier le code de telle sorte la LED va clignoter avec une fréquence de 0.5 Hz et 2 Hz.
7.	Modifier le code et le circuit pour que 7 autres LEDs en plus reliées au PORTB. Les LEDs seront allumées l’une après l’autre.
   
![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/95c0dd54-81e4-4ba6-9c91-93ab2ac8d71a)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/370651ac-9675-4e2d-adee-c3afa7f3addd)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/32b50bd1-71a7-4ab5-9a65-50381e4b8989)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/fdb75a0d-fc9a-48a7-996b-e1450ab6414b)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/0e70b662-6088-48ed-ab11-7507414e7dab)






