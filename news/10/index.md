---
title: 60 FPS
excerpt: C'est le 10e numéro de la newsletter de Samuel Hackwill! yiyyouyyooy
description: C'est le 9e numéro de la newsletter de Samuel Hackwill! yiyyouyyooy
layout: single
toc: true
toc_sticky: true
og_image: /news/10/media/montage.jpeg
locale: "fr"
header: /news/10/media/montage.jpeg
---

Chères et chers camarades,

c'est le dixième numéro de cette newsletter soi-disant biannuelle. Bienvenue à celleux qui nous rejoignent.

La dernière fois que je vous ai écrit, c'était en Septembre 2024 et je vous avais parlé beaucoup trop en détail du projet de loi sur la fin de vie en france, et ce que les représentants du culte avaient à en dire. Et ce qu'ils ont a en dire, c'est que la vie est la propriété de dieu donc pas touche minouche. Enfin, pour être plus spécifique, c'est ce qu'a dit le représentant des Orthodoxes de France. Les autres étaient moins maladroits et ont mieux réussi à faire semblant que leurs arguments jouaient sur un plan rationnel. Les débats sur le projet de loi, qui avait été interrompus par la dissolution de l'assemblée nationale, ont repris ce mois de Mai. Le texte sera voté mardi, et ensuite ça sera au tour du Sénat de l'examiner. A mon avis : ça va passer. A mon avis : le cadre d'application de la loi est insuffisant et devra être élargi.

<iframe width="560" height="315" src="https://www.youtube.com/embed/GA5Q0531-Eg?si=dPXqF42TbZjYvvjp" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<!--
 (grosso modo : la vie est la propriété de dieu donc pas touche minouche. Bon ils ont pas vraiment dit ça mais c'est ce qui se cache derrière leurs arguments pseudo-rationnels).

Les débats ont repris ce mois de Mai et on dirait que le texte est bien parti pour être adopté, enfin je ne vois pas dans l'avenir mais le projet de loi a beaucoup de poids politique derrière lui. E. Macron a dit que si le Sénat s'amusait à faire de l'obstruction, il claquerait son ultimate (un référendum).

Bon mais j'ai lu un peu les textes en diagonale et de toutes façons ce n'est pas très intéressant, a mon sens ce projet de loi remplace simplement la sédation profonde et continue jusqu'à la mort par des méthodes plus directes pour faire mourir les mourants. Cette nouvelle loi ne changera rien au fait qu'on doive patiemment attendre d'entrer dans la phase terminale de son agonie, et, bons élèves, que l'on puisse justifier de douleurs insupportables et continuelles au docteur pour ouvrir notre droit au guichet de l'aide à mourir. -->

# Tryhard

Cette fois je veux vous raconter un peu comment j'ai fabriqué _Tryhard_, ma performance pour 56 pointeurs de souris, qui m'a occupé ces derniers mois, et je profite de l'occasion pour vous inviter aux prochaines représentations qui ont lieu à [Paris du 19 au 21 Juin, au MAIF Social Club](https://programmation.maifsocialclub.fr/evenements/tryhard/) (c'est dans le marais, rue de Turenne). Come say hi!

