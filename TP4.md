# 1.4.	TP4 : Principe de l’affichage multiplexé
Comme nous l'avons appris dans le TP2, un affichage à 7 segments est le premier type d'un affichage électronique qui utilise 7 barres de LED disposés d'une manière qui peut être utilisé pour montrer les chiffres 0-9. En fait, le segment 8 si vous comptez le point décimal, mais le nom générique adopté est un affichage à 7 segments. Ces périphériques sont cou-ramment utilisés dans les horloges numériques, les compteurs électroniques, la signalisa-tion et d'autres équipements pour afficher uniquement des données numériques.
Les segments des affichages sont normalement désignés par les lettres «a» à «g». La façon la plus simple d'afficher un chiffre sur les 7 segments est de trouver un moyen de détermi-ner ou de rechercher le motif correspondant au chiffre à afficher.
Cela peut être quelque chose comme une table montrant les nombres et le segment corres-pondant qui devrait être activé ou désactivé pour afficher quelque chose et le nombre re-quis (cela peut être en décimal, hexadécimal ou en format binaire) pour être envoyé au port où l'afficheur est connecté pour afficher un numéro spécifique.
Pour afficher plus de chiffres, il est possible de joindre plus d'un afficheur à sept segments pour 1 chiffre au besoin.
Un afficheur d’un chiffre sur 7 segments ne peut afficher que des nombres de 0 à 9, un afficheur à 2 chiffres peut afficher des nombres compris entre 00 et 99, un afficheur à 3 modules de 7 segments pour des chiffres entre 000 et 999, un afficheur à 3 modules de 7 segments pour des chiffres entre 0 et 9999, …etc.
La figure 7.8 montre un affichage à 7 segment à 2 chiffres connecté à un microcontrôleur PIC16F877.
Lorsque plus de chiffres sont nécessaires pour être affichés, nous devons trouver une meil-leure technique pour connecter plus de 1 segment à 7 segments d'affichage à un microcon-trôleur, car si nous les relier comme l'affichage à 1 chiffre, nous serons bientôt à court d'en-trée / Broches de sortie.
Un affichage à 1 chiffre à 7 segments nécessite 7 broches de sortie, un chiffre à 2 chiffres nécessiterait 14 et un chiffre à 4 chiffres nécessiterait 28, ce n'est certainement pas un moyen efficace d'utiliser un microcontrôleur. La technique commune largement utilisée est de multiplexer les chiffres pour enregistrer les broches d'entrée / sortie. Tous les chiffres partagent les mêmes broches de microcontrôleur plus quelques broches supplémentaires pour relier les chiffres à la masse ou à une puissance positive selon que l'on utilise des seg-ments de cathode ou d'anode communs.

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/a70ec126-1b3e-4e33-9492-d891b958db92)

                                                  Fig. 7.8.

Avec le multiplexage, un affichage à 2 chiffres ne nécessitera que 9 broches, un affichage à 3 chiffres nécessitera 10 broches, un affichage à 4 chiffres nécessitera 11 broches et ainsi de suite. Un autre avantage du multiplexage des LEDs à 7 segments est de réduire considéra-blement la consommation d'énergie.
Dans les applications multiplexées, tous les segments de chiffre sont entraînés en parallèle en même temps, mais seulement la broche commune (par exemple, anode ou cathode) du chiffre requis est activée. En activant ou en désactivant les chiffres si vite qu'il donne l'im-pression à l'œil que les deux écrans sont allumés en même temps que l'œil humain ne peut pas le différencier lorsque la vitesse est trop élevée. Cette technique est basée sur le prin-cipe de Persistance de la Vision de nos yeux. Si les images changent à un rythme de 25 (ou plus) images par seconde, l'œil humain ne peut pas détecter ce changement visuel.
Par exemple, disons que nous voulons afficher le chiffre '17' sur un affichage cathodique commun à 2 chiffres. Les étapes sont données ci-dessous :
1. Envoyez les données pour afficher '7' sur les deux chiffres.
2. Activer le chiffre gauche en mettant à la masse son axe cathodique (envoyer un haut à la base du transistor) et désactiver le chiffre droit.
3. Attendez un moment. (Un court délai)
4. Envoyer les données pour afficher '1' sur les deux chiffres.
5. Activez le chiffre droit en mettant à la terre son axe cathodique (envoyez un
La base du transistor) et désactiver le chiffre gauche.
6. Attendez un moment (un court délai).
7. Revenez à l'étape 1. En faisant cela rapidement, l'œil ne remarquera aucune fluctuation.
Les broches communes de chaque chiffre sont généralement contrôlées à l'aide de commu-tateurs de transistors, presque n'importe quel transistor NPN tel que le transistor de type BC108 pourrait être utilisé à cet effet. Une résistance de 1KΩ peut être utilisée pour limiter le courant de base à environ 4mA suffisamment pour saturer le transistor.
La figure 1 montre comment un affichage à 2 chiffres peut être connecté à un microcontrô-leur à l'aide de transistors NPN pour commander les lignes de segment. Notez que le ré-glage de la base d'un transistor à la logique HAUTE mettra le transistor en marche et donc permettra à la goupille commune de cathode reliée à lui.

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/31221351-7ea8-47b7-aa85-9db9210c4696)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/14b9e03f-a201-4c84-a258-51ea9d24e5b0)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/b182e109-15a7-4a05-9578-35ddf42a2a9d)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/42e06192-1fee-4030-84af-6ed4b1c0ddd1)

![image](https://github.com/LyesSaadSaoud/MicroController.github.io/assets/78357759/16d417fc-5843-467b-acb4-04174e089b5a)

1.	Réaliser le circuit de la figure 7.8, en utilisant le logiciel Proteus.
2.	Comme a été montré dans les TPs précédant. Éditer et compiler le code en assem-bleur TP4.asm.
3.	Double clic sur le PIC16F877 pour charger le fichier Project_4.X.production.hex , pour mon cas : C:/Users/Lyes/MPLABXProjects/Project_4.X/dist/default/production/Project_4.X.production.hex
4.	Exécuter le circuit dans Proteus, les deux afficheurs vont afficher des zéros. 
5.	Appuyer sur les boutons poussoirs pour tester le comptage et le décomptage.
6.	Calculer la durée de temporisation. 
7.	Modifier le code et le circuit pour ajouter un troisième afficheur sept segments. Les opérations de comptage et décomptage vont varier de 000 à 999. 





