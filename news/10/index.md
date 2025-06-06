---
title: 60 FPS
excerpt: Dans le 10e numéro de la newsletter de Samuel Hackwill, on parle de performance informatique.
description: Dans le 10e numéro de la newsletter de Samuel Hackwill, on parle de performance informatique.
layout: single
toc: true
toc_sticky: true
og_image: /news/10/media/montage.jpeg
locale: "fr"
header: /news/10/media/montage.jpeg
---

_(temps de lecture : 20 minutes)_

Chères et chers camarades,

c'est le dixième numéro de cette newsletter soi-disant biannuelle. Bienvenue à celleux qui nous rejoignent.

La dernière fois que je vous ai écrit, c'était en Septembre 2024 et [je vous avais parlé beaucoup trop en détail](https://samuelhackwill.github.io/news/9/#un-bouddhiste-un-musulman-un-protestant-deux-catholiques-et-un-orthodoxe-sont-à-lassemblée-nationale) du projet de loi sur la fin de vie en France, et ce que les représentants du culte avaient à en dire : "la vie est la propriété de dieu donc pas touche minouche". Enfin, pour être plus spécifique, c'est ce qu'a dit le représentant des Orthodoxes de France (il a pas dit le mot "minouche" mais il a quand même réussi à traiter les députés de _païens_). Le texte a été adopté cette semaine à l'AN, et maintenant c'est au tour du Sénat de l'examiner.

<!-- Je ne sais pas quoi vous dire d'autre à ce sujet, j'oscille entre "c'est un petit progrès dans la bonne direction" et "c'est tellement infime que ça vaut même pas la peine d'en parler". https://x.com/samhackwill/status/1928428707938463816 -->

Anyway!

Cette newsletter va être assez technique, parce que j'ai envie de vous raconter comment j'ai fabriqué mon nouveau spectacle (que j'ai créé ce mois d'Avril).

![Stéphanie entourée de souris qui cliqueu cliqueu cliqueu](/news/10/media/playsteph.jpeg)

_↑ Ici on voit Stéphanie, avec qui j'ai la chance de travailler, et le dispositif tout autour d'elle._

## Tryhard

Cette saison j'ai donc passé beaucoup de temps à programmer des ordinateurs pour _Tryhard_, ma performance pour 56 pointeurs de souris, où le public complète une série de CAPTCHAs ("je ne suis pas un robot").

j'ai fait un teaser ici si vous voulez voir à quoi ça ressemble ↓

<iframe width="560" height="315" src="https://www.youtube.com/embed/zJ4_k_tVlSc?si=4SX-Ngzw4bncGjvN" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
<br/>

Écrire cette pièce était un challenge passionnant sur le plan technique et expressif. J'ai appris plein de choses! J'ai envie de partager avec vous les _3 obstacles techniques_ les plus intéressants que j'ai rencontré en cours de route, et les méthodes que j'ai mises en place pour les résoudre ou les contourner.

Par ailleurs je profite de l'occasion pour

- remercier les équipes de l'Elysée (où ont eu lieu les premières!), qui est officiellement le meilleur théâtre de Lyon
- vous inviter aux prochaines représentations de _Tryhard_ qui ont lieu à [Paris du 19 au 21 Juin, au MAIF Social Club](https://programmation.maifsocialclub.fr/evenements/tryhard/) (NB: gloups c'est complet, mais je vous encourage à vous inscrire sur la liste d'attente, apparemment ça bouge pas mal avant le jour J! Faites moi signe si vous galérez à avoir une place!)
- Je vais également jouer à Nantes en Décembre, come say hi!

## Introduction

![Le dispositif de Tryhard : un public, des souris, un écran](/news/10/media/schema.png)

Je me permets de reprendre du début. Dans Tryhard, tout le monde a une souris, et chaque personne est représentée à l'écran par son pointeur de souris. Nice! ou devrais-je dire, Mice! Mais comment on fait pour mettre 56 souris ensemble sur un écran?

Vous n'avez peut-être jamais tenté de brancher deux souris sur un ordinateur, mais si c'était le cas vous auriez constaté que ça ne fait pas apparaître un deuxième pointeur à l'écran. Et pourquoi ça? C'est simplement que les interfaces que nous utilisons n'ont pas été conçues avec le prémisse qu'on soit tout un groupe assis devant l'ordinateur. Ranger le bureau de son ordinateur n'a pas été pensé comme une activité familiale. Cliquer-glisser est un geste solitaire. Nos ordinateurs (contemporains) sont _personnels_, et les interfaces physiques que nous utilisons pour leur parler sont _mono-utilisateur_ par défaut. C'est ce qui fait (pour moi) une bonne partie du charme de ce projet : j'ai pris une interface _single-player_ datant des années 60 (le pointeur de souris), pour la hacker et la rendre _multijoueur_. A mon sens il y a une vertu de faire ça au théâtre, pour extraire certaines expériences numériques de la sphère privée et les rendre plus collectives, conviviales.

Partant de là, j'ai fait une étude de l'état de l'art, puis formulé tout un tas d'hypothèses plus ou moins techniques, plus ou moins chères, plus ou moins encombrantes, pour parvenir à mes fins, mais aucune n'était convaincante. J'ai fini par aboutir à la conclusion que la façon la plus simple de "brancher 56 souris ensemble" était d'utiliser une nuée de micro-ordinateurs, chacun connectés à un petit nombre de souris filaires (j'ai écarté l'option des souris sans fil parce que j'avais peur des interférences radio et je n'avais pas envie d'acheter 56 souris seulement pour faire un test grandeur nature, mais who knows peut être que c'est une solution crédible!), et reliés entre eux par "internet" (c'est à dire avec le protocole TCP/IP, des câbles RJ45, un routeur et un switch). C'est ça le dispositif de Tryhard!

