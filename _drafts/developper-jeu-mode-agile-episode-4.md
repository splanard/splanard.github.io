---
layout: post
title: "Développer un jeu en mode agile - Épisode 4"
date: 2025-01-01 09:00:00 +0100
tags: article dev game-design
---

# On attaque les choses sérieuses !

La question de la prochaine étape se pose à nouveau. Et, à nouveau, la meilleure façon d'y répondre est de commencer par faire le bilan de ce que j'ai&nbsp;:

- un jeu à 2 joueurs, au tour par tour
- un des joueurs est piloté par une IA, l'autre par un utilisateur à travers une IHM web
- le jeu se déroule sur un plateau constitué d'une grille 3x3
- Le déroulement du jeu est asynchrone : à chaque tour, le moteur du jeu attend l'action du joueur piloté via l'IHM pour poursuivre son exécution.

En faisant ce bilan, je m'aperçois qu'on se rapproche déjà beaucoup du _Triple Triad_&nbsp;!

**La prochaine étape pourrait donc être de coder le moteur du jeu _Triple Triad_**. Et pour limiter la complexité, chaque joueur se verra proposé 5 cartes aléatoires en début de partie. Cela m'évitera de devoir gérer la complexité d'un choix de cartes parmi une collection, pour le moment.

Je sais déjà qu'il y a pas mal de travail : modélisation des cartes, effet de retournement lors de la pose, calcul du score, etc. Donc au boulot&nbsp;!&nbsp;🔥

🕐... 🕑... 🕒...

> Quantité de travail : moteur du jeu ~3h + affichage ~3h (total&nbsp;: ~11h. Oui, je laisse tomber les 15' qui traînent depuis un moment... Au bout de 11h, on n'est plus à 15' près&nbsp;!)

Je n'avais pas anticipé que passer d'un morpion au _Triple Triad_ me prendrait autant de temps...

Pour le moment, je n'ai pas fait l'effort de créer des cartes originales&nbsp;: je me suis contenté de reprendre les valeurs figurant sur les 11 cartes de niveau 1 du jeu original dans _Final Fantasy VIII_ (merci les Wiki qui traînent depuis 20 ans 😅).

Ce qui m'a pris le plus de temps&nbsp;:

- Pour le moteur de jeu, simuler des parties pour les tests unitaires. Je ne suis d'ailleurs pas très satisfait de la lisibilité du résultat. Il faudra que j'améliore ça.
- Pour l'affichage, l'agencement des cartes sur l'écran, afin qu'on puisse voir à la fois le plateau et les mains des 2 joueurs sur le même écran. Faire en sorte que les cartes restent bien carrées ET responsives m'a également retardé un peu.

J'ai à nouveau fait des concessions : mon IA joue toujours au hasard, par exemple&nbsp;😫. Mais, après 6h de travail, il fallait que je sorte quelque chose...

Il y a énormément de choses améliorables&nbsp;! J'ai déjà en tête une liste conséquente. Entre autres&nbsp;:

- Améliorer l'affichage des cartes qui ne rend pas très bien sur mobile (chiffres trop gros).
- Rendre l'IA adverse plus intelligente.
- Ré-équilibrer les points des cartes car, selon la distribution initiale, heureusement que l'IA joue n'importe comment. Sinon je n'aurais aucune chance de gagner.
- Intégrer une image sur chaque carte. Et donc en profiter pour introduire enfin le thème du jeu...&nbsp;🙄
- Marquer un temps d'attente artificiel lorsque l'IA joue, pour permettre une meilleure lecture du jeu (actuellement, l'aspect instantané est perturbant).
- Créer un effet visuel de retournement des cartes, au lieu d'un simple changement de couleur comme actuellement. Idéalement, un effet visuel de pose serait pas mal aussi.
- Déterminer aléatoirement le joueur qui joue le premier. Actuellement, c'est toujours l'utilisateur, ce qui peut déséquilibrer le jeu.
- Améliorer certaines parties du code dont je ne suis pas satisfait.

Bref... la liste est longue.&nbsp;😓

**MAIS**, malgré tout cela, **j'ai un jeu _Triple Triad_ fonctionnel !** Le contrat de cette étape est rempli ✅ (avec, une fois de plus, certaines concessions. Mais c'est le quotidien du développement agile). J'ai gravi une marche de plus de l'immense escalier dont j'ai encore du mal à voir le sommet.

Gros avantage, le jeu est utilisable sur mobile&nbsp;!

![](/assets/images/pokemon-triad/triple-triad-mobile-1.png)

![](/assets/images/pokemon-triad/triple-triad-mobile-2.png)

Cela me permet de le tester très facilement, n'importe quand, depuis n'importe où. C'est donc une fonctionnalité que je vais prendre soin de conserver lors des prochaines évolutions.

# Quelques améliorations

La précédente marche était haute. 😰

6h de travail, dans un contexte professionnel, ce n'est pas grand chose. À peine une journée de travail.

Sauf que, dans ce cas, je ne suis pas dans un contexte professionnel. Chaque heure de développement passée sur ce jeu, je la fais en plus de ma journée pro&nbsp;: parfois sur ma pause déjeuner, mais le plus souvent le soir/la nuit. Donc, selon ma motivation et les contraintes à la maison, 6h peuvent s'étaler sur une semaine...

Une semaine pour voir quelque chose de nouveau (et pas transcendant qui plus est 😅), c'est long&nbsp;! Le découragement peut se pointer à l'improviste, n'importe quand. Un petit _coup de flemme_ suffit à lui ouvrir la porte&nbsp;😈.

Je décide donc que les étapes suivantes seront petites. Les améliorations possibles ne manquent pas (voir plus haut). Je pioche donc dedans celles qui me plaisent le plus et/ou me prennent le moins de temps. Le but est de me re-booster en livrant de la valeur très rapidement.

🕐... 🕑... 🕒...

Et voilà le résultat&nbsp;:

- Quelques améliorations techniques (Clean Code, refactoring)&nbsp;: `2 sessions de 1h`
- Légère amélioration de l'UI (affichage des cartes, identification des "mains" des 2 joueurs, ajout d'un titre)&nbsp;: `1h`
- Ajout du thème&nbsp;: `2h`
- Création d'un set de cartes _starter_ pour l'utilisateur et d'un set de cartes aléatoire pour l'IA&nbsp;: `30'`
- Amélioration de l'IA&nbsp;: `1h`
- Déterminer aléatoirement le joueur qui joue le premier&nbsp;: `5'`

