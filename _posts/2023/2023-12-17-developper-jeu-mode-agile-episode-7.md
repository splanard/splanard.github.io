---
layout: post
title: "Développer un jeu en mode agile - Épisode 7"
date: 2023-12-17 09:00:00 +0100
tags: article dev game-design
---

# Welcome&nbsp;!

Il se passe plus d'un mois sans que je ne touche à ce projet. C'est d'ailleurs un petit miracle que j'aie fini par m'y remettre&nbsp;!&nbsp;😯

Entre temps, j'ai pensé à une amélioration qui m'avancerait légèrement vers la persistance d'une collection de pokémons pour le joueur : l'ajout d'un écran d'accueil.

Actuellement, l'utilisateur arrive directement sur l'écran de choix de la zone à explorer. Si on imagine que l'utilisateur a des données persistantes, il faudrait au préalable les charger. Cet écran d'accueil peut porter cette responsabilité&nbsp;:

- vérifier si des données sont disponibles pour l'utilisateur courant
- si oui, les lui présenter brièvement (`Nous avons trouvé le compte "Martin"`) et proposer de les charger.
- si non, proposer la création d'un compte en demandant un nom d'utilisateur.

Dans les deux cas, à la fin, on se retrouverait sur l'écran de choix des zones d'exploration. Mais on peut imaginer afficher le nom de l'utilisateur quelque part, pour montrer que les données de l'utilisateur sont bien chargées.

Je pense qu'on reste dans une évolution coûteuse, mais nécessaire à une quelconque persistance d'information pour l'utilisateur...&nbsp;🔥

🕐...

Ce n'était finalement pas si long ! En `1h`, j'ai un écran d'accueil (moche, mais je ne suis pas inspiré pour le design pour le moment. Donc autant avancer sur le reste) qui permet de créer, charger ou supprimer un compte. Les données du compte sont enregistrées dans un cookie du navigateur. La base d'une persistance de donnée locale, sur le navigateur de l'utilisateur, est posée. Et cela va grandement enrichir l'expérience de jeu&nbsp;!

> Quantité de travail totale : 29h

# Sur la lancée&nbsp;!

Reprendre le projet et créer un écran d'accueil m'a vraiment remotivé&nbsp;!&nbsp;💪

J'ai donc rapidement enchaîné plusieurs sessions de travail dans les jours qui ont suivi&nbsp;:

- Quelques **améliorations graphiques** et surtout une grosse phase d'**ajout de contenu** (plusieurs pokémons et zones supplémentaires)&nbsp;`+3h`

- **Refactoring** et **persistance de la collection** de cartes (immuable pour le moment, certes) dans le cookie. J'ai rencontré pas mal de problèmes avec le parsing du JSON en une classe JS fonctionnelle (méthodes comprises).&nbsp;`+2h`

- Rendre l'**affichage des cartes** réellement **responsive**. Sur ce point, j'avoue que le bénéfice utilisateur était minime, mais la façon dont j'avais géré les différentes largeurs d'affichage ne me convenait pas du tout (notamment la taille de police variable pour les valeurs des cartes...). Je suis bien plus satisfait de la version actuelle (j'ai remplacé le texte par des images SVG de chiffres, ce qui m'a beaucoup simplifié la tâche pour rendre les cartes responsive).&nbsp;`+3h`

- **Constitution d'un set de cartes** en début de partie, à partir des cartes dans la collection du joueur. Là encore, pour le moment, les cartes sont toujours les mêmes. Mais c'est une étape de plus vers le fait de pouvoir ajouter des cartes à sa collection.&nbsp;`+2h`

# Enfin, de nouvelles cartes&nbsp;!

Puis, enfin, la marche qui me faisait si peur il y a encore quelques mois/semaines, l'**acquisition de nouvelles cartes**&nbsp;!!&nbsp;🎉

Dorénavant, à la fin de chaque partie, en cas de victoire, le joueur peut choisir une des cartes adverses. Et cette carte est ajoutée à sa collection personnelle&nbsp;! Lui permettant de choisir en début de partie des cartes différentes, potentiellement plus "fortes" que les cartes de départ, ou dont le type est plus adapté aux rencontres à venir.

En termes de _gameplay_, c'est l'ajout le plus intéressant depuis longtemps sur le jeu. À tel point qu'en ajoutant encore de nouvelles cartes et zones à explorer, **la version actuelle pourrait constituer un MVP**&nbsp;!

Et, grâce à toutes les étapes préparatoires précédentes, tous les petits ajouts réalisés ces dernières semaines, cette étape ne m'a pris _que_ `2h` supplémentaires.

Suite au déploiement de cette nouvelle version, j'ai ensuite passé environ `1h` à débuguer des petits problèmes (dont un assez impactant pour la jouabilité : le composant qui permet d'acquérir une nouvelle carte ne s'affichait pas sur la version déployée&nbsp;😅) et à réaliser de petites améliorations graphiques.

> Quantité de travail totale : 42h

Voilà le résultat&nbsp;:

![](/assets/images/pokemon-triad/pokemon-triad-almost-mvp-1.png)

![](/assets/images/pokemon-triad/pokemon-triad-almost-mvp-2.png)