Enfin, il faut ajouter encore deux machines : un vidéoprojecteur, et mon laptop qui joue le rôle de serveur (les micro-ordinateurs dans la salle lui retransmettent le signal des souris), sur lequel tourne une application web (à base de Node.js) qui a pour fonction de simuler une nuée de "pointeurs de souris". Je dis "pointeurs de souris" entre guillemets, parce qu'en fait ce sont de vulgaires `<div>` avec une image de pointeur de souris en `background-image`, pour ceux à qui ça parle. Ce ne sont pas de _vrais_ pointeurs de souris, ou on pourrait dire, des pointeurs de souris _natifs_. Ce sont des frankenstein de pointeurs de souris, un agglomérat putréfié de code et de pixels exhibant artificiellement des comportements proches de ceux d'un pointeur de souris normal. Ce qui a des conséquences pratiques, on y reviendra dans la dernière partie sur le _jank_.

## Problème n°1 : la latence

Je suis donc arrivé a une sorte de sandwich technique dont chaque couche apporte un peu de complexité supplémentaire. Ma toute première inquiétude était que quand on bouge la souris, tout ce beau monde ait besoin de trop de temps pour réfléchir, et que cela cause des délais d'affichage perceptibles visuellement (c'est à dire : on bouge la souris, il se passe rien pendant un temps x, puis le curseur bouge avec un peu de retard). Enfin, mon objectif n'était pas que la latence soit tout à fait invisible (pour cela il aurait fallu qu'elle soit inférieure à 20 millisecondes), mais qu'elle soit suffisamment faible pour ne pas être désagréable, disons, anything en-dessous de 200ms.