![montage pour les premières au Théâtre de l'Elysée, Lyon](/news/10/media/montage.jpeg)

Je suis content parce que je n'étais pas 100% sûr que j'allais y arriver quand je me suis mis en route, il y a un an. J'ai rencontré plein de problèmes artistiques et techniques passionnants.

Les trois problèmes techniques les plus méchants que j'ai rencontré étaient :

1. la latence (c'est à dire la quantité de temps qui s'écoule entre le moment où on bouge la souris et le moment où le pointeur bouge à l'écran)

<!-- mon problème avec la latence c'est que c'est désagréable, ça donne une impression de "poids", ou de machine à gaz, à la personne qui utilise la souris. Not good -->

2. le jitter (c'est à dire les fluctuations de la latence - parfois yen a po, parfois y'en a plin)

<!-- mon problème avec le jitter c'est que c'est imprévisible et ça rend impossible de suivre son pointeur à l'écran (il se met à "sauter" d'un point à un autre). Super not good, deal breaker -->

3. le framerate (c'est à dire le nombre d'images que l'écran _parvient_ à afficher par seconde, on en veut 60 idéalement).
<!-- 
mon problème avec le framerate c'est que comme j'utilise le navigateur web en tant que moteur graphique, il n'est que moyennement optimisé pour les effets visuels. D'un point de vue technique, Tryhard n'est qu'un vulgaire site web, qui doit animer la position à l'écran de 56 souris + plein d'autres trucs 60 fois par seconde. Ça veut dire que mon ordinateur dispose d'un budget de 16.66 millisecondes (c'est à dire 1 seconde divisé par 60 images, soit ~0,016 secondes) maximum pour faire tous ses calculs at any given time (qui est où, qui fait quoi, qui clique sur quoi, quel résultat ça doit avoir, etc etc etc). That's not a lot of millisecondes les amis!

Alors après vous pourriez certes me dire mais Samuel 24 images par seconde ça suffit pour créer l'illusion de mouvement, même 12 images? est ce que tu n’exagérerais pas un petit peu là ho? et c'est là où vous faites une erreur cruciale, mais c'est normal, vous n'avez pas reçu un conditionnement pavlovien des beaux-arts, où on apprend que quand on fait quelque chose de visuellement SIMPLE, il faut que TOUT SOIT PARFAIT, sinon c'est la dèche.

Donc not vraiment a deal breaker, néanmoins, ça m'aurait brisé le coeur que ma performance tourne à 12 FPS tel un triple-A avec tous les réglages à fond sur un PC gamer bas de gamme. -->

Regardons un peu plus en détail de quoi il s'agit à chaque fois :

## la latence

Je me permets de reprendre du début. Le dispositif de Tryhard nécessitait de "brancher 56 souris ensemble", c'est à dire de rassembler 56 pointeurs de souris sur le même écran. Vous savez probablement que quand on branche deux souris sur le même ordinateur, ça ne fait pas apparaître un deuxième pointeur à l'écran. Les interfaces que nous utilisons n'ont pas été conçues avec le prémice qu'on soit toute une famille derrière un écran à composer un texte ensemble, avec chacun son clavier, ou de réorganiser des fichiers sur le bureau, chacun avec sa souris. Nos ordinateurs sont _personnels_, et leurs interfaces sont _single-player_ par défaut.Néanmoins cela pose un problème pratique pour moi : je ne peux pas prendre appui sur les systèmes existants et je dois en bricoler un moi-même.

Partant de là, j'ai formulé tout un tas d'hypothèses plus ou moins techniques, plus ou moins chères, plus ou moins encombrantes, dont aucune n'était convaincante. J'ai fini par aboutir à la conclusion que la façon la plus simple de "brancher 56 souris ensemble" était d'utiliser une nuée de micro-ordinateurs (j'ai opté pour des raspberry pi, mais il y a très certainement de meilleurs candidats), connectés à un petit nombre de souris chacun (j'ai écarté l'option des souris sans fil parce que j'avais peur des interférences radio et je n'avais pas envie d'acheter autant de souris pour faire un test grandeur nature, mais who knows peut être que c'est une solution crédible!), et reliés entre eux par "internet" (c'est à dire avec le protocole TCP/IP, des câbles RJ45, un routeur et un switch).

Je suis donc arrivé a une sorte de sandwich technique dont chaque couche apporte un peu de latence supplémentaire. Mon inquiétude était que quand on bouge la souris, tout ce beau monde ait besoin de trop de temps pour réfléchir, et que ce soit perceptible visuellement. Enfin, mon objectif n'était pas que la latence soit invisible (pour cela il aurait fallu qu'elle soit inférieure à 20ms), mais qu'elle soit suffisamment faible pour ne pas être désagréable, disons, anything en-dessous de 200ms.

mon estimation de latence pour chaque couche de mon infrastructure était la suivante :