![](/assets/images/pokemon-triad/pokemon-triad-almost-mvp-3.png)

![](/assets/images/pokemon-triad/pokemon-triad-almost-mvp-4.png)

![](/assets/images/pokemon-triad/pokemon-triad-almost-mvp-5.png)

# Création d'un backlog

Mon projet arrive bientôt à **une étape charnière&nbsp;: le MVP** (Minimum Viable Product). On s'approche d'un jeu utilisable, sur lequel un utilisateur va prendre plaisir à passer un peu de temps et pourra revenir plus tard continuer sa partie en cours.

Jsuqu'à présent, le développement du jeu s'effectuait en ligne droite&nbsp;: ajouter les fonctionnalités principales, les unes après les autres, pour atteindre ce MVP. C'était linéaire, un peu comme conduire sur une route dans le désert américain... Le MVP est au bout de la route, donc on avance sans trop se poser de question.

Une fois le MVP atteint, c'est une toute autre histoire&nbsp;!

Les possibilités d'évolution sont multiples, les chemins à explorer divergent. Et il faudra faire des choix&nbsp;! Car à partir de là, tous les chemins ne mènent pas au même endroit.&nbsp;🧭

J'ai déjà identifié un grand nombre d'évolutions possibles. Je n'avais pas encore pris le temps de tracer ces idées, car je voulais rester concentré sur la première version du jeu. Mais je vais maintenant pouvoir **constituer un backlog**, qu'il faudra que je priorise, selon l'orientation que je veux donner au jeu.

Et pour cela, je vais utiliser les _Issues_ de GitLab, plateforme sur laquelle est hébergé le code source du projet.

🕐...

C'est ainsi que je me retrouve avec un backlog contenant 25 éléments&nbsp;😅, allant d'une simple correction de style à l'implémentation d'un inventaire ou d'un système monétaire.

J'ai marqué avec un tag `mvp` certains éléments de la liste qui me paraîssent indispensables pour pouvoir déployer une première version publique&nbsp;:

- **Visualiser sa collection**&nbsp;: à quoi bon collectionner des cartes si on ne peut pas prendre quelques minutes pour observer avec fierté sa collection&nbsp;!&nbsp;😏

- **Ajouter de nouveaux contenus**&nbsp;: nouvelles cartes, nouvelles zones. Même si je n'ajoute pas tout dans un premier temps, il faut tout de même une expérience de jeu suffisamment longue pour que l'utilisateur y prenne plaisir et patiente jusqu'à l'ajout des contenus suivants.

- **Proposer plusieurs ambiances graphiques**, selon la zone visitée. Actuellement, il y a de l'herbe au beau milieu du Mont Selenite, ça ne fait pas sérieux...&nbsp;🙄

- **Mieux identifier les types** des cartes. Actuellement, le type de l'attaque n'est pas clairement identifié. Les types utilisés pour calculer l'efficacité d'une attaque non plus... Il faudrait avoir un indice visuel pour identifier ces éléments au premier coup d'oeil.

- **Rappel des forces et faiblesses**&nbsp;: il n'y a actuellement aucune indication de quel type est efficace contre quels types. C'est une information importante pour utiliser les forces/faiblesses dans le jeu, mais qui n'est pour le moment consultable nulle part...

- **Afficher le score en fin de partie**.

- **Améliorer l'effet visuel de sélection** d'une carte, dans la phase de jeu. Actuellement, une ombre dorée apparaît sous la carte sélectionnée. Mais j'ai pu constater que certaines personnes avaient du mal à la voir&nbsp;: elle est trop discrète (et moyennement bien réalisée, soyons lucide).

- **Améliorer l'IA adverse**&nbsp;: les parties sont actuellement trop faciles à gagner. Même avec le set _starter_, on peut gagner des parties dans tous les zones, même sans utiliser correctement les forces/faiblesses. L'IA aurait bien besoin d'un coup de boost&nbsp;!&nbsp;🤖

- **Ajouter un moyen de contact** dans l'application, pour que des utilisateurs puissent me donner leurs impressions, signaler des bugs, faire des suggestions, etc.

- **Ajouter un léger délai avant l'affichage du résultat d'une partie**. Actuellement, l'affichage du résultat est instantané, au moment où on pose la dernière carte. Cela ne permet pas de visualiser l'effet de la dernière carte posée, c'est un peu déroutant. Pas terrible en termes d'expérience utilisateur.

- **Mettre en place un système d'analyse d'audience**, afin de me permettre d'avoir des informations sur la fréquentation du site.

- **Améliorer le rendu sur les résolutions larges**. Actuellement, le design a été pensé pour un affichage sur mobile, en mode portrait. Il faudrait que j'améliorer le rendu sur des résolutions plus larges&nbsp;: mobile en mode paysage, tablette, PC.

- **Ajouter des liens de partage via les réseaux sociaux**, pour aider le jeu à se faire connaître sur le net.

Il en reste du travail...&nbsp;😥

<a class="navigation next" href="{% link _posts/2024/2024-05-29-developper-jeu-mode-agile-episode-8.md %}">Développer un jeu en mode agile - Épisode 8</a>