Soit `+6h35` de travail (on va dire 6h, hein... mais morcelées, avec des déploiements intermédiaires du résultat), pour un **total de `~17h`** depuis le début du projet.

Et après ça, l'objectif est atteint : je suis complètement re-motivé&nbsp;!!&nbsp;✅💪

Déjà, l'introduction du thème m'a replongé dans mes souvenirs d'enfance (Pokémon rouge/bleu, ça commence à dater un peu... 🙄). Étant donc un vieux schnock, j'ai choisi d'introduire les pokémons de première génération&nbsp;! Merci Poképédia, dont j'ai honteusement pompé les images (et ce n'est que le début...).

Mais ça commence à ressembler à quelque chose&nbsp;!

![](/assets/images/pokemon-triad/pokemon-triad.png)

Hein&nbsp;? Oui...&nbsp;?

Bon, OK. On est encore bien loin de la cible que j'ai en tête. Surtout que, depuis que je me suis replongé dans les descriptifs des jeux originaux, que je me suis rappelé des personnages partiellement oubliés, les idées de systèmes de jeu et de scénarisation fusent dans ma tête à la moindre occasion.&nbsp;✨

Mais ça progresse. Et c'est ça qui compte&nbsp;!

L'IA adverse n'est toujours pas d'une efficacité exceptionnelle, mais elle ne joue plus complètement au hasard. Elle calcule le nombre de cartes qu'elle est capable de retourner avec chacune des cartes qu'elle a encore en main. Puis elle choisit la carte et l'emplacement qui lui permettent de retourner un maximum de cartes advserses. Bon, à nombre de retournements égal, c'est du hasard... J'améliorerai encore cela à l'avenir.

Mais pas trop&nbsp;!! Car **pour une expérience utilisateur réussie, il faut des IA avec des failles&nbsp;!** Pour que le joueur prenne satisfaction à gagner des parties. Si je lui colle dès les premières parties une IA qui fait des prévisions de jeu sur plusieurs tours et optimise la pose pour cacher les valeurs faibles contre les bords, le joueur va se prendre raclée sur raclée jusqu'à l'en dégoûter du jeu&nbsp;😅. L'expérience utilisateur s'arrêterait alors assez rapidement... Il est donc fort probable que je garde cette première IA (avec peut-être de très légères améliorations) pour les premiers niveaux de difficulté du jeu.

D'autant plus que, selon le set de cartes que reçoit l'adversaire, c'est déjà suffisamment compliqué de gagner comme ça&nbsp;! 😓
