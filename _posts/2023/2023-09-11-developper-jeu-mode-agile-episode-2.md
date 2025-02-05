---
layout: post
title: "Développer un jeu en mode agile - Épisode 2 : Par où commencer ?"
date: 2023-09-11 09:00:00 +0200
tags: article dev game-design
category: pokemon-triad
---

# Par où commencer ?

Dès lors que j'ai décidé du thème de ce projet de mise à l'épreuve, les idées fusent dans ma tête. Plus le temps passe et plus je suis convaincu qu'il y a une idée exploitable en termes de _gameplay_.

J'imagine tout ce que je vais pouvoir/devoir implémenter&nbsp;:

- la collection de cartes
- le système de jeu pour les affrontements
- une montée de niveaux
- une scénarisation, pour donner un peu de contexte au jeu
- ...

Hop hop hop ! On se calme !! ✋🛑

Je dois me souvenir que la démotivation et l'égarement sont mes ennemis.

Mon expérience professionnelle me l'a démontrée&nbsp;: pour aller loin, il faut procéder par étapes. Par **_petites_** étapes, de préférence. Et avec ma liste de tâches ci-dessus, je pars déjà beaucoup trop loin. ❌

Donc, je reprends... quelles sont **_réellement_** les premières étapes de mon projet&nbsp;?

- Un jeu à 2 joueurs.
- Un gagnant, un perdant. Donc des conditions de victoire.
- Comme je ne veux pas me lancer dans un mode multi-joueurs pour le moment (beaucoup trop d'embêtements, je le sens d'avance sans avoir besoin de chercher), ce sera un jeu solo. Donc l'adversaire devra être une IA.

Quel est le jeu le plus simple que je connaisse qui implique 2 joueurs&nbsp;?

_Pierre, feuille, ciseaux_&nbsp;!

Voilà quelque chose de plus raisonnable. La première chose que je dois développer est donc un jeu "_pierre, feuille, ciseaux_", avec un joueur contrôlé par l'utilisateur et l'autre piloté par une IA (au moins, l'IA ne sera pas trop difficile à développer&nbsp;😝).

> Quantité de travail : difficile à évaluer

_Là encore, cette réflexion s'est faite petit à petit depuis que l'idée m'est venue. À ce moment-là, je n'ai encore pas écrit la moindre ligne de code._

# Pierre, feuille, ciseaux !

Je veux aller vite, je veux faire simple. Je vais donc utiliser des outils et technos que je connais et maîtrise déjà.&nbsp;👍

Il se trouve que je suis développeur web, donc le jeu prendra la forme d'un site web.

Pour le moment, je n'ai aucun intérêt à avoir un serveur, une base de données, etc. Je veux simplement créer un mini-jeu qui soit utilisable en ligne. Je n'ai donc besoin que d'un site purement web&nbsp;: HTML, JavaScript, CSS.

Et il se trouve que je maîtrise bien le framework Vue.js. J'ai l'habitude de travailler avec, donc je ne perdrai pas de temps sur des questions "_comment faire&nbsp;?_" et je pourrai me focaliser sur "_quoi faire&nbsp;?_".

C'est parti&nbsp;!&nbsp;🔥

Il y a quelques mois, je m'étais créé un starter Vue.js 3 (avec TypeScript)&nbsp;: un projet déjà pré-configuré selon mes préférences, prêt à l'emploi. J'exporte cette coquille vide dans mon tout nouveau projet `pokemon-triad`, je mets rapidement à jour les dépendances obsolètes.

Voilà, j'ai un projet tout neuf, prêt à démarrer le "vrai" travail&nbsp;: l'implémentation du jeu _pierre, feuille, ciseaux_ !

> Quantité de travail : 15 min

J'attaque ensuite le développement du jeu en lui-même.

Je ne perds pas de temps sur les graphismes pour le moment&nbsp;:

- D'une part, ce jeu est une étape très temporaire&nbsp;: le jeu _pierre, feuille, ciseaux_ n'est pas du tout l'objectif final. Donc peu importe si le design est minimaliste. L'important est d'avoir quelque chose qui fonctionne, rapidement.

- D'autre part, je préfère me focaliser d'abord sur le système de jeu, puis retravailler les graphismes. Un jeu moche, qui fonctionne, on peut s'en servir. Une interface graphique jolie mais qui ne fait rien... eh bah ça ne sert à rien&nbsp;!

Par habitude, je sépare la partie `domain` de la partie `ui`. La structure de mon projet ressemble donc rapidement à ceci&nbsp;:

```text
src
  domain
    Game.ts
    Player.ts
    AIControlledPlayer.ts
    UIControlledPlayer.ts
    ...
  ui
    components
      RockPaperScissors.vue <-- mes composants Vue.js iront ici
      ...
    App.vue
    main.ts
```

Tout ce qui se trouve dans `domain` représente la logique de mon jeu, la structure des notions manipulées, etc. Tout y est complètement indépendant de l'affichage. Il n'y a d'ailleurs dans ce code aucune référence quelconque à Vue.js, et c'est voulu. Si demain je veux changer de framework, et utiliser React ou Angular par exemple, ce qui se trouve dans `domain` est entièrement réutilisable.

Dans `ui`, je stockerai tout ce qui concernera l'affichage&nbsp;: les composants Vue.js, principalement.

Assez vite, j'arrive à quelque chose d'utilisable.

![](/assets/images/pokemon-triad/rock-paper-scissors-1.jpg)

Si je clique sur le bouton "Scissors".

![](/assets/images/pokemon-triad/rock-paper-scissors-2.jpg)

Cool, j'ai gagné&nbsp;! Si si, j'ai gagné du premier coup... 🙄

Et non, pour la pierre, je n'ai rien trouvé d'autre dans la bibliothèque d'emojis standards qu'une statue de l'île de Pâques... 😑

J'ai un jeu, il fonctionne&nbsp;: premier objectif atteint&nbsp;! ✅

> Quantité de travail : 30' (total : 45')

<a class="navigation next" href="{% link _posts/2023/2023-09-15-developper-jeu-mode-agile-episode-3.md %}">Lire la suite</a>