Je n'ai pas réussi à retrouver une bonne vidéo des distributeurs de billets SNCF pour donner un exemple de latence désagréable ; j'ai seulement trouvé cette vidéo intitulée ["J'achète mon billet à un distributeur automatique"](https://www.youtube.com/watch?v=zQrNUwgvvfM&ab_channel=R%C3%A9gionAuvergne-Rh%C3%B4ne-Alpes) où la machine fonctionne inhabituellement bien. On voit tout de même que lorsque l'utilisateur CLiQuE de toutes ses forces, l'ordinateur ne donne aucune indiction qu'il a bien pris en compte le clic : une à deux secondes de latence s'écoulent "silencieusement" avant que le menu suivant s'affiche. Not ideal. Néanmoins, je peux vous dire que contrairement à ce qu'on voit sur cette vidéo, le distributeur de billets qui est resté au soleil et à la pluie tous les jours de sa vie en gare de _LÉPIN-LE-LAC LA BAUCHE_, lui, exhibait facilement une latence de 2000 à 3000ms entre le moment où on tournait la tournette et une réaction de sa part, ce qui rend très technique le fait de sélectionner _CHAMBÉRY-CHALLES LES EAUX_ dans le menu, bon sang de bois, et voilà le train qui arrive en gare et on va rater le train à cause de cette maudite machine qui a certes pris la pluie pendant des années mais faillit maintenant à son devoir de service public.

Mon but était éventuellement de faire mieux que le distributeur de billets de la gare de Lépin, et qu'il n'y ait pas une latence de 3000ms entre un mouvement de souris et sa répercussion à l'écran. Quand j'ai fait le tout premier prototype de Tryhard, j'ai constaté visuellement que la latence était tout à fait raisonnable (contre toute attente), alors je suis tout simplement passé à autre chose.

<iframe width="560" height="315" src="https://www.youtube.com/embed/8Z8ORJYKFIU?si=jpboFxB_bpGyMVVj" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

_↑ Ceci est la toute première preuve de concept. À droite, on me voit bouger deux souris branchées sur le même PC, dont je "capture" les mouvements sur deux "machines virtuelles" grâce à une webapp qui suit mes mouvements. À gauche, je centralise tous les signaux sur une autre page web. C'est foireux mais ça marche! À ce stade il me restait encore un an pour optimiser tout ça._

Le stack (qui est une autre façon de dire : "sandwich technique") était biennnn différent à cette époque, j'ai tout réécrit depuis. Vous pouvez checker cette conversation [ici](https://github.com/function61/screen-server/discussions/10) si vous savoir comment j'ai fabriqué le tout premier prototype, avec l'aide d'un développeur Finlandais et de mon ami Etienne Boutin, qui est dev également. À l'époque il y avait plein de couches de virtualisation en plus, c'était vraiment une usine à gaz. Mais ça m'a mis sur la bonne voie et c'était la confirmation que mon approche était plausible.

<iframe width="560" height="315" src="https://www.youtube.com/embed/Cqm0_0RljFg?si=Jp-SbneHYh9f22H8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
<br/>

J'ai essayé de mesurer la latence approximative qu'il y a dans Tryhard (dans sa version actuelle), de la manière suivante : j'ai filmé mon écran au ralenti avec un téléphone (240 images / seconde), et j'ai utilisé [ffmpeg](https://fr.wikipedia.org/wiki/FFmpeg) pour décomposer la vidéo en fichiers jpeg, 1 image / frame. J'ai ensuite compté les images avec les doigts de ma main entre le moment où je frappe ma souris et le moment où le pointeur se décide à bouger (j'ai compté 28 frames, donc ça fait une latence de 28 x (1000/240) = ~104 ms). Not _that_ bad.

C'est une latence parfaitement acceptable pour mes besoins. Elle est perceptible, mais en réalité le plus important pour un jeu où on a besoin de bouger son curseur dans un espace 2D, c'est que la latence soit _constante_ et donc _prévisible_. Il est préférable qu'il y ait un peu de latence tout le temps mais sans grande variation, que très peu de latence most of the time avec des gros pics intempestifs. C'est la définition même du deuxième grand méchant problème informatique que j'ai rencontré : le jitter (variation de latence).

<!-- (ça n'a pas directement de rapport mais voici [une courte vidéo](https://youtu.be/6EwaW2iz4iA?feature=shared) que j'ai rencontré au cours de mes recherches et qui parle des stratégies mises en place dans les FPS pour compenser les variations de latence entre les joueur.euse.s, fascinating!) -->

## Problème n°2 : le jitter

La première fois que j'ai fait un test en conditions réelles avec 14 raspberry pi répartis dans les gradins du cinéma Méliès, à Villeneuve-d'Ascq, avec une quarantaine d'humains au bout de leurs souris, j'ai rencontré un problème plutôt inquiétant : les pointeurs ne se déplaçaient pas avec fluidité à l'écran. On le voit dans cette vidéo :

<iframe width="560" height="315" src="https://www.youtube.com/embed/M6oVb8VxIsw?si=t9gcmfeq4j4JnqP8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
  
_↑ ci-dessus j'ai mis une tite flèche à côté d'un curseur (EMO), pour qui tout se passe plutôt bien initialement, mais à un moment il semble tétanisé, avant de sauter subitement à une autre position. C'est ça le jitter. D'autres curseurs ont une latence qui varie encore plus vite, et la majorité d'entre eux n'ont pas un déplacement fluide._

Cette fois-ci on ne parle pas seulement d'un problème qui est désagréable, mais qui rend tout à fait impossible de repérer et de suivre son curseur à l'écran. J'étais obligé de résoudre ce problème si je voulais qu'il y ait un moment dans la performance où tout le monde joue ensemble, et plus globalement si je voulais que l'expérience soit confortable.

Dans la mesure où je ne pouvais pas séquestrer le public pendant un mois pour leur demander de faire bouger les souris à chaque fois que j'avais besoin d'éprouver une nouvelle version du code, j'ai dû mettre en place un environnement de test automatisé pour comprendre d'où ces variations de latence venaient. Dans la vidéo ci-dessous on voit le _jitter_ en action : certains curseurs font des sortes de pauses quand ça leur chante, pendant que les autres ont un déplacement fluide. Quand j'ai vu ça j'étais à la fois content (youpi j'ai réussi à répliquer le bug!) et dubitatif (pourquoi y'en a qui sont en bonne santé mais pas les autres alors que c'est le même code, les mêmes ordinateurs, les mêmes souris? huUuhH)

<iframe width="560" height="315" src="https://www.youtube.com/embed/dd-tWj8EB-c?si=aNuHfIR-uX5kArK6" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

_↑ Un détail intéressant dans la vidéo : à côté de chaque curseur s'affiche la latence maximum observée pour ce client ; vous remarquerez qu'il y a des valeurs négatives, ce qui est impossible (on ne peut pas remonter dans le temps, tout ça tout ça). Après avoir résolu mon problème de Jitter, j'ai été tenté de comprendre ce qui se passait avec ces valeurs aberrantes (probablement des problèmes d'horloge sur les rasps), et j'ai gaspillé une journée entière à essayer (en vain) de résoudre le "problème" - qui n'en était pas un puisque je n'avais pas du tout besoin de connaître ces valeurs (mon seul problème c'était le jitter). Qué s'apelerio, la différence subtile entre faire un pas de côté essentiel et le moment où on oublie quel était le problème à la base pour s'engouffrer dans une fiévreuse procrastination de code._

Long story short, le _jitter_ survenait parce que les raspberry étaient connectés ensemble en wi-fi, que j'utilisais un pauvre routeur sans antenne posé dans le fond de la salle, et que les corps humains dans la salle, qu'il faut considérer comme des gros sacs d'eau, ne sont pas gentils avec les ondes wifi (ils les bloquent). Les micro-ordinateurs répartis dans la salle avaient donc du mal a maintenir une connection stable avec le serveur, à part celui qui était juste à côté du routeur, ce qui explique pourquoi _quelques_ curseurs à l'écran ont un déplacement parfaitement fluide.

J'ai donc mis tous les rasps en connection filaire et le problème était 100% réglé. J'ai poussé un très gros soupir de soulagement parce qu'à l'époque j'étais convaincu à tort que le jitter était causé par le protocole de communication que j'utilisais entre les raspberry et le serveur (_WebSockets_), et je n'avais pas du tout envie de taper dans la gamme au-dessus parce que les protocoles plus rapides sont compliqués (une personne m'a dit un jour à leur sujet : "attention l'artiste, il faut savoir compiler du C"). Or je ne sais pas compiler du C, moi. Que la personne qui a déjà compilé du C me jette la première pierre.

