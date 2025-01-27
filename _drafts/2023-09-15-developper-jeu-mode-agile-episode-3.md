---
layout: post
title: "Développer un jeu en mode agile - Épisode 3"
date: 2023-09-15 09:00:00 +0100
tags: article dev game-design
---

# Et maintenant... ?

Bon, en 45 minutes, j'ai un jeu _pierre, feuille, ciseaux_ qui fonctionne. C'est cool. Mais je fais quoi, maintenant&nbsp;?&nbsp;🤔

Le premier contrat que je m'étais fixé est rempli : un jeu solo, sur navigateur, où l'utilisateur joue contre une IA.

Je pourrais donc attaquer la création du jeu final, maintenant... non&nbsp;?

Non. Pas encore.

D'une part, la marche serait encore trop haute entre un simple _pierre, feuille, ciseaux_ (moche à souhait) et un jeu de style _Triple Triad_ pleinement fonctionnel. J'ai besoin d'étapes intermédiaires pour ne pas risquer le découragement.

D'autre part, il me manque encore un élément essentiel&nbsp;!

Ok, j'ai créé un jeu sur navigateur. Mais qui peut y jouer, pour le moment&nbsp;?

Uniquement moi, en fait&nbsp;! Et à condition d'avoir un environnement de développement ouvert et de lancer un serveur local avec `vite` (l'outil de build de Vue.js) pour servir le contenu web. Ce jeu ne remplit pas une partie du contrat : il n'est pas disponible en ligne, utilisable par n'importe qui.

Et c'est une condition importante. Car il n'y a rien de plus démotivant que de travailler sur un projet seul, sans _feedback_. Lorsque je commencerai à avoir quelque chose d'utilisable, je veux que des joueurs y jouent&nbsp;! Je veux qu'ils me disent si c'est bien, si ça avance dans la bonne direction, qu'ils me donnent des idées. Ne serait-ce qu'un petit message «&nbsp;_Eh, c'est prometteur ton jeu. Préviens-moi quand tu auras avancé_&nbsp;».

Je pourrais me dire «&nbsp;_ça va, tu as le temps&nbsp;! Tu t'occuperas de ça quand tu auras une première version viable_&nbsp;». C'est une idée... Mais non&nbsp;! Par expérience, je sais que s'occuper de ce genre de "détail" à la fin d'un projet est une mauvaise idée. Je vais donc tacler cette question dès maintenant.

# Hébergement sur GitHub Pages

Je sais que GitHub propose des hébergements de sites web gratuits (front uniquement), pour des pages persos, des CV, des sites vitrines d'entreprise.

Je vais donc faire un tour sur la [documentation officielle](https://pages.github.com/).

Il n'y a vraiment pas besoin de grand chose&nbsp;!&nbsp;🙂

- Je me crée une adresse Gmail dédiée au jeu (ça me servira le jour où je voudrai collecter des feedbacks ou assurer un support)
- Avec cette adresse, je crée un compte GitHub dédié
- Je crée un dépôt avec le nom adapté
- J'y dépose la version distribuable de mon application Vue.js

Et, tada&nbsp;!!

![](/assets/images/pokemon-triad//rock-paper-scissors-online.jpg)

J'ai un nom de domaine `pokemon-triad.github.io` prêt à l'emploi. Et mon jeu temporaire est accessible dessus, par n'importe qui possédant un navigateur web et une connexion Internet.&nbsp;😎

> Quantité de travail : 30' (total : 1h15)

# Tic tac toe !

Maintenant que je suis en mesure de déployer très facilement le jeu en ligne, je me repenche sur la _marche_ suivante. Admettons que je ne m'attaque pas directement au _Triple Triad_. Comment identifier l'étape suivante&nbsp;?

Je fais le bilan de ce que j'ai déjà : un jeu à 2 joueurs, avec un gagnant et un perdant. Mais il ne se joue qu'en une seule action&nbsp;! Un unique choix de l'utilisateur et le jeu est déjà terminé. Dans le _Triple Triad_, les joueurs jouent chacun à leur tour, jusqu'à ce que l'ensemble des cartes soient posées. Et l'issue du jeu n'est calculée qu'à la fin.

Quel jeu, plus simple que le _Triple Triad_, possède un mode de fonctionnement similaire&nbsp;?

Très vite, je pense au _Morpion_ (_Tic tac toe_ en anglais).

Coder ce jeu nécessiterait&nbsp;:

- de passer d'un tour unique, simultané pour les 2 joueurs, à un jeu en plusieurs tours, avec alternance entre les deux joueurs
- de développer une IA plus "intelligente", qui réalise un choix à partir de données d'entrées (l'état actuel de la grille de morpion)
- de détecter la fin de la partie lorsque certaines conditions sont remplies, et d'en déterminer l'issue (victoire, défaite ou égalité)

J'aurai de toute façon besoin de tout cela dans le jeu final. Donc cela me paraît être une bonne _prochaine étape_. Go&nbsp;!&nbsp;🔥

🕐... 🕑... 🕒...

> Quantité de travail : ~4h (total : 5h15)

Principale difficulté rencontrée, qui me paraît évidente maintenant mais que je n'avais pas identifiée dans la liste précédente&nbsp;: la notion d'asynchrone.

Lorsque je lance le jeu via ma classe `Game`, je ne peux pas dérouler les tours de jeu un par un dans une bête boucle `for`. Car un de mes deux joueurs interagit via une IHM&nbsp;! Par conséquent, le programme doit _attendre_ l'action de ce joueur pour pouvoir se dérouler (problème que je n'avais pas pour le _pierre, feuille, ciseaux_ car il n'y a qu'un seul tour de jeu).

J'ai donc ressorti les `Promise`, ce qui entraîne généralement un petit délai de réalisation... 😅.

J'aurais pu "tricher" en pilotant l'exécution des tours de jeu depuis l'extérieur de la classe `Game`. Mais cela m'aurait probablement amené à piloter le jeu depuis la couche de présentation, ce qui n'est pas du tout la philosophie que je souhaite appliquer.

J'y ai passé un peu plus de temps que je ne l'espérais, mais ça fonctionne&nbsp;:

![](/assets/images/pokemon-triad/tic-tac-toe.jpg)

Bon, j'ai fait des concessions sur l'intelligence de l'IA : elle joue au hasard 🙄. Mais en voyant les heures de travail s'écouler, je me suis dit que développer une IA plus performante pour un jeu jetable (parce qu'on n'est toujours pas à la cible _Triple Triad_&nbsp;!) était une perte de temps.

J'ai donc un jeu en ligne, où un utilisateur peut jouer en plusieurs tours contre une IA : objectif atteint&nbsp;!&nbsp;✅
