---
layout: post
title: "Comprendre ce qu'est la « dette technique »"
date: 2018-03-14 09:00:00 +0100
tags: article dev
---

_Cet article est une version révisée d'un [article initialement publié sur LinkedIn](https://www.linkedin.com/pulse/comprendre-ce-quest-la-dette-technique-s%25C3%25A9bastien-planard/)._

La « dette technique » est un terme fréquemment employé dans le secteur du développement logiciel, mais souvent mal compris par les _non-développeurs_. J'ai constaté qu'il existait de nombreux articles traitant du sujet, mais pas toujours avec la simplicité suffisante pour être accessibles à tous. Je me suis donc essayé à l'exercice de vulgarisation. 🙂

# Rien n'est parfait... pas longtemps, en tout cas !

Imaginez qu'un logiciel informatique soit une machine complexe, comme une chaîne de montage de voitures par exemple. Au cours de la vie de cette machine, on fera dessus de nombreuses interventions :

- **Réparation** : lorsqu'on découvre un défaut dans la chaîne de montage, ou qu'une pièce casse, il faut réparer pour **rétablir son fonctionnement normal** et continuer à produire des voitures.

Dans le cas du logiciel informatique, on le fait aussi : on corrige des bugs, on redémarre des serveurs qui se sont arrêtés, etc.

- **Amélioration** : si on veut que notre chaîne de montage puisse produire d'autres modèles de voitures que la classique citadine, il va falloir lui apporter des modifications, la reconfigurer pour la rendre plus générique. Par exemple, on va faire en sorte que la section qui fixe les roues puisse travailler avec des roues de tailles différentes, on va s'assurer que tous les gabarits de chassis passent sur la chaîne, etc. Je vous laisse imaginer tout ce qui peut varier d'un modèle de voiture à l'autre... Bref, **de nouvelles capacités sont ajoutées** à la chaîne de montage.

Dans le cas du logiciel informatique, on réalise également ce genre d'opérations. On appelle généralement cela des évolutions : les développeurs vont modifier le code pour ajouter au produit de nouvelles fonctionnalités.

- **Entretien** : enfin, il arrive que certains composants de la machine deviennent trop vieux. La technologie évoluant, il faut parfois remplacer certaines parties de la chaîne de montage, voire la chaîne entière, pour rester compétitif sur le marché. Par exemple, on va automatiser le serrage des boulots qui, auparavant, était réalisé à la main. Cela nécessite d'investir dans un nouveau bras articulé muni de l'outil adapté pour serrer les boulons. Ou bien on va remplacer le pistolet à peinture car il en existe maintenant des plus performants, qui vont nous permettre de produire plus vite. Et pourtant, **les fonctionnalités de la chaîne restent inchangées** : après l'entretien, elle produira exactement les mêmes voitures qu'avant.

Vous voyez venir la suite ? 😏 Dans le cas du logiciel informatique, c'est exactement pareil ! Et c'est ce troisième type d'intervention technique qui va nous intéresser dans cet article !

# Tentative de définition

On peut définir la dette technique comme **le décalage entre l'état technique d'un logiciel à un moment donné** (la façon dont son code est écrit, les langages et outils utilisés, etc.) **et ce qui se fait de mieux** dans le domaine, de plus performant, à ce même moment.

Cette dette peut s'accumuler de plusieurs façons.

# L'évolution naturelle des technologies

Vous l'ignorez peut-être, mais les langages de programmation et les outils utilisés pour le développement logiciel évoluent. Et même très vite !

Vous avez beau choisir le top du top des technologies informatiques au démarrage d'un projet, **ces technos vont évoluer et/ou vieillir**. Vous n'y couperez pas. Des mises à jour seront apportées aux technologies que vous utilisez. Et d'autres, potentiellement plus performantes, vont apparaître.

D'autres technologies que vos concurrents choisiront peut-être d'adopter, leur apportant un avantage compétitif sur vous...

**Chaque mois ou année qui passe, votre retard technologique s'accumule**. Et cela peut commencer dès le lendemain du démarrage du projet ! 😓

Ce retard est une partie de votre dette technique. Ce n'est la faute de personne, c'est simplement une conséquence du progrès technologique dans votre domaine d'activité. Et dans le cas du développement logiciel, cet aspect de la dette technique s'accumule particulièrement vite.

# « On n'a pas le temps ! »

Un peu moins avouable mais tout aussi présent, un autre aspect de la dette technique rencontré en informatique peut être illustré par cette phrase, que vous avez probablement déjà entendue si vous travaillez dans le secteur :

«&nbsp;_On n'a pas le temps, il faut livrer la semaine prochaine ! On va faire comme ça pour l'instant et on corrigera plus tard_&nbsp;».

Que celles et ceux qui ont déjà travaillé sur des projets informatiques et n'ont jamais entendu cette phrase lèvent la main ! (Et dites-moi dans quelle boîte vous travaillez aussi... 😝) Et, souvent, «&nbsp;plus tard&nbsp;» se transforme en «&nbsp;jamais&nbsp;».

Au démarrage d'un projet, on est motivé, on a du budget et du temps. On va donc se fixer certains principes de qualité. Parce que coder proprement, c'est important !

Mais **lorsque vont arriver les premières livraisons**, les premières urgences, le client qui tape du poing sur la table parce que cet énième décalage de livraison est inadmissible... **on va peut-être lâcher un peu ces beaux principes** : négliger un peu la qualité du code, remettre à plus tard des montées de version, etc.

Ce renoncement est plus ou moins rapide selon les entreprises et les équipes, mais il arrive généralement à un moment ou à un autre, dans une certaine mesure...