## Problème n°3 : le jank

Le dernier problème que j'ai rencontré est arrivé assez tard dans le projet ("tard" comme "30 jours avant la première"). Je travaillais sur une séquence où chaque souris pouvait enfanter de nouvelles souris, saturant progressivement l'écran avec plein de petites souris. Ça n'a l'air de rien mais c'était beaucoup trop violent pour le navigateur web! Ou plutôt, c'était violent parce que c'était _codé_ d'une certaine façon qui n'était pas nécessairement optimisée. J'ai observé un ralentissement du framerate, c'est à dire que toutes les animations devenaient saccadées et pas fluides.

<iframe width="560" height="315" src="https://www.youtube.com/embed/tAeU1XIyIP8?si=C0JLlOEYWCuM64Pz" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

_↑ Alors dans cette vidéo on ne voit pas de ralentissement du framerate parce que je jouais tout seul ; mais quand je l'ai fait en live avec une vingtaine de personnes ça a tout pété. Je n'ai pas de vidéo malheureusement, mais imaginez un écran saturé de pointeurs de souris grabataires avec des déplacements tout saccadés. J'avais aussi rajouté un effet de flammes en fond d'écran. ET des explosions._

Ce phénomène survient parce que tout programme voulant afficher des choses sur un écran dispose d'un budget de 16,66 millisecondes pour faire tous ses calculs. Pourquoi 16,66ms? Comme vous le savez, l'illusion de mouvement est créée par l'affichage à un rythme rapide d'images fixes : c'est comme ça que fonctionnent les écrans. Aujourd'hui, on considère qu'en moyenne les écrans grand public affichent 60 images par seconde. Donc pour qu'un écran puisse bien faire son boulot, il faut que l'ordinateur puisse lui fournir ces 60 images par seconde ; c'est pour ça qu'il doit se dépêcher d'en fabriquer une nouvelle toutes les 16,66ms.

