---
layout: post
title: "Développer un jeu en mode agile - Épisode 8 : MVP"
date: 2024-05-29 09:00:00 +0200
tags: article dev game-design
category: pokemon-triad
---

# Minimum Viable Product&nbsp;!

Suite à la constitution du backlog pour le MVP, 4 mois se sont écoulés, sans que je ne touche à la moindre ligne de code&nbsp;🙁. Moins de motivation, beaucoup d'autres choses à faire, à la maison comme au travail. La période des fêtes de fin d'année a contribué à cette longue pause.

Mais j'ai tout de même fini par m'y remettre. Et pas à moitié&nbsp;!

Et j'ai une super nouvelle&nbsp;: après environ 2 mois de reprise d'activité sur ce projet, j'ai un MVP&nbsp;!&nbsp;🤯

Je vous raconte...

# Fonctionnalités ajoutées

Voici toutes les fonctionnalités sur lesquelles j'ai travaillé ces 2 derniers mois&nbsp;:

- Quelques **améliorations mineures de l'interface utilisateur**, pour rendre l'application plus agréable à l'oeil et ergonomique. C'est toujours un design très minimaliste car, clairement, je ne suis pas _designer_ 😅. Mais ça me convient pour le moment.

- J'ai ajouté encore pas mal de contenu : des **nouvelles zones**, des **nouvelles cartes**. Et j'ai partiellement industrialisé l'ajout de contenu, ce qui me permettra d'être plus efficace à l'avenir (rien que dans les premières générations du jeu, il y a 251 pokémons, répartis dans des dizaines de zones différentes... si j'y passe ne serait-ce 15 min pour chaque, ça fait... _beaucoup trop&nbsp;!!!_).

- Un écran pour **visualiser sa collection de cartes**&nbsp;! C'est encore loin d'être parfait, et on pourrait y ajouter plein de fonctionnalités. Mais il a le mérite d'exister.

- Un **menu** dans l'application, permettant de basculer entre différents écrans&nbsp;: l'aventure (sélection des zones + parties), la collection du joueur, une page d'information à propos du jeu.

- Une **IA renforcée**, qui joue un peu plus intelligemment. J'ai conservé la précédente pour les premiers niveaux du jeu, mais rapidement c'est la nouvelle qui prend le relai, ce qui rend le jeu un peu plus difficile (et donc plus intéressant à mes yeux).

- Ajout d'une **rareté** sur les cartes. Ce n'était pas prévu dans mon backlog initial. Sur ce coup-là, je me suis juste fait plaisir, parce que ça me tentait à ce moment-là&nbsp;😁.

- Le **déblocage des zones** au fur et à mesure de la progression du joueur, plutôt que de les avoir toutes disponibles au début. Cela force une progression lente de la difficulté, et ça donne un peu de rythme à l'expérience de jeu.

- Et, enfin, un **écran de tutoriel**, qui explique les règles de jeu&nbsp;! Parce que ça manquait sérieusement.

En plus de toutes de évolutions _produit_, j'ai également passé beaucoup de temps sur du refactoring, de l'amélioration technique de certains composants, et la mise à jour des dépendances du projet.

Pour être honnête, j'ai complètement perdu la trace du temps passé sur le projet...&nbsp;🙁 C'est dommage car je n'aurai plus moyen de savoir combien de temps j'y ai passé en tout. L'information m'aurait intéressée.

Plutôt que de vous publier des captures d'écrans additionnelles, je vous invite à aller voir à quoi ressemble le jeu désormais -> [Pokémon Triad](https://pokemontriad.github.io). Sur mobile, de préférence&nbsp;: l'expérience joueur y est plus intéressante.

# Quelques constats intéressants

## L'agilité VS la planification

Les plus courageu(x/ses) d'entre vous pourront s'amuser à comparer la liste de fonctionnalités ci-dessus avec mon backlog de l'épisode précédent.

Je vous _sploil_ le constat&nbsp;: la liste n'est pas identique.&nbsp;😊

Certaines des fonctionnalités qui me paraîssaient essentielles à l'époque n'ont pas été implémentées. D'autres, non prévues, ont été ajoutées en cours de route.

Et c'est là, selon moi que réside la grande **force de l'agilité**&nbsp;! Plutôt que de suivre un plan, une liste de fonctionnalités à réaliser, je me suis posé la question à chaque instant&nbsp;: **qu'est-ce qui a le plus de sens, _maintenant_&nbsp;?** Que puis-je ajouter _maintenant_ pour maximiser l'expérience utilisateur ou apporter de la valeur au projet&nbsp;? Sachant que _conserver ma motivation_ est une valeur en soit dans ce projet. Donc, parfois, implémenter une fonctionnalité qui ne servait à rien, mais qui me faisait plaisir, était la priorité du moment.

Mon backlog n'est finalement qu'un pense-bête, un aide-mémoire dans lequel je stocke des idées. Mais je le repriorise en permanence. Et si, à un moment donné, ce qui a le plus de sens à mes yeux c'est une fonctionnalité qui n'est pas tracée dedans, tant pis pour le backlog, je la fais quand même&nbsp;!

