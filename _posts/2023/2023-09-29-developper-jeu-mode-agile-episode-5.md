---
layout: post
title: "Développer un jeu en mode agile - Épisode 5"
date: 2023-09-29 09:00:00 +0100
tags: article dev game-design
---

# Et maintenant ? (_again_)

À nouveau, je me retrouve devant ce dilemme : que faire ensuite&nbsp;?

Si je reprends la liste des améliorations identifiées précédemment, il me reste&nbsp;:

- Marquer un temps d'attente artificiel lorsque l'IA joue, pour permettre une meilleure lecture du jeu (actuellement, l'aspect instantané est perturbant).

- Créer un effet visuel de retournement des cartes, au lieu d'un simple changement de couleur comme actuellement. Idéalement, un effet visuel de pose serait pas mal aussi.

Deux améliorations dont je sais déjà qu'elles peuvent me prendre pas mal de temps. Parce qu'elles sont liées à de la synchronisation entre l'exécution du code et l'affichage, et potentiellement l'introduction d'une notion d'asynchrone dans l'histoire. Par expérience, je sais que ça sent les ennuis... D'autant plus que les animations CSS n'est pas mon domaine de prédilection.

L'introduction du thème m'a également mis en tête deux évolutions intéressantes&nbsp;:

- La possibilité d'avoir une collection de cartes, d'en choisir cinq au début de chaque partie. Et de pouvoir _capturer_ des cartes supplémentaires en cas de victoire, pour enrichir sa collection.

- Introduire la notion de type/élément, très présente dans les jeux originaux, qui viendraient appliquer des modificateurs sur les valeurs des cartes. Cela rendrait le gameplay un peu moins monotone/prédictible.

**_Comment choisir entre ces 4 tâches ?_**&nbsp;🤔

Eh bien il faut les **prioriser**&nbsp;! Quelle valeur apportent-elles chacune&nbsp;? Et quel est l'effort nécessaire pour les développer&nbsp;?

| Fonctionnalité                      | Apport au gameplay | Effort de dev nécessaire |
| ----------------------------------- | :----------------: | :----------------------: |
| Temps d'attente entre les tours     |         ➕         |            ➕            |
| Effets visuels de retournement/pose |         ➕         |           ➕➕           |
| Collection de cartes                |       ➕➕➕       |        ➕➕➕➕➕        |
| Types                               |        ➕➕        |           ➕➕           |

En réfléchissant à la construction de ce tableau, j'ai identifié une solution très simple pour la première amélioration&nbsp;: introduire le **temps d'attente** du côté de l'IA, vu que les interactions entre le moteur de jeu et les joueurs sont déjà gérées avec des promesses. C'est un peu de la triche, mais ça a le mérite d'être ultra-rapide à mettre en oeuvre&nbsp;😋.

La **collection de cartes** est clairement ce qui apporterait le plus au _gameplay_, mais la quantité de travail est très importante&nbsp;: gérer un état persistant entre 2 parties, créer un nouveau composant pour que l'utilisateur puisse choisir ses cartes, coordonner ce nouveau composant avec l'actuel composant qui permet d'afficher le déroulement d'une partie...

Les **effets visuels** apporteraient finalement assez peu de valeur à côté du reste, car le jeu reste tout de même jouable sans.

Les **types** apporteraient une nouvelle dimension au gameplay, sans être trop long ou difficiles à introduire.

Par conséquent, les deux prochaines fonctionnalités à développer sont, dans cet ordre&nbsp;:

- Le temps d'attente
- Les types

Et voilà comment on priorise des tâches de façon rationnelle&nbsp;!&nbsp;😉

➡ GO ! 🔥

🕐... 🕑... 🕒...

- Délai artificiel entre deux tours de jeu : `30'`
- Amélioration de l'interface graphique (tâche improvisée en cours de route 😅)&nbsp;: `30'`
- Les types&nbsp;: `4h` 😣

> Quantité de travail totale : **22h** depuis la première ligne de code.

Le délai entre les tours était effectivement rapide à développer. Et il apporte ÉNORMÉMENT à la lisibilité du jeu&nbsp;! Je n'avais pas imaginé à quel point. Il pourrait manquer quelques améliorations&nbsp;: un spinner d'attente pour matérialiser encore plus le tour de jeu adverse, des messages éphémères indiquant «&nbsp;À vous de jouer !&nbsp;», par exemple. Mais le gain est déjà excellent avec simplement 1,5s de délai avant que l'IA ne joue (valeur choisie arbitrairement après plusieurs essais).

Je me suis improvisé une petite tâche courte d'amélioration de l'IHM avant d'attaquer les types (et pas les _éléments_ comme j'avais appelé ça précédemment. Je suis rouillé... En même temps, les jeux originaux, ça fait 25 ans&nbsp;!).

Et enfin, les _types_&nbsp;! Et là, ça a été un peu plus éprouvant. J'ai quasiment fait les 4h d'une traite (en me couchant beaucoup trop tard&nbsp;😩). Tout cela regroupe&nbsp;:

- La **conception** : comprendre comment fonctionnaient ces types dans les jeux originaux, trouver comment l'adapter à ma version _Triple Triad_ (pas simple... j'ai tenté un truc : on verra bien&nbsp;!).

- La partie "**moteur de jeu**" : implémenter la comparaison des types pour déterminer des modificateurs de valeurs, puis faire en sorte que ces modificateurs soient pris en compte dans les calculs de retournement lors de la pose d'une carte.

- Enfin, la partie "**UI**" : représenter les modificateurs sur les valeurs (pas entièrement satisfait du résultat) et afficher les types des différents Pokémons pour que l'utilisateur puisse faire un choix éclairé (pas du tout satisfait du résultat&nbsp;! J'ai essayé de bricoler quelque chose avec des pictogrammes, mais j'ai fini par laisser tomber parce que je ne m'en sortais pas avec la transparence des images. Je me suis contenté de ces bulles de couleur, mais ce n'est pas beau. Il faudra que je trouve un moyen d'arranger ça).

![](/assets/images/pokemon-triad/pokemon-triad-types.png)

# Une très haute marche à venir&nbsp;😨

Si je regarde ce qu'il reste comme amélioration imminente&nbsp;:

| Fonctionnalité                      | Apport au gameplay | Effort de dev nécessaire |
| ----------------------------------- | :----------------: | :----------------------: |
| Effets visuels de retournement/pose |         ➖         |           ➕➕           |
| Collection de cartes                |       ➕➕➕       |        ➕➕➕➕➕        |

Maintenant que le délai d'attente a été implémenté, le gain des effets visuels de pose et de retournement des cartes est devenu très minime. Je pense que ce n'est pas intéressant d'y passer du temps maintenant.

Il me reste donc... gérer une collection de cartes pour pouvoir enchaîner plusieurs parties, et ainsi enrichir sa collection.

Mais vu le temps que j'ai passé sur les types, ce bloc me fait très peur&nbsp;😬. Je ne le sens pas. La marche est trop haute, je suis fatigué et j'ai peu de temps libre. J'ai vraiment peur que ça traîne pendant trop longtemps...

Je vais donc, pour le moment, essayer de réaliser quelques améliorations graphiques. Histoire de rendre le jeu un peu plus présentable, bien que ça ne soit pas mon domaine de prédilection.

Ça fait un peu "refus d'obstacle", dit comme ça. Mais "_qui veut aller loin ménage sa monture_"&nbsp;!

🕐...

Et voilà ce que ça donne après `1h` de boulot.

![](/assets/images/pokemon-triad/pokemon-triad-ui-upgrade.png)

On n'est pas encore sur de l'appli mobile professionnelle (je pense qu'à un moment, si j'arrive à amener le gameplay à un niveau suffisant pour commencer à diffuser le lien du jeu à droite à gauche, il faudra que je me fasse aider sur le design. Clairement, je bricole dans ce domaine...), mais je suis satisfait du gain pour le temps passé&nbsp;!

> Quantité de travail totale : 23h

<a class="navigation next" href="{% link _posts/2023/2023-10-30-developper-jeu-mode-agile-episode-6.md %}">Développer un jeu en mode agile - Épisode 6</a>
