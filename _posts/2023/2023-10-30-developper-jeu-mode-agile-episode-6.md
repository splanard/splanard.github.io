---
layout: post
title: "Développer un jeu en mode agile - Épisode 6"
date: 2023-10-30 09:00:00 +0100
tags: article dev game-design
---

# Oupsi&nbsp;!

Je crée un fichier `README` public, pour que celui-ci soit copié sur l'environnement GitHub qui me sert d'environnement de déploiement. De façon à ce que les utilisateurs GitHub qui tomberaient sur mon projet en recherchant des mots-clés puissent trouver l'URL de la version déployée. Et commencer à utiliser le jeu&nbsp;! (Je l'ai déjà dit : il va me falloir du _feedback_ utilisateur assez vite 🙂).

Donc j'en profite pour ajouter une petite mention "_Si vous voulez me faire part de vos retours, vous pouvez m'écrire par e-mail_", et je donne l'adresse que j'ai utilisée pour créer mon compte GitHub.

Au passage, je me dis «&nbsp;_Tiens, si j'allais voir si par hasard j'ai reçu des emails ?_&nbsp;». On ne sait jamais...

Et là, impossible de retrouver quel mot de passe j'avais utilisé pour le compte Gmail&nbsp;!&nbsp;😱

J'essaie tous les mots de passe que j'aurais pu choisir à l'époque, mais tout est refusé... Et comme j'ai fait ça rapidement, je n'avais pas configuré de méthode de récupération quelconque.

Donc mon compte Gmail est perdu à jamais. Parce que Google ne m'autorisera pas à le récupérer avec si peu d'informations enregistrées dessus...

Et donc le compte GitHub, dont j'ai également oublié le mot de passe (parce que c'est probablement le même, me connaissant) est également inaccessible. Je ne peux pas utiliser la fonction "Mot de passe oublié" car mon adresse email est inaccessible. Et tout ce que j'ai en local pour me connecter à GitHub est une clé SSH. Donc le compte GitHub est perdu également...

La loose.&nbsp;😥

Je dois me retaper la création d'un nouveau compte Gmail, puis d'un nouveau compte GitHub. Et j'ai donc un nouvel environnement de déploiement : http://pokemontriad.github.io.

> Quantité de travail (perdue) : 1h...

Note pour l'avenir&nbsp;: **toujours enregistrer les mots de passe, ou avoir un moyen de récupérer les comptes&nbsp;!**

# À la recherche de la marche perdue

Toujours assez démotivé par la hauteur de la marche suivante (la gestion de la collection de cartes), il se passe plusieurs semaines sans que je ne touche la moindre ligne de code. Enfin, c'est aussi et surtout parce que je n'ai pas de temps à y consacrer.

Mais mon cerveau continue de réfléchir en tâche de fond...&nbsp;⚙

«&nbsp;_Choisir parmi sa collection.... enrichir sa collection... des rencontres différentes... une difficulté croissante... choix aléatoire parmi une pré-sélection de pokémons..._&nbsp;»

Et la notion de _niveau_ apparaît&nbsp;!

Si les rencontres du joueur sont complètement aléatoires, il est fort probable qu'il soit confronté à des cartes dont les valeurs sont très hautes. Et que, même face à la faible intelligence de l'IA, il perde souvent. Il faut donc des niveaux de difficulté croissante. Donc des niveaux dans lesquels le set de cartes de l'adversaire est choisi aléatoirement parmi une sous-sélection pré-configurée.

De plus, le fait de rencontrer des pokémons différents dans différentes zones colle très bien avec l'esprit du jeu&nbsp;!

Je peux donc commencer à travailler sur ces niveaux de difficulté. Et il se trouve que le scénario des jeux me fournit tout ce dont j'ai besoin pour définir ces zones&nbsp;: un nom, une ambiance graphique, une pré-sélection de pokémons, etc. En plus de m'économiser des efforts d'invention, cela rappellera de bons souvenirs aux joueurs qui ont connu les jeux de première génération.&nbsp;😉

Et la marche est tout de suite beaucoup moins importante&nbsp;!!

Le joueur ne pourra pas encore acquérir de nouveaux pokémons, certes. Mais il pourra arpenter différentes zones, pour des affrontements de difficulté croissante.

La quantité de travail me semble raisonnable.

Donc c'est parti&nbsp;!!&nbsp;🔥

🕐... 🕑... 🕒...

> Quantité de travail : 4h, soit maintenant 28h au total (ça commence à faire&nbsp;!)

Pour une marche censée être moins haute, il m'aura quand même fallu 4h de travail.

Je me rends compte que 4h, c'est un peu la taille critique d'une nouvelle fonctionnalité. C'est une durée que j'ai du mal à caser dans mon emploi du temps pour réussir à la traiter en 2-3 jours maximum (je ne veux pas que les fonctionnalités traînent trop longtemps... depuis le temps que je vous en parle, vous devez commencer à bien cerner le petit démon qui me guette&nbsp;😈).

- `3h` pour la partie "moteur de jeu" + l'intégration à l'IHM existante&nbsp;: modéliser une répartition de probabilités d'apparition, réaliser un choix aléatoire pondéré par ces probabilités, modéliser une zone d'exploration, créer le composant de sélection d'une zone et gérer son interaction avec les autres (entre autres, le composant qui affiche le plateau de jeu n'est maintenant plus responsable que de la gestion d'une unique partie&nbsp;: c'est un composant parent qui gère les actions "rejouer" ou "changer de zone").

- `1h` pour améliorer le rendu de l'affichage des zones&nbsp;: une petite image (toujours honteusement pompée sur le net&nbsp;🙄), un cadre, un agencement temporaire mais qui me plaît bien.

Et voilà le résultat&nbsp;:

![](/assets/images/pokemon-triad/pokemon-triad-areas.png)

Les pokémons rencontrés varient d'une zone à l'autre, rendant les parties légèrement différentes. Mais toujours pas très difficiles, pour être honnête...&nbsp;😅

<a class="navigation next" href="{% link _posts/2023/2023-12-17-developper-jeu-mode-agile-episode-7.md %}">Développer un jeu en mode agile - Épisode 7</a>