- la souris = ~10 ms (la souris doit traiter le signal optique et comprendre dans quelle direction elle bouge. J'utilise des souris bas de gamme mais les souris "gamer" sont plus rapides)
- traitement du signal raspberry pi = 5-16 ms (le rasp ne fait que lire et expédier le signal de la souris, tel qu'il est interprété par les drivers de son système d'exploitation, donc ça va très vite. Néanmoins je l'ai bridé pour qu'il ne le fasse pas plus de 60 fois par seconde, afin de ne pas surcharger le serveur qui reçoit l'information de l'autre côté. Donc en fonction du moment précis où la souris est bougée par rapport à ces échéances d'envoi toutes les 16ms, cela peut créer entre 1 et 16ms de latence.)
- latence paquet TCP/IP entre les raspberry pi & le serveur = ~2 ms (le signal voyage à la vitesse de la lumière dans les cables ethernet, donc plutôt très vite, mais il se passe plein de trucs dans le switch et le routeur et ça prend du temps paraît-il)
- affichage de la souris = 5-16 ms (tous les rasps parlent un peu quand ils veulent au serveur, 60 fois par seconde, et le serveur, de son côté, compile et écrase tous ces signaux 60 fois par seconde également, avant d'afficher la souris dans sa position mise à jour sur le grand écran.)

total <44ms.

en pratique, la latence que j'observe dans mon système varie d'un facteur 2, woops! Il y a donc une latence d'environ 100ms entre le moment où on bouge la souris et le moment où ce mouvement est répercuté sur le pointeur. Pour obtenir une estimation de la latence, j'ai filmé mon écran et ma main au ralenti avec mon téléphone (240 images / seconde), et j'ai utilisé ffmpeg pour décomposer la vidéo en fichiers jpg, 1 image / frame. J'ai ensuite compté les images où le pointeur glande après que j'eusse percuté la souris de ma paume irascible

<!-- ![Démonstration de la latence : le pointeur de souris bouge après un délai](/news/10/media/latency.mov)
_désolé souris_ -->

Quoi qu'il en soit c'était une latence parfaitement acceptable pour mes besoins. Elle est perceptible, mais en réalité le plus important pour qu'un jeu où on a besoin de bouger rapidement son curseur dans un espace 2D soit confortable, c'est que la latence soit constante et prévisible. Ce qui nous amène à ma deuxième dite bête noire informatique.

## Jitter

La première fois que j'ai fait un test en conditions réelles avec 14 raspberry pi répartis dans un gradin et des humains au bout de chaque souris (en Décembre 2024), j'ai rencontré un problème plutôt inquiétant : il y avait manifestement une très forte variation de latence dans le déplacement des pointeurs à l'écran. On le voit dans cette vidéo :

<!-- ![Ma première rencontre avec le jitter](/news/10/media/jitter.mov)
_bro_ -->

plusieurs pointeurs ont un comportement saccadé, certains à tel point qu'ils disparaissent subitement hors-cadre. Cette fois-ci on ne parle pas seulement d'un problème qui est désagréable, mais qui rend tout à fait impossible de retrouver et de suivre son curseur à l'écran. J'étais 100% obligé de résoudre ce problème si je voulais qu'il y ait un moment dans la performance où tout le monde joue ensemble.

<!-- ![Je réplique le bug du jitter à la maison : certains pointeurs ont un déplacement saccadé](/news/10/media/jitter2.mov)
à gauche! à droite! -->

Dans la mesure où je ne pouvais pas séquestrer le public pendant un mois pour tester et modifier le système avec elleux, j'ai dû mettre en place un environnement de test automatisé pour comprendre d'où ces variations de latence venaient. Dans la vidéo on voit le _jitter_ en action : certains curseurs font subitement une sorte de pause pendant que les autres ont un déplacement fluide. Long story short, le _jitter_ survenait parce que les raspberry étaient connectés ensemble en wi-fi, que j'utilisais un pauvre routeur sans antenne posé dans le fond de la salle, et que les corps humains dans la salle, qu'il faut considérer comme des gros sacs d'eau, ne sont pas gentils avec les ondes wifi (ils les bloquent). Les micro-ordinateurs répartis dans la salle avaient donc du mal a maintenir une connection stable avec le serveur, à part celui qui était juste à côté du routeur, ce qui explique pourquoi _quelques_ curseurs à l'écran ont un déplacement parfaitement fluide.

J'ai donc mis tous les rasps en connection filaire et le problème était 100% réglé. J'ai poussé un très gros soupir de soulagement parce qu'à l'époque j'étais convaincu à tort que le jitter était causé par le protocole de communication que j'utilisais entre les raspberry (_WebSockets_), et je n'avais pas du tout envie de taper dans la gamme au-dessus parce que les protocoles plus rapides que j'avais en tête sont compliqués (une personne m'a dit un jour à leur sujet : "attention, il faut savoir compiler du C"). Or je ne sais pas compiler du C, moi. Que la personne qui a déjà compilé du C me jette la première pierre.

## Framerate

Comme on l'a dit plus haut, tout programme affichant des choses sur un écran dispose d'un budget de 16,66 millisecondes pour faire tous ses calculs (si on veut que ça soit fluide et good-looking. Enfin, "fluide" dans la mesure où ce programme est affiché par un vidéoprojecteur ou un moniteur 60Hz, qui produit donc 60 images par seconde. Le budget est encore plus réduit si on désire produire des effets visuels au maximum des capacités d'affichage d'un moniteur 240Hz par exemple). Quand un calcul prend plus de temps que 16,66 millisecondes, il va bloquer l'affichage pendant 1 frame, ce qui veut dire que le navigateur produit maintenant 59 images / secondes, sniff. Si cette situation se répète ou que des calculs bloquent l'affichage pendant plusieurs frames, l'image peut devenir saccadée. En fait c'est exactement le même symptôme que le jitter, mais qui a des causes différentes! (Dans le cas du jitter, c'est la connection qui bug, alors que dans le cas qu'on est en train de décrire, il s'agit plutôt d'une surcharge des capacités de calcul de l'ordinateur). Levez la main si vous aimez les ordinateurs.

<!-- ![Une souris qui créé plein de souris](/news/10/media/autoclic.mov)
_quand 56 personnes font ça en même temps, malheureusement ça pète tout._
 -->

J'ai rencontré des problèmes de ralentissement de mon framerate lors d'une séquence de Tryhard où je permet au public d'enfanter de nouveaux curseurs. L'écran se retrouve alors absolument rempli de petits curseurs animés qui cliquent tous frénétiquement. Ça n'a l'air de rien mais c'était beaucoup trop violent pour le navigateur web! Ou plutôt, c'était violent parce que c'était _codé_ d'une certaine façon qui n'était pas nécessairement optimisée.

<!-- ![Dev Tools](/news/10/media/fps.png)
une vue des outils de performance de Google Chrome. Pour y accéder = F12, puis cliquez sur "performance". Sur l'image j'étais en train d'auditer une séquence assez chère en calculs où je fais tomber plein de CAPTCHAs du ciel. On voit des captures d'écran de ce que le navigateur affiche dans la deuxième ligne en partant du haut. -->

J'ai utilisé les outils de Google Chrome pour auditer mon code et essayer de comprendre si j'avais de la marge de manoeuvre pour le rendre plus efficace. Dans Chrome, on peut enregistrer une session de navigation, et exporter les données brutes au format json (c'est une sorte de gros fichier texte). J'ai collé ce fichier dans _gloups_ chatGPT pour qu'il l'analyse et me donne des pistes sur les parties les plus gourmandes en calculs de mon code. Pour autant que je puisse en juger, c'était une méthode très efficace et j'ai amélioré les parties les plus bancales de ma base de code. Néanmoins les gains en performance n'étaient pas suffisants et j'ai dû abandonner la séquence de la saturation de curseurs (il aurait fallu prendre une approche trop différente et entièrement reprendre de zéro, en utilisant d'autres langages et d'autres logiques).

## Conclusion

<!--
Mon inquiétude au sujet de la latence est directement liée au dispositif que j'ai imaginé pour "brancher 56 souris ensemble". Après avoir formulé des premières hypothèses naïves de type "et si on mettait bout à bout un milliard de rallonges USB" ou "et si c'était plein de souris sans fil connectées au même ordinateur" ou "et si on soudait ensemble tous les cables USB et on fabriquait une sorte de signal électrique composite", je suis arrivé à une hypothèse plus réaliste et cost-effective : utiliser une nuée de micro-ordinateurs avec 4 souris chacun, qui envoient le signal de chaque souris à un serveur centralisant toutes les informations. C'est ce serveur qui affiche ensuite les pointeurs de souris à l'écran (ce sont donc des _simulations_ de pointeurs de souris) -->

<!-- , que j'ai commencé en gros en me disant "lol et si on prenait 56 souris et qu'on les branchait ensemble", et que j'ai fini en chantant "ça tient la charge" quelques jours avant la première, au théâtre de l'Elysée. Entre ces deux moments, il a dû se passer pas mal de choses pour que je sois autant de bonne humeur et que je décide de filmer l'écran du théâtre en criant "javascript javascript oh yeah". -->

<!--
# l'état de l'art

Je me souviens du moment où je suis arrivé à la conclusion qu'il était nécessaire et important que j'arrive à brancher 56 souris ensemble. J'étais dans un train magnifiquement silencieux entre Strasbourg et Bruxelles. Et je venais de pitcher _Tryhard_ à deux personnes. A l'époque j'avais une idée assez vague de ce que je voulais faire, mais j'avais fait une première étape de travail où le but de la pièce était de compléter des CAPTCHAs textuels, de ce type :

[image]

une personne montait au tableau et tapait le captcha comme il pouvait. J'avais


 qui après m'avoir écouté patiemment avaient pris la parole pour me signifier, sur le mode de la réponse, ou de la contradiction diplomatique, que ce qui était important au théâtre, c'était que le théâtre est un fait collectif, sous-entendu c'était une caractéristique insuffisamment explorée dans mon pitch, à bientôt. À l'époque, le dispositif de _Tryhard_ ne comprenait qu'un seul contrôleur (un clavier), et ne permettait qu'à une seule personne de jouer à la fois, pendent que le reste du public regardait la personne jouer.

Je ne pense pas que le retour de mes collègues était uniquement inspiré par le fait que le dispositif n'était _que_ single player, mais me voilà dans le train en train d'arriver à la conclusion qu'un contrôleur c'est bien mais que PLUS de contrôleurs c'est MIEUX.
 -->
