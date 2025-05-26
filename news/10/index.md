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

La dernière fois que je vous ai écrit, c'était en Septembre 2024. Je vous avait fait un compte-rendu détaillé d'une table ronde de l'Assemblée nationale (en France), lors de laquelle un groupe de religieux avaient été invités à venir donner leur avis au sujet du projet sur la fin de vie. C'est [ici](https://samuelhackwill.github.io/news/9/#un-bouddhiste-un-musulman-un-protestant-deux-catholiques-et-un-orthodoxe-sont-à-lassemblée-nationale) si vous voulez vous y replonger. Les débats sur le projet de loi, qui avaient été interrompus par la dissolution de l'assemblée nationale, ont repris ce mois de Mai. Le texte sera voté à l'Assemblée aujourd'hui même (mardi 27 mai), et ensuite ça sera au tour du Sénat de l'examiner.

Mais!

Je n'ai pas assez parlé d'informatique la dernière fois donc, avec votre permission, parlons de code.

 <!-- et ce que les représentants du culte avaient à en dire. Et ce qu'ils ont a en dire, c'est que la vie est la propriété de dieu donc pas touche minouche. Enfin, pour être plus spécifique, c'est ce qu'a dit le représentant des Orthodoxes de France. Les autres étaient moins maladroits et ont mieux réussi à faire semblant que leurs arguments jouaient sur un plan rationnel. Les débats sur le projet de loi, qui avait été interrompus par la dissolution de l'assemblée nationale, ont repris ce mois de Mai. Le texte sera voté mardi, et ensuite ça sera au tour du Sénat de l'examiner. A mon avis : ça va passer. A mon avis : le cadre d'application de la loi est insuffisant et devra être élargi. -->

<!--
 (grosso modo : la vie est la propriété de dieu donc pas touche minouche. Bon ils ont pas vraiment dit ça mais c'est ce qui se cache derrière leurs arguments pseudo-rationnels).

Les débats ont repris ce mois de Mai et on dirait que le texte est bien parti pour être adopté, enfin je ne vois pas dans l'avenir mais le projet de loi a beaucoup de poids politique derrière lui. E. Macron a dit que si le Sénat s'amusait à faire de l'obstruction, il claquerait son ultimate (un référendum).

Bon mais j'ai lu un peu les textes en diagonale et de toutes façons ce n'est pas très intéressant, a mon sens ce projet de loi remplace simplement la sédation profonde et continue jusqu'à la mort par des méthodes plus directes pour faire mourir les mourants. Cette nouvelle loi ne changera rien au fait qu'on doive patiemment attendre d'entrer dans la phase terminale de son agonie, et, bons élèves, que l'on puisse justifier de douleurs insupportables et continuelles au docteur pour ouvrir notre droit au guichet de l'aide à mourir. -->

![Stéphanie entourée de souris qui cliqueu cliqueu cliqueu](/news/10/media/playsteph.jpeg)

# Tryhard

J'aimerais vous dire deux mots au sujet de la fabrication de _Tryhard_, ma performance pour 56 pointeurs de souris, que j'ai créée ce mois d'Avril. Je profite de l'occasion pour vous inviter aux prochaines représentations qui ont lieu à [Paris du 19 au 21 Juin, au MAIF Social Club](https://programmation.maifsocialclub.fr/evenements/tryhard/) (c'est dans le marais, rue de Turenne). Come say hi!

Je vais vous parler ici plus en détail des trois pires difficultés techniques que j'ai rencontré sur mon chemin, et qui ont toutes trait à la _performance_ (c'est à dire la vitesse et l'efficience des calculs qui font exister _Tryhard_) :

1. la _latence_ (c'est à dire la quantité de temps qui s'écoule entre le moment où on bouge la souris et le moment où le pointeur bouge à l'écran)
2. le _jitter_ (c'est à dire les fluctuations de la latence - parfois yen a po, parfois y'en a plin)
3. le _jank_ (c'est quand l'ordinateur n'arrive plus à produire 60 images par secondes et que du coup les effets visuels perdent en fluidité).

Regardons un peu plus en détail de quoi il s'agit à chaque fois, et des méthodes que j'ai mises en place pour régler le problème :

## introduction

![Le public de Tryhard, assis dans des sièges rouges et des souris sur les genoux](/news/10/media/crowd.jpg)

Je me permets de reprendre du début. Tryhard, c'est une performance où je dispose 56 souris dans les gradins d'un théâtre ou d'une salle de cinéma, que le public utilise pour jouer (et le jeu, en l’occurrence, est de compléter une série de CAPTCHAs). Chaque personne est représentée à l'écran par son pointeur de souris. Nice! ou devrais-je dire, Mice! Mais comment on fait pour mettre 56 souris ensemble sur un écran?

Vous avez peut-être déjà essayé de brancher deux souris sur un ordinateur. Non? Ok personne n'a jamais essayé de brancher plus d'une souris sur un ordinateur, mais si c'était le cas vous auriez constaté que ça ne fait pas apparaître un deuxième pointeur à l'écran. Et pourquoi ça? C'est simplement que les interfaces que nous utilisons n'ont pas été conçues avec le prémisse qu'on soit toute un groupe derrière son écran à interagir simultanément avec une multitude de _contrôleurs_ (des souris, des claviers, des joysticks, etc). Ranger le bureau de son ordinateur n'a pas été pensé comme une activité familiale. Nos ordinateurs (contemporains) sont _personnels_, et leurs interfaces sont _mono-utilisateur_ par défaut. C'est ce qui fait (pour moi) une bonne partie du charme de ce projet : j'ai pris une interface _single-player_ datant des années 60 (le pointeur de souris), et je l'ai hackée pour la rendre _multijoueur_. (Je ne suis évidemment pas le premier à le faire, et mon travail se place dans la filiation de tout un courant d'expériences artistiques, sociales ou logicielles : [twitch plays pokemon](https://en.wikipedia.org/wiki/Twitch_Plays_Pokémon), [Figma](https://www.google.com/search?client=safari&sca_esv=7f3b3e58f08bcb9f&rls=en&q=figma+dev+mode&tbm=nws&source=lnms&fbs=AIIjpHxU7SXXniUZfeShr2fp4giZ1Y6MJ25_tmWITc7uy4KIeiAkWG4OlBE2zyCTMjPbGmMYufkAHQMrNdZtNwJ4J9GCNYntnPRFCDKspCuZSigBgFj3YFbhCGG3Fh1WL1C-KtQHcz4c3qzD-9H44aKPrM7x3yYC7ZJYDwGiUipxC-dldPt0h_bLGXWAVZgSnUgKmmj6df21&sa=X&ved=2ahUKEwisrsOj1cCNAxVCQ6QEHRW1FxQQ0pQJegQIDhAB&biw=1440&bih=772&dpr=2), les projets du [studio Moniker](https://studiomoniker.com) pour n'en citer que trois)

Partant de là, j'ai formulé tout un tas d'hypothèses plus ou moins techniques, plus ou moins chères, plus ou moins encombrantes (je voulais tourner en train donc c'était une donnée importante aussi), pour parvenir à mes fins, mais aucune n'était convaincante. J'ai fini par aboutir à la conclusion que la façon la plus simple de "brancher 56 souris ensemble" était d'utiliser une nuée de micro-ordinateurs (j'ai opté pour des raspberry pi. J'aurais probablement pu utiliser des machines moins "généralistes", mais ça m'aurait coûté plus de recherche et de temps), connectés à un petit nombre de souris chacun (j'ai écarté l'option des souris sans fil parce que j'avais peur des interférences radio et je n'avais pas envie d'acheter 56 souris seulement pour faire un test grandeur nature, mais who knows peut être que c'est une solution crédible!), et reliés entre eux par "internet" (c'est à dire avec le protocole TCP/IP, des câbles RJ45, un routeur et un switch). C'est ça le dispositif de Tryhard!

Enfin, il faut ajouter encore deux machines : mon laptop qui joue le rôle de serveur (les micro-ordinateurs dans la salle lui retransmettent le signal des souris), sur lequel tourne une application web qui a pour fonction de simuler une nuée de "pointeurs de souris" sur une page web, et un vidéoprojecteur qui vidéoprojecte tout ça sur grand écran. Je dis "pointeurs de souris" entre guillemets, parce qu'en fait ce sont de vulgaires `<div>` avec une image de pointeur en `background-image`, pour ceux à qui ça parle. Ce ne sont pas de _vrais_ pointeurs de souris, ou on pourrait dire, des pointeurs de souris _natifs_. Ce sont des frankenstein de pointeurs de souris, un agglomérat putréfié de code et de pixels exhibant artificiellement des comportements proches de ceux d'un pointeur de souris normal. Ce qui a des conséquences pratiques, on y reviendra dans la dernière partie sur le _jank_.

## la latence 🙀

Je suis donc arrivé a une sorte de sandwich technique dont chaque couche apporte un peu de complexité supplémentaire. Mon inquiétude était que quand on bouge la souris, tout ce beau monde ait besoin de trop de temps pour réfléchir, et que ce soit perceptible visuellement. Enfin, mon objectif n'était pas que la latence soit tout à fait invisible (pour cela il aurait fallu qu'elle soit inférieure à 20 millisecondes), mais qu'elle soit suffisamment faible pour ne pas être désagréable, disons, anything en-dessous de 200ms.

Je n'ai pas réussi à retrouver une bonne vidéo des distributeurs de billets SNCF pour donner un exemple de latence désagréable ; j'ai seulement trouvé cette vidéo intitulée ["J'achète mon billet à un distributeur automatique"](https://www.youtube.com/watch?v=zQrNUwgvvfM&ab_channel=R%C3%A9gionAuvergne-Rh%C3%B4ne-Alpes) où la machine fonctionne inhabituellement bien. On voit tout de même que lorsque l'utilisateur CLiQuE de toutes ses forces, l'ordinateur ne donne aucune indiction qu'il a bien pris en compte le clic : une à deux secondes de latence s'écoulent "silencieusement" avant que le menu suivant s'affiche. Not ideal. Néanmoins, je peux vous dire que le distributeur de billets qui est resté au soleil et à la pluie tous les jours de sa vie en gare de _LÉPIN-LE-LAC LA BAUCHE_ exhibait facilement une latence de 2000 à 3000ms entre le moment où on tournait la tournette et une réaction de sa part, qu'on puisse enfin sélectionner _CHAMBÉRY-CHALLES LES EAUX_ dans le menu, bon sang de bois, et voilà le train qui arrive en gare et on va rater le train à cause de cette maudite machine qui a certes pris la pluie pendant des années mais faillit maintenant à son devoir de service public.

Mon but était éventuellement de faire mieux que le distributeur de billets de la gare de Lépin, et qu'il n'y ait pas une latence de 3000ms entre un mouvement de souris et sa répercussion à l'écran. Quand j'ai fait le tout premier prototype de Tryhard, j'ai constaté visuellement que la latence était tout à fait raisonnable (contre toute attente), alors je suis tout simplement passé à autre chose.

<iframe width="560" height="315" src="https://www.youtube.com/embed/8Z8ORJYKFIU?si=jpboFxB_bpGyMVVj" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Le stack (c'est à dire ce qu'on a appelé tout à l'heure "sandwich technique") était biennnn différent à cette époque, j'ai tout réécrit depuis. Vous pouvez checker cette conversation [ici](https://github.com/function61/screen-server/discussions/10) si vous savoir comment j'ai fabriqué le tout premier prototype, avec l'aide d'un développeur Finlandais et de mon ami Etienne Boutin.

<iframe width="560" height="315" src="https://www.youtube.com/embed/Cqm0_0RljFg?si=Jp-SbneHYh9f22H8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Pour délirer j'ai essayé de mesurer la latence approximative qu'il y a dans Tryhard (la version actuelle), de la manière suivante : j'ai filmé mon écran au ralenti avec mon téléphone (240 images / seconde), et j'ai utilisé [ffmpeg](https://fr.wikipedia.org/wiki/FFmpeg) pour décomposer la vidéo en fichiers jpeg, 1 image / frame. J'ai ensuite compté les images avec les doigts de ma main entre le moment où je frappe ma souris et le moment où le pointeur se décide à bouger (j'ai compté 28 frames, donc ça fait une latence de 28 x (1000/240) = ~104 ms). Not _that_ bad.

C'est une latence parfaitement acceptable pour mes besoins. Elle est perceptible, mais en réalité le plus important pour un jeu où on a besoin de bouger rapidement son curseur dans un espace 2D, c'est que la latence soit _constante_ et donc _prévisible_. Il vaut mieux qu'il y ait un peu de latence tout le temps mais sans grande variation, que très peu de latence most of the time avec des gros pics intempestifs. C'est la définition même du deuxième grand méchant problème informatique que j'ai rencontré : le jitter.

<!--
mon estimation de latence pour chaque couche de mon infrastructure était la suivante :

- la souris = ~10 ms (la souris doit traiter le signal optique et comprendre dans quelle direction elle bouge. J'utilise des souris bas de gamme mais les souris "gamer" sont plus rapides)
- traitement du signal raspberry pi = 5-16 ms (le rasp ne fait que lire et expédier le signal de la souris, tel qu'il est interprété par les drivers de son système d'exploitation, donc ça va très vite. Néanmoins je l'ai bridé pour qu'il ne le fasse pas plus de 60 fois par seconde, afin de ne pas surcharger le serveur qui reçoit l'information de l'autre côté. Donc en fonction du moment précis où la souris est bougée par rapport à ces échéances d'envoi toutes les 16ms, cela peut créer entre 1 et 16ms de latence.)
- latence paquet TCP/IP entre les raspberry pi & le serveur = ~2 ms (le signal voyage à la vitesse de la lumière dans les cables ethernet, donc plutôt très vite, mais il se passe plein de trucs dans le switch et le routeur et ça prend du temps paraît-il)
- affichage de la souris = 5-16 ms (tous les rasps parlent un peu quand ils veulent au serveur, 60 fois par seconde, et le serveur, de son côté, compile et écrase tous ces signaux 60 fois par seconde également, avant d'afficher la souris dans sa position mise à jour sur le grand écran.)

total <44ms. -->
<!--
[vidéo du pointeur de souris]

en pratique, la latence que j'observe dans mon système varie d'un facteur 2, woops! Il y a donc une latence d'environ 100ms entre le moment où on bouge la souris et le moment où ce mouvement est répercuté sur le pointeur. Pour obtenir une estimation de la latence, j'ai filmé mon écran et ma main au ralenti avec mon téléphone (240 images / seconde), et j'ai utilisé ffmpeg pour décomposer la vidéo en fichiers jpg, 1 image / frame. J'ai ensuite compté les images où le pointeur glande après que j'eusse percuté la souris de ma paume irascible -->

<!-- ![Démonstration de la latence : le pointeur de souris bouge après un délai](/news/10/media/latency.mov)
_désolé souris_ -->

## le jitter 🙀

La première fois que j'ai fait un test en conditions réelles avec 14 raspberry pi répartis dans les gradins du cinéma Méliès, à Villeneuve-d'Ascq, avec une quarantaine d'humains au bout de leurs souris, j'ai rencontré un problème plutôt inquiétant : il y avait manifestement une très forte variation de latence dans le déplacement des pointeurs à l'écran. On le voit dans ces deux vidéos, tournées par Julie Holin qui m'a filé la main sur cette résidence (merci Julie!) :

<iframe width="560" height="315" src="https://www.youtube.com/embed/X1CXLbgvw9A?si=3017jopQsZIPCSTL" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

ci-dessus j'ai mis une tite flèche rouge à côté d'un curseur particulièrement jittery (APA). Au lieu de se déplacer avec souplesse d'un point à un autre, il "saute" ; sa position n'est pas mise à jour 60 fois par seconde comme on aimerait, mais dans ce cas, 3 ou 4 fois maximum, gloups.

<iframe width="560" height="315" src="https://www.youtube.com/embed/M6oVb8VxIsw?si=t9gcmfeq4j4JnqP8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

ci-dessus j'ai mis une tite flèche à côté d'un autre curseur (EMO), pour qui tout se passe plutôt bien initialement, mais à un moment il semble tétanisé, avant de sauter subitement à une autre position. C'est ça le jitter.

La majorité des pointeurs ont un comportement saccadé. Cette fois-ci on ne parle pas seulement d'un problème qui est désagréable, mais qui rend tout à fait impossible de repérer et de suivre son curseur à l'écran. J'étais obligé de résoudre ce problème si je voulais qu'il y ait un moment dans la performance où tout le monde joue ensemble.

<iframe width="560" height="315" src="https://www.youtube.com/embed/dd-tWj8EB-c?si=aNuHfIR-uX5kArK6" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Dans la mesure où je ne pouvais pas séquestrer le public pendant un mois pour tester et modifier le système avec elleux, j'ai dû mettre en place un environnement de test automatisé pour comprendre d'où ces variations de latence venaient. Dans la vidéo on voit le _jitter_ en action : certains curseurs font subitement une sorte de pause pendant que d'autres ont un déplacement fluide. Quand j'ai vu ça j'étais à la fois content (youpi j'ai réussi à répliquer le bug!) et dubitatif (pourquoi y'en a qui sont en bonne santé mais pas les autres alors que c'est le même code, les mêmes ordinateurs, les mêmes souris? huUuhH)

Long story short, le _jitter_ survenait parce que les raspberry étaient connectés ensemble en wi-fi, que j'utilisais un pauvre routeur sans antenne posé dans le fond de la salle, et que les corps humains dans la salle, qu'il faut considérer comme des gros sacs d'eau, ne sont pas gentils avec les ondes wifi (ils les bloquent). Les micro-ordinateurs répartis dans la salle avaient donc du mal a maintenir une connection stable avec le serveur, à part celui qui était juste à côté du routeur, ce qui explique pourquoi _quelques_ curseurs à l'écran ont un déplacement parfaitement fluide.

J'ai donc mis tous les rasps en connection filaire et le problème était 100% réglé. J'ai poussé un très gros soupir de soulagement parce qu'à l'époque j'étais convaincu à tort que le jitter était causé par le protocole de communication que j'utilisais entre les raspberry (_WebSockets_), et je n'avais pas du tout envie de taper dans la gamme au-dessus parce que les protocoles plus rapides que j'avais en tête sont compliqués (une personne m'a dit un jour à leur sujet : "attention, il faut savoir compiler du C"). Or je ne sais pas compiler du C, moi. Que la personne qui a déjà compilé du C me jette la première pierre.

## les baisses de framerate 🙀

Le dernier problème que j'ai rencontré est arrivé assez tard dans le projet. Je travaillais sur une séquence où chaque souris pouvait enfanter de nouvelles souris, saturant progressivement l'écran avec plein de petites souris. Ça n'a l'air de rien mais c'était beaucoup trop violent pour le navigateur web! Ou plutôt, c'était violent parce que c'était _codé_ d'une certaine façon qui n'était pas nécessairement optimisée. J'ai observé un ralentissement du framerate, c'est à dire que toutes les animations devenaient saccadées et pas fluides.

<iframe width="560" height="315" src="https://www.youtube.com/embed/tAeU1XIyIP8?si=C0JLlOEYWCuM64Pz" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
Là il n'y a pas de problème parce que je fais ça tout seul dans mon coin ; mais quand je l'ai fait en live pendant une sortie de résidence avec une vingtaine de personnes ça a tout pété

Ce phénomène survient parce que tout programme voulant afficher des choses sur un écran dispose d'un budget de 16,66 millisecondes pour faire tous ses calculs (si on veut que ça soit fluide et good-looking. Enfin, "fluide" dans la mesure où ce programme est affiché par un vidéoprojecteur ou un moniteur 60Hz, qui produit donc 60 images par seconde. Le budget est encore plus réduit si on désire produire des effets visuels au maximum des capacités d'affichage d'un moniteur 240Hz, qui produit 240 images par seconde).

Quand les calculs prennent plus de temps que 16,66 millisecondes, il vont bloquer l'affichage pendant 1 frame, ce qui veut dire que le navigateur produit maintenant 59 images / secondes, sniff. Si cette situation se répète ou que des calculs bloquent l'affichage pendant plusieurs frames, l'image peut devenir saccadée. En fait c'est exactement le même symptôme que le jitter, mais qui a des causes différentes! (Dans le cas du jitter, c'est la connection qui bug, alors que dans le cas qu'on est en train de décrire, il s'agit plutôt d'une surcharge des capacités de calcul de l'ordinateur). Levez la main si vous aimez les ordinateurs.

<!-- Comme on l'a dit plus haut, tout programme affichant des choses sur un écran dispose d'un budget de 16,66 millisecondes pour faire tous ses calculs (si on veut que ça soit fluide et good-looking. Enfin, "fluide" dans la mesure où ce programme est affiché par un vidéoprojecteur ou un moniteur 60Hz, qui produit donc 60 images par seconde. Le budget est encore plus réduit si on désire produire des effets visuels au maximum des capacités d'affichage d'un moniteur 240Hz par exemple). Quand un calcul prend plus de temps que 16,66 millisecondes, il va bloquer l'affichage pendant 1 frame, ce qui veut dire que le navigateur produit maintenant 59 images / secondes, sniff. Si cette situation se répète ou que des calculs bloquent l'affichage pendant plusieurs frames, l'image peut devenir saccadée. En fait c'est exactement le même symptôme que le jitter, mais qui a des causes différentes! (Dans le cas du jitter, c'est la connection qui bug, alors que dans le cas qu'on est en train de décrire, il s'agit plutôt d'une surcharge des capacités de calcul de l'ordinateur). Levez la main si vous aimez les ordinateurs. -->

![Dev Tools](/news/10/media/fps.png)
une vue des outils de performance de Google Chrome. Pour y accéder = F12, puis cliquez sur "performance". Sur l'image j'étais en train d'auditer une séquence assez chère en calculs où je fais tomber plein de CAPTCHAs du ciel. On voit des captures d'écran de ce que le navigateur affiche dans la deuxième ligne en partant du haut.

J'ai utilisé les outils de Google Chrome pour auditer mon code et essayer de comprendre si j'avais de la marge de manoeuvre pour le rendre plus efficace. Dans Chrome, on peut enregistrer une session de navigation, et exporter les données brutes au format json (c'est une sorte de gros fichier texte). J'ai collé ce fichier dans _gloups_ chatGPT pour qu'il l'analyse et me donne des pistes sur les parties les plus gourmandes en calculs de mon code. J'ai alors entrepris plusieurs optimisations, mais les gains en performance n'étaient pas suffisants et j'ai dû abandonner cette séquence (il aurait fallu prendre une approche trop différente et entièrement reprendre la base de code à partir de zéro, en utilisant d'autres langages et une logique toute autre).

En informatique il n'y a pas de solution parfaite, mais que des _trade-offs_, c'est à dire des options qui ont du pour et du contre ; il faut trouver un équilibre entre coût, complexité du code, performance, etc.

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