Et je comprends maintenant parfaitement qu'on puisse vouloir travailler en agilité sans _sprints_.

## La force des _petites_ fonctionnalités

Je reviens toujours au fait que découper les tâches de façon à **limiter la durée d'une évolution** est vraiment capital pour avancer vite&nbsp;! Les fameux _baby steps_...

Je fais toujours en sorte que mes ajouts de fonctionnalités soient **réalisables en moins de 4h**. Idéalement, même 2 ou 3h. Quitte à ce qu'une fonctionnalité soit initialement implémentée en mode dégradé (fonctionnelle et utilisable, mais pas forcément au top de ce qu'elle pourrait être), et améliorée par la suite.

Au-delà de 3h, j'arrive rarement à réaliser l'ensemble en une seule session. Et reprendre une fonctionnalité _en travaux_ 🚧 après plusieurs jours, ce n'est pas très motivant. Donc ça traîne...

## Une application fonctionnelle à tout moment

L'application est "en prod", au sens où elle est **accessible en ligne**, depuis n'importe quel navigateur, **depuis le tout premier jour**.

Le fait de voir les choses avancer, de pouvoir systématiquement déployer et utiliser le résultat sur mon téléphone est **incroyablement stimulant**&nbsp;! Je vois immédiatement le résultat. Je peux m'amuser avec l'application, la tester un peu en conditions réelles. Je peux également la montrer à d'autres personnes, pour avoir leur opinion, voir s'ils arrivent à l'utiliser (ce qui m'a très rapidement amené à constater que le tutoriel était indispensable pour le MVP&nbsp;😅).

Utiliser moi-même l'application en situation réelle fait que j'ai très rapidement d'autres idées d'améliorations (je suis mon propre _feedback_ utilisateur, en quelques sorte).

J'alterne entre des phases où&nbsp;:

- je remplis mon backlog de toute les idées d'amélioration qui me viennent
- je priorise l'existant&nbsp;: qu'est-ce qui apportera le plus de valeur à un joueur&nbsp;?
- je traite les tâches prioritaires
- je déploie

Et j'itère comme cela, en circuit court. C'est très confortable comme façon de travailler.

# Quelle suite ?

Bon, ça y est, j'ai ma première version. Le jeu fonctionne, je peux le montrer, des utilisateurs peuvent commencer à le découvrir.

Et maintenant&nbsp;?&nbsp;🤔

Personnellement, je suis sur une lancée assez active niveau dev, donc **je n'ai pas envie de m'arrêter là**. J'ai déjà identifié dans le backlog une dizaine de fonctionnalités qui constituraient une bonne version suivante.

Mais je commence à ressentir l'envie de voir le jeu utilisé. Et, pour le moment, je n'ai pas l'impression que ce soit franchement le cas. J'ai mis de côté la mise en place de l'analyse d'audience, parce que je me suis perdu dans les méandres de Google Tag Manager (pour faire simple, je n'ai rien compris... ça me semblait plus simple à mettre en oeuvre il y a 15 ans). Et j'ai renoncé à la publication sur les réseaux sociaux car j'ai appris d'un ami que _The Pokémon Company_ avait une politique très agressive concernant les droits d'auteur. Je n'ai pas franchement envie qu'ils me repèrent&nbsp;😅. Même si je n'ai aucune intention de commercialiser le jeu, car c'est principalement dans un but de développement personnel que je l'ai créé.

Pourtant, **j'aimerais avoir des retours de vrais utilisateurs&nbsp;!**

Je n'ai pas de solution pour le moment, mais il va falloir que je réfléchisse à la façon de diffuser le lien du site tout en passant sous les radars.

Il pourrait aussi être intéressant de **trouver un(e) graphiste ou illustratrice** pour travailler en binôme avec moi sur le design du site, qui est très largement améliorable. Mais, là encore : comment trouver&nbsp;? Par quel canal de communication passer&nbsp;?

Et enfin, il faudrait que je travaille un peu sur l'**automatisation du process de versionnement et de déploiement de l'application**. Maintenant qu'une version 1 va être publiée, et en particulier si je commence à diffuser le lien, il faudra que je sois plus rigoureux et méthodique dans la publication de nouvelles fonctionnalités.

# Objectif atteint

Dans tous les cas, **l'objectif initial de cette aventure est atteint**&nbsp;: avec les bonnes méthodes de travail, sur un sujet qui me plaît, je suis capable d'investir suffisamment de temps et d'énergie (même en parallèle d'une activité professionnelle soutenue), pour créer un jeu.&nbsp;✅

À lui seul, ce constat m'apporte déjà énormément&nbsp;!

Cela ouvre peut-être la voie à un autre projet, plus ambitieux, un jour. Ou simplement à poursuivre celui-ci pour le moment.

La suite, peut-être, au prochain épisode...&nbsp;😉