Pourtant, ce sont justement ces bonnes pratiques, définies au début du projet et améliorées par la suite, qui assurent la productivité de l'équipe. Y renoncer, même temporairement, c'est donc accepter que dans les semaines/mois/années à venir, on produira moins vite.

Certes, la veille de la livraison, vous allez gagner du temps en allant à l'encontre de certains principes de qualité. Mais la dégradation de la qualité technique de votre produit, et son impact sur votre productivité à moyen ou long terme, vous fera finalement perdre plus de temps que vous n'en avez gagné...

**Ces dérives par rapport aux bonnes pratiques de développement participent donc à la fameuse dette technique** : c'est un décalage entre la qualité du code actuelle et la ligne directrice qu'on s'était fixée au début du projet (ou celle des concurrents qui, eux, ont tenu bon et produisent maintenant plus vite que vous).

# Comprendre, contrôler et réduire la dette

**La dette technique est un handicap, un malus** qui s'applique sur un projet ou une entreprise. Elle réduit sa productivité, que ce soit en valeur absolue ou vis-à-vis de concurrents moins «&nbsp;endettés&nbsp;». Elle frustre également les équipes techniques, ce qui n'est pas bon pour le moral des troupes.

**Mais elle n'est pas anormale pour autant !** 🤯

Personne n'est responsable du fait que la technique évolue : c'est comme ça, il faut faire avec. Aucun projet ne reste à la pointe de façon permanente, à moins de s'en donner les moyens (et cela peut coûter très cher de s'en donner les moyens !! Toutes les entreprises ne peuvent pas se le permettre). Et même le perfectionniste que je suis sait reconnaître qu'il faut parfois faire des concessions **_temporaires_** sur la qualité du code pour des raisons financières ou opérationnelles.

Mais il est indispensable de savoir **estimer l'ampleur de cette dette et de la maintenir à un niveau acceptable**.

Imaginez que vous fassiez le choix de ne pas faire évoluer du tout vos technologies informatiques, parce que cela prend trop de temps / coûte trop cher. Outre le fait que vos concurrents ne feront peut-être pas le même choix, les personnes compétentes dans une technologie trop ancienne se font de plus en plus rares au fil des années. Jusqu'au jour où il n'existe plus personne qui sache maintenir ou faire évoluer votre logiciel trop vieux. Et dans ce cas, c'est la refonte qui vous attend : devoir redévelopper entièrement votre produit.

Pour rester compétitif, il faut donc **régulièrement contrôler et réduire cette dette**. Le nom est d'ailleurs bien choisi&nbsp;: une dette, ça se rembourse. Sinon, on finit par avoir des problèmes...

Dans le développement informatique, on le fait :

- en se tenant informé de l'évolution des technologies du secteur,
- en mettant en place des indicateurs de qualité de code et en les suivant au quotidien,
- en recodant parfois un morceau du logiciel de façon à le rendre plus performant et/ou plus réutilisable,
- en adoptant de nouveaux langages et outils au cours de la vie du projet, pour tenter de suivre le progrès technologique, ne pas se laisser trop distancer,
- en prenant le temps de mettre à jour les composants externes utilisés.

# Et qui va payer ?

En tant que développeur, je constate que les clients, les managers, les directeurs (bref, ceux qui paient) ont souvent beaucoup de mal à comprendre cet aspect du développement.

«&nbsp;_On paie sans avoir quoi que ce soit de nouveau en échange !_&nbsp;»

«&nbsp;_Quand une fonctionnalité est en place et qu'elle fonctionne, pourquoi devrait-on encore payer pour&nbsp;? Quand c'est fait, c'est fait._&nbsp;»

Il y a du vrai dans ces remarques, qui vous rappellent peut-être des choses que vous avez pu dire ou entendre. Mais **pour que le produit puisse continuer de fonctionner et d'apporter sa plus-value au client de façon continue, le contrôle de la dette technique est un investissement indispensable**.

Revenons à l'hypothétique chaîne de montage de voitures...

Avez-vous déjà vu une machine qui, une fois installée, tourne indéfiniment sans aucun entretien&nbsp;? Si vous ne prenez pas le temps de l'entretenir et de la moderniser, elle continuera un temps à produire ses 2 voitures par jour. Mais votre concurrent, lui, en produira 5, puis 10, puis 20 par jour, avec de meilleures finitions. Lorsqu'un élément de la vôtre cassera, il ne sera plus possible de commander les pièces détachées pour le réparer, car ce matériel sera devenu obsolète. Et enfin, lorsque votre chaîne de montage, rouillée et rafistolée à la hâte à maintes reprises, s'arrêtera définitivement, il ne vous restera plus qu'à investir de très grosses quantités d'argent dans de nouveaux équipements tout neufs, mais dont l'installation prendra beaucoup de temps avant d'être en mesure de produire à nouveau. Alors qu'un entretien et une modernisation régulière vous auraient permis de rester dans la course... 😔

# Pour aller plus loin...

J'ajoute en complément un lien vers l'excellent article du blog Ippon : [La dette technique tue les grenouilles](https://blog.ippon.fr/2018/03/15/la-dette-technique-tue-les-grenouilles/). L'article, dont je vous recommande la lecture, bien qu'il soit basé sur [une fable qui s'avère scientifiquement fausse](https://fr.wikipedia.org/wiki/Fable_de_la_grenouille), introduit la notion de "dette psychologique"&nbsp;: la dette technique n'aura pas d'impact uniquement sur vos produits et votre compétitivité. Elle aura également un impact fort sur le moral et la motivation de vos équipes de développement.