Dans ce scénario où on utilise ce type d'écran, quand les calculs de l'ordinateur prennent plus de temps que 16,66 millisecondes, il vont bloquer l'affichage pendant 1 frame, ce qui veut dire que l'ordinateur produit maintenant 59 images/seconde en moyenne. Si cette situation se répète ou que des calculs bloquent l'affichage pendant plusieurs frames, l'image peut devenir saccadée. En fait c'est exactement le même symptôme que le jitter (hélas, mien pointeur ne se mouvoist point avecques fluidité!), mais qui a des causes différentes. (Dans le cas du jitter, c'était la connection entre les raspberry pi et le serveur qui posait problème, alors que dans le cas qu'on est en train de décrire, il s'agit plutôt d'une surcharge _locale_ des capacités de calcul du serveur). Levez la main si vous aimez les ordinateurs.

Pour régler ce problème, il fallait donc que je comprenne quelles parties de mon code étaient les plus gourmandes en calculs, et faisaient exploser mon budget de 16,66ms. Sauf que c'est là où j'arrive à mes limites. Les problèmes de performance tels que je vous décris sont passionnants, mais ils nécessitent une compréhension fine de ce qui se déroule à _bas niveau_ "dans" l'ordinateur, et des conséquences mathématiques qui se cachent derrière telle ou telle stratégie employée pour résoudre un problème de programmation. Je vais tenter de vous donner un exemple.

Considérons un problème pratique que j'avais dans Tryhard. J'ai plein de pointeurs de souris, et j'ai envie de répliquer un effet sympathique et très commun sur le web : quand on survole certains éléments de la page avec sa souris, le pointeur pointu devient une main gantée de blanc, tel [mickey mouse](https://ux.stackexchange.com/questions/52503/who-created-the-mac-mickey-pointer-cursor). Si j'étais en train d'écrire du code pour un site web classique, il me suffirait d'ajouter une règle CSS comme ça :

```css
button:hover {
  cursor: pointer;
}
```

_↑ ce bout de code dit au navigateur web que tous les élements de type "button" doivent dire au pointeur de souris ("cursor") de changer son apparence ("pointer") quand ils sont survolés (":hover")._

...et je peux dormir sur mes deux oreilles, parce que je sais que les coûts de performance sont infimes. Cette ligne de code marchera parfaitement, même s'il y a 10000 boutons à l'écran, même si je bouge ma souris très vite, etc. Pourquoi? Parce que j'utilise une fonction "native" du navigateur web, qui agit sur le curseur "natif" de mon système d'exploitation. "Natif", dans notre cas, ça veut dire "bien foutu". "Bien foutu", ça veut dire que ça a été codé par des ingénieurs expérimenté.e.s avec des langages _bas niveau_ comme C ou C++, qui sont plus _abstraits_ que les langages que j'utilise moi, mais beaucoup plus performants.

MAIS. Vous vous rappelez peut-être que je disais en exergue qu'il n'y a pas de "vrais" pointeurs de souris dans Tryhard ; seulement des _simulations_ de pointeurs de souris. Des souris-frankenstein. C'est la raison pour laquelle je ne peux pas profiter "gratuitement" des fonctions natives du navigateur. Je dois tout ré-écrire moi-même a grands coups de Javascript, un langage plus accessible que le C ou le C++, mais beaucoup moins performant. Si je veux mon petit effet de je-survole-un-truc-et-hop-je-deviens-une-main-gantée, je dois effectivement écrire une fonction qui fait un truc comme ça :

```
"regarde où sont les 56 pointeurs de souris
les uns après les autres, et croise leurs coordonnées
avec celles des x boutons affichés à l'écran ;
si il y a une intersection entre les
coordonnées d'un pointeur et celles d'un bouton,
change l'apparence de ce pointeur de souris en un joli
gant blanc de mickey mouse. Ah oui et au fait,
fais ça *60 fois par seconde* stp. Oui c'est beaucoup
mais que voulez vous y'a l'écran qui veut manger son
image toutes les 16,66ms je vous rappelle."
```

la grosse différence ici, c'est que contrairement à notre pti bout de CSS tout à l'heure, le _coût en calculs de notre fonction croît (de manière linéaire) avec le nombre d'éléments affichés sur la page_. Au bout d'un moment, la quantité de calculs croît trop, et le navigateur web n'arrive plus à faire son taff en 16,66ms, ce qui finit par ralentir la fréquence d'affichage (ça _jank_).

<iframe width="560" height="315" src="https://www.youtube.com/embed/-2ktIHGBCU4?si=v40TwbnG7Y9Jw8hY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

_↑ ça c'est un "stress test" que j'ai programmé assez tôt dans l'écriture de Tryhard pour vérifier que mon code moyennement optimisé pouvait quand même tourner à 60Hz. Ça m'a rassuré et a confirmé que même si ma stratégie n'était pas optimale, elle était suffisamment performante pour la majorité de mes besoins. Ici on ne voit presque pas de jank alors qu'il se passe pas mal de trucs._

Je précise ici que contrairement à ce que mon ton pourrait suggérer, tout n'est pas de la faute de _Javascript_, le pauvre, qui demeure un langage très puissant avec lequel on peut faire plein de choses très vite. En réalité, il y a des manières plus ou moins _performantes_ de résoudre un problème de programmation, quelle que soit la langue, et par exemple, la stratégie que je viens de décrire pour changer l'apparence d'un pointeur de souris n'est pas la plus optimisée. On pourrait faire plus efficace, même avec _Javascript_. Par exemple :

```
"comme les boutons ne bougent pas, plutôt que de
demander leur position au navigateur 60 fois par
seconde, stocke leur position une fois pour toutes
dans une variable et regarde simplement si les
pointeurs se déplacent dans les mêmes
plages de coordonnées. Et un café s'il vous plaît"
```

Ou :

```
"c'est lent de chercher dans une graaaande liste
contenant tous les boutons à chaque fois
pour chaque pointeur : divise l'espace en 4 régions
(en haut à gauche, en haut à droite, en bas à gauche,
en bas à droite), met les boutons de chaque région dans la
variable correspondante, et regarde uniquement dans
la variable qui contient les boutons du coin de l'image où
se trouve le pointeur de souris, comme ça tu cherches
dans une liste qui est 4 fois moins longue, duh."
```

On voit que potentiellement, à mesure que le code devient plus performant, il devient aussi plus complexe et peut être plus difficile à lire (bon là en l’occurrence c'est pas du code mais on voit que mes petites instructions deviennent de plus en plus _convoluted_ comme on dit dans le métier.)

Anyways! Concluons : il me restait 30 jours pour terminer la pièce, les effets visuels les plus importants (que je ne vous montre pas pour pas vous spoiler!) fonctionnaient bien et à 60Hz, j'ai donc décidé de renoncer à la séquence qui était trop gourmande en calculs. Pour que cette séquence fonctionne bien, j'aurais dû profondément changer mon approche, peut-être même utiliser des langages différents, mais quoi qu'il en soit replonger pendant des semaines dans ma base de code pour la réécrire (on parle de "Refactoring").

## Conclusion

Comme souvent en informatique, il n’y a pas de solution parfaite, mais des compromis ; il faut trouver un équilibre entre complexité-lisibilité du code, rapidité de mise en oeuvre, et performance. C'est aussi pour ça qu'on se retrouve régulièrement à tout réécrire, ou réécrire une partie de son code ; à mesure que la situation change, les paramètres de l'équation se déplacent et modifient la "désirabilité" d'une option.

C'est tout pour cette fois! Merci d'avoir tout lu, wow!!! J'ai l'impression d'avoir [pair-programmé](https://en.wikipedia.org/wiki/Pair_programming) avec vous, c'était cool.

<3 <3 <3

Merci à [Stéphanie Aflalo](https://www.instagram.com/stephanie.aflalo), qui m'a accompagné de l'α à l'Ω durant la conception de la pièce, Étienne Boutin, qui m'a aidé à faire le premier prototype, Thomas Riou qui a assuré la production à l'Amicale, et Diane Landais, qui a écrit une partie de la base de code, et m'a conseillé face aux problèmes de performance.

<3 <3 <3

merci également à : Julie Holin, Célestine Dahan, Marine Thevenet, Pierre Pedinotti, les playtesteur.euse.s, Cédric Jouniaux, les contributeur.ice.s de Meteor.js, les allié.e.s, Camille Lamy, Joonas “Fi”, Jacques-Daniel Pillon, Victor Pauly, Jacob Lyon.

![Le gros sac contenant l'intégralité du dispositif](/news/10/media/bag.jpeg)
_↑ ça c'est le sac où je mets Tryhard ; il contient l'intégralité du dispositif, sauf les tapis de souris qui sont envoyés par la poste. Not bad huh_
