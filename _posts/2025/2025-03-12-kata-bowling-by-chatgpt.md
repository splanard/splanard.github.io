---
layout: post
title: "Le kata Bowling par ChatGPT"
date: 2025-03-17 22:30:00 +0100
tags: article dev kata
---

Dans mon <a href="{% link _posts/2025/2025-03-10-kata-bowling.md %}">article précédent</a>, je présentais une approche du kata de code "Bowling", dont le but est d'écrire un programme qui calcule le score final d'un joueur lors d'une partie de bowling.

L'article était long. Je n'y avais donc pas abordé l'IA, à la fin, comme je l'avais fait pour <a href="{% link _posts/2025/2025-02-13-kata-suite-fibonacci.md %}">le kata sur la suite de Fibonacci</a>.

C'est l'objectif de cet article&nbsp;: élaborer un prompt à partir de l'énoncé du kata "Bowling", le fournir à un moteur d'IA générative pour analyser le code produit.

# Quel énoncé&nbsp;?

Premier dilemme&nbsp;: quel énoncé fournir&nbsp;?

Une des premières tâches que j'ai réalisées lors de mon approche du kata était de reformuler l'[énoncé initial](https://codingdojo.org/kata/Bowling/), car je trouvais les termes employés hasardeux.

D'abord, je me dis que cette première étape fait partie de ma démarche personnelle. Il serait donc intéressant de voir comment l'IA s'en sort avec l'énoncé initial.

Mais en y réfléchissant, je trouve intéressant de fournir, dans un deuxième temps, l'énoncé reformulé. Pour voir si la formulation de l'énoncé a un impact sur la qualité du code produit. Même si, dans le cas présent, le calcul du score d'une partie de bowling est un sujet tellement connu qu'il est fort probable que le contenu précis de l'énoncé fasse peu de différence&nbsp;: l'IA va dans tous les cas aller piocher dans ses connaissances des règles du bowling pour répondre...

_Le moteur utilisé est un GPT-4o mini gratuit trouvé en ligne. Il doit exister plus performant, mais le but n'est pas ici de faire un comparatif des IA génératives. C'est plutôt d'analyser le contenu sur lequel pourrait se baser un développeur junior qui utiliserait l'IA un peu trop naïvement..._

# Premier essai, l'ascenseur émotionnel

## Prompt

Voici donc le premier prompt que je fournis.

«&nbsp;_En utilisant le langage TypeScript, je souhaite que tu crées un programme qui répond à l'énoncé suivant (qui est en anglais)&nbsp;:_

_"Create a program which, given a valid sequence of rolls for one line of American Ten-Pin Bowling, produces the total score for the game. Here are some things that the program will not do:_

- _it will not check for valid rolls._
- _it will not check for correct number of rolls and frames._
- _it will not provide scores for intermediate frames._

_We can briefly summarize the scoring for this form of bowling:_

- _Each game, or “line” of bowling, includes ten turns, or “frames” for the bowler._
- _In each frame, the bowler gets up to two tries to knock down all the pins._
- _If in two tries, he fails to knock them all down, his score for that frame is the total number of pins knocked down in his two tries._
- _If in two tries he knocks them all down, this is called a “spare” and his score for the frame is ten plus the number of pins knocked down on his next throw (in his next turn)._
- _If on his first try in the frame he knocks down all the pins, this is called a “strike”. His turn is over, and his score for the frame is ten plus the simple total of the pins knocked down in his next two rolls._
- _If he gets a spare or strike in the last (tenth) frame, the bowler gets to throw one or two more bonus balls, respectively. These bonus throws are taken as part of the same turn. If the bonus throws knock down all the pins, the process does not repeat: the bonus throws are only used to calculate the score of the final frame._
- _The game score is the total of all frame scores."_

_Le code produit doit être entièrement en anglais. Et je souhaite que tu génères également les tests unitaires associés aux éléments de code produits en utilisant la syntaxe de la librairie Vitest (describe, it, expect)._&nbsp;»

## Woaw...

Et voici le code produit&nbsp;:

```ts
function calculateBowlingScore(rolls: number[]): number {
  let totalScore = 0;
  let rollIndex = 0;

  for (let frame = 0; frame < 10; frame++) {
    if (isStrike(rolls, rollIndex)) {
      totalScore += 10 + strikeBonus(rolls, rollIndex);
      rollIndex++;
    } else if (isSpare(rolls, rollIndex)) {
      totalScore += 10 + spareBonus(rolls, rollIndex);
      rollIndex += 2;
    } else {
      totalScore += sumOfRolls(rolls, rollIndex, 2);
      rollIndex += 2;
    }
  }

  return totalScore;
}

function isStrike(rolls: number[], rollIndex: number): boolean {
  return rolls[rollIndex] === 10;
}

function isSpare(rolls: number[], rollIndex: number): boolean {
  return rolls[rollIndex] + rolls[rollIndex + 1] === 10;
}

function strikeBonus(rolls: number[], rollIndex: number): number {
  return rolls[rollIndex + 1] + rolls[rollIndex + 2];
}

function spareBonus(rolls: number[], rollIndex: number): number {
  return rolls[rollIndex + 2];
}

function sumOfRolls(rolls: number[], rollIndex: number, numberOfRolls: number): number {
  return rolls.slice(rollIndex, rollIndex + numberOfRolls).reduce((a, b) => a + b, 0);
}
```

Et les tests qui vont avec&nbsp;:

```ts
describe("Bowling Score Calculator", () => {
  it("calculates score for a game with no strikes or spares", () => {
    const rolls = [1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 1, 2];
    expect(calculateBowlingScore(rolls)).toBe(45);
  });

  it("calculates score for a game with spares", () => {
    const rolls = [5, 5, 3, 4, 2, 8, 1, 9, 10, 2, 3];
    expect(calculateBowlingScore(rolls)).toBe(63);
  });

  it("calculates score for a game with strikes", () => {
    const rolls = [10, 10, 10, 2, 3, 4, 5, 6, 7, 8];
    expect(calculateBowlingScore(rolls)).toBe(88);
  });

  it("calculates score for a perfect game", () => {
    const rolls = Array(12).fill(10); // 10 strikes + 2 bonus rolls
    expect(calculateBowlingScore(rolls)).toBe(300);
  });

  it("calculates score for a game with mixed strikes and spares", () => {
    const rolls = [10, 3, 7, 9, 0, 10, 0, 8, 8, 2, 10, 10, 10];
    expect(calculateBowlingScore(rolls)).toBe(193);
  });
});
```

À première vue, ça me semble pas mal du tout&nbsp;!&nbsp;😲

Je constate que, là où j'avais choisi une approche objet, l'IA a choisi des fonctions. C'est le risque avec TypeScript... Mais c'est un détail&nbsp;: j'aurais pu explicitement lui dire que je voulais une approche objet, si cela avait été un critère.

En tout cas, je suis **agréablement surpris par le résultat**. L'algo est très proche de la version à laquelle j'en suis moi-même arrivé. Les écueils que j'ai rencontrés en cours de route semblent avoir été évités. Les responsabilités sont moins séparées, mais le résultat est d'autant plus concis.

## ...ou pas&nbsp;!

Par acquis de conscience, j'ajoute les quelques imports manquants, puis j'exécute les tests unitaires.

```ts
describe("Bowling Score Calculator", () => {
  it("calculates score for a game with no strikes or spares", () => {
    const rolls = [1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 1, 2];
    expect(calculateBowlingScore(rolls)).toBe(45); // ❌ résultat reçu: 48
  });

  it("calculates score for a game with spares", () => {
    const rolls = [5, 5, 3, 4, 2, 8, 1, 9, 10, 2, 3];
    expect(calculateBowlingScore(rolls)).toBe(63); // ❌ résultat reçu: 71
  });

  it("calculates score for a game with strikes", () => {
    const rolls = [10, 10, 10, 2, 3, 4, 5, 6, 7, 8];
    expect(calculateBowlingScore(rolls)).toBe(88); // ❌ résultat reçu: 102
  });

  it("calculates score for a perfect game", () => {
    const rolls = Array(12).fill(10); // 10 strikes + 2 bonus rolls
    expect(calculateBowlingScore(rolls)).toBe(300); // ✅
  });

  it("calculates score for a game with mixed strikes and spares", () => {
    const rolls = [10, 3, 7, 9, 0, 10, 0, 8, 8, 2, 10, 10, 10];
    expect(calculateBowlingScore(rolls)).toBe(193); // ❌ résultat reçu: NaN
  });
});
```

Aïe&nbsp;!&nbsp;😅

Bon, il va falloir débuguer tout ça... Comment se fait-il que les tests unitaires fournis par l'IA échouent&nbsp;?

### Cas 1 : ni spare, ni strike

```ts
it("calculates score for a game with no strikes or spares", () => {
  const rolls = [1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 1, 2];
  expect(calculateBowlingScore(rolls)).toBe(45); // ❌ résultat reçu: 48
});
```

Déjà, on peut noter que le nombre de lancers ne correspond pas à une partie complète. Mais ce n'est pas forcément très grave.

Plus grave, les lancers sont invalides. Par exemple, un lancer à 5 puis un lancer à 6 dans le même carreau, ce n'est pas possible. Certes, l'énoncé précisait qu'il n'était pas nécessaire de valider les lancers, mais ça n'empêche pas de passer des données représentatives tout de même&nbsp;!&nbsp;🫤

Mais tout cela n'explique pas pourquoi le test échoue...

Par contre, 1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9 + 0 + 1 + 2 = 48. D'où sort le 45 de l'assertion&nbsp;? Pas la moindre idée.

Le corrige donc ce premier test. Et avec des valeurs correctes, le test passe&nbsp;:

```ts
it("calculates score for a game with no strikes or spares", () => {
  const rolls = [1, 2, 3, 4, 5, 3, 7, 2, 9, 0, 1, 2];
  expect(calculateBowlingScore(rolls)).toBe(39); // ✅
});
```

### Cas 2 : spares

```ts
it("calculates score for a game with spares", () => {
  const rolls = [5, 5, 3, 4, 2, 8, 1, 9, 10, 2, 3]; // ❌ résultat reçu: 71
  expect(calculateBowlingScore(rolls)).toBe(63);
});
```

Bon, il y a aussi un strike dans le jeu de connées de ce cas de test... pas idéal, compte-tenu de l'intention de test annoncée&nbsp;😒

J'ajoute un zéro pour supprimer le strike (et ajouter un spare un peu spécial par la même occasion), je recompte les points, et ça passe&nbsp;:

```ts
it("calculates score for a game with spares", () => {
  const rolls = [5, 5, 3, 4, 2, 8, 1, 9, 0, 10, 2, 3];
  expect(calculateBowlingScore(rolls)).toBe(58); // ✅
});
```

### Cas 3 : strikes

```ts
it("calculates score for a game with strikes", () => {
  const rolls = [10, 10, 10, 2, 3, 4, 5, 6, 7, 8];
  expect(calculateBowlingScore(rolls)).toBe(88); // ❌ résultat reçu: 102
});
```

Là encore, les derniers lancers ne sont pas valides&nbsp;: je vais corriger ça. Et, à nouveau, c'est la valeur de l'assertion qui est fausse... Si je corrige les valeurs, ça passe&nbsp;:

```ts
it("calculates score for a game with strikes", () => {
  const rolls = [10, 10, 10, 2, 3, 4, 5, 6, 1, 4, 4];
  expect(calculateBowlingScore(rolls)).toBe(96); // ✅
});
```

### Cas 4 : strikes & spares

```ts
it("calculates score for a game with mixed strikes and spares", () => {
  const rolls = [10, 3, 7, 9, 0, 10, 0, 8, 8, 2, 10, 10, 10];
  expect(calculateBowlingScore(rolls)).toBe(193); // ❌ résultat reçu: NaN
});
```

Dans ce cas-là, les lancers ne représentent toujours pas une partie complète. Et je pense que le programme essaie de prendre en compte l'effet des 2 derniers strikes et plante (parce qu'il essaie d'accéder à des indices qui n'existent pas dans le tableau des lancers...).

Je corrige le cas de test, et ça passe&nbsp;:

```ts
it("calculates score for a game with mixed strikes and spares", () => {
  const rolls = [10, 3, 7, 9, 0, 10, 0, 8, 8, 2, 10, 0, 10, 9, 0, 10, 10, 10];
  expect(calculateBowlingScore(rolls)).toBe(172); // ✅
});
```

Enfin&nbsp;!

### Conclusion

L'algo est robuste, mais les tests générés étaient vraiment nuls...&nbsp;😒

J'ai du mal à comprendre comment l'IA peut produire un algorithme qui fonctionne, mais fournir des tests unitaires qui ne passent même pas car les assertions sont fausses.

Peut-être que, parmi l'ensemble des ressources dans lesquelles l'IA puise pour être capable de répondre à ma demande, l'algorithme qui calcule le score est assez courant, alors que les tests associés le sont moins... Peut-être qu'interpréter ma demande et trouver l'algorithme est à la portée d'une IA, mais que concevoir un jeu de tests cohérents l'est moins... Je ne sais pas.

# Déterminisme

Une chose qui me gêne régulièrement dans les IA génératives, surtout lorsque c'est appliqué à du code, c'est le manque de déterminisme. Traduction&nbsp;: si on leur donne 2 fois **_exactement_** la même consigne, le résultat peut être différent.

Je sais que c'est quelque chose qui peut se régler sur le moteur d'IA, mais ma connaissance s'arrête là.

En revanche, dans mon approche de l'IA, j'essaie de prendre le point de vue d'un jeune développeur qui commencerait à utiliser cet outil, un peu naïvement (peut-être parce qu'il lit partout sur LinkedIn que les devs qui ne maîtrisent pas l'IA vont disparaître, par exemple...&nbsp;😅). Si on prend un moteur ChatGPT lambda, grauit, facile à trouver en ligne, et qu'on lui donne 2 fois la même consigne... que se passe-t-il&nbsp;?

_A few minutes later..._

Ici, le code produit est sensiblement le même à chaque fois. Les tests, eux, varient... mais restent toujours d'aussi mauvaise qualité (lancers incohérents, valeurs fausses dans les assertions, etc.).

Le fait que le code soit identique est **plutôt rassurant**.

Mais **cela m'intrigue** tout de même... comment expliquer que, cette fois-ci, l'IA soit déterministe, alors que j'ai déjà constaté qu'elle pouvait ne pas l'être&nbsp;?&nbsp;🤔 (et je parle exactement du même moteur d'IA)

# Variations

À ce moment de mon expérimentation, je fais l'hypothèse que la réponse qualitative (en termes de code "de prod" uniquement, les tests étant de très mauvaise qualité) ainsi que son aspect déterministe sont exclusivement liés au fait que le **problème est mondialement connu**, et que les **implémentations sont très répandues** sur Internet.

Pour tenter de prouver ce point, je vais procéder à plusieurs tests&nbsp;:

- fournir le même énoncé en français (le nom "bowling" étant identique en anglais et en français, je pense que l'IA va bien s'en sortir)
- fournir le même énoncé en faisant disparaître complètement le vocabulaire du bowling

## Énoncé en français

### Prompt

Voilà l'énoncé, **traduit en français**, que j'ai fourni à l'IA&nbsp;:

_En utilisant le langage TypeScript, génère un programme qui, lorsqu’on lui fournit une séquence valide de lancers pour un joueur d’une partie de bowling américain à 10 quilles, calcule le score total du joueur pour la partie._

_Voici les règles à respecter :_

- _Chaque partie contient 10 carreaux_
- _Lors de chaque carreau, le joueur possède jusqu’à 2 lancers pour tenter de faire tomber toutes les quilles_
- _Si, après 2 lancers, il n’a pas réussi à faire tomber les 10 quilles, son score pour ce carreau est égale au nombre de quilles tombées après les 2 lancers._
- _Si, après 2 lancers, les 10 quilles sont tombées, on appelle cela un “spare”. Le score pour ce carreau est alors de 10 plus le nombre de quilles qui tomberont lors de son prochain lancer._
- _Si, après le premier lancer du carreau, les 10 quilles sont tombées, on appelle cela un “strike”. Le carreau est alors terminé, il n’y a pas de second lancer. Le score pour ce carreau est alors de 10 plus la somme du nombre de quilles qui tomberont lors de ses deux prochains lancers._
- _Si le joueur obtient un spare ou un strike lors du dernier (dixième) carreau, il obtient respectivement 1 ou 2 lancers supplémentaires. Ces lancers additionnels font partie du même carreau. Peu importe le nombre de quilles renversé lors de ces lancers additionnels, aucun autre lancer ne sera octroyé. Ils sont simplement utilisé pour tenir compte de l’effet du spare ou du strike sur le score final._
- _Le score total de la partie est la somme du score de chaque carreau._

_Et voici ce que le programme n'a pas besoin de faire et ne doit pas faire :_

- _Vérifier que les lancers sont valides (que le nombre de quilles tombées est inférieur ou égal à 10, par exemple)_
- _Vérifier le nombre de lancers et/ou de carreaux fournis_
- _Fournir le score intermédiaire pour chaque carreau (seul le score final est calculé)_

_Le code produit doit être entièrement en anglais. Génère également les tests unitaires associés aux éléments de code produits en utilisant la syntaxe de la librairie Vitest (describe, it, expect)._

### Résultat

Je ne copie-colle pas le résultat&nbsp;: comme je m'en doutais, la traduction ne lui pose pas de problème particulier. Et **le résultat est sensiblement identique** à la première version (code similaire, tests semblables et tout aussi faux).

Je me demande même si les règles de l'énoncé sont interprétées, à partir du moment où l'IA identifie qu'il s'agit du score du bowling. Peut-être que la simple information «&nbsp;_un programme qui calcule le score total d'un jour pour une partie de bowling_&nbsp;» lui suffit pour aller identifier une solution...

Dans tous les cas, je constate bien que **la langue n'a pas d'impact** sur le résultat. Mais j'aurais été surpris du contraire.

## Variation mineure dans les règles

Je décide d'éclaicir le point soulevé précédemment&nbsp;: **les règles mentionnées dans l'énoncé sont-elles lues/interprétées par l'IA à partir du moment où elle identifie qu'il s'agit d'un jeu connu&nbsp;?**

Je tente de modifier légèrement l'énoncé, en remplaçant simplement quelques valeurs dans l'énoncé (le nombre de points pour un "spare" est remplacé par 12 + le lancer suivant, le nombre de points pour un "strike" est remplacé par 15 + les 2 lancers suivants).

Je fournis le prompt altéré...

Et je constate que **le code s'est adapté&nbsp;!** La mécanique est toujours la même, l'algorithme presque identique. Mais certaines valeurs `10` ont été remplacées par les valeurs `12` et `15` que j'ai fournies.

Les règles de l'énoncé sont donc bien interprétées. L'IA est **capable de s'adapter à une légère variation de paramètres**.

## Énoncé opacifié

Suite à une discussion avec un collègue à la machine à café, je me suis posé la question de savoir si l'IA serait capable de fournir un algo aussi concis avec **l'énoncé d'un jeu inconnu**. Si je transforme le prompt pour faire disparaître complètement les notions associées au "bowling", l'IA sait-elle reconnaître les règles du bowling&nbsp;? Ou est-elle capable de produire un algorithme de calcul du score d'aussi bonne qualité&nbsp;?

Rien qu'à formuler la question, je suis à peu près convaincu que non. Mais il faut tester pour être sûr...

### Prompt

Voilà donc le prompt modifié.

_En utilisant le langage TypeScript, je souhaite que tu crées un programme qui répond à l'énoncé suivant (qui est en anglais)._

« _Create a program which, given a valid sequence of tries for one game of "Frog Tumble", produces the total score for the game._

_We can briefly summarize the scoring for this game:_

- _Each game includes ten turns for each player._
- _During each turn, the player gets up to two tries to knock over all the frogs._
- _If in two tries, he fails to knock them all over, his score for that turn is the total number of frogs knocked over in his two tries._
- _If in two tries he knocks them all over, this is called a "snake" and his score for the turn is ten plus the number of frogs knocked over on his next try (in his next turn)._
- _If on his first try in the turn he knocks over all the frogs, this is called a "heron". His turn is over, and his score for the turn is ten plus the simple total of the frogs knocked over in his next two tries._
- _If he gets a snake or heron in the last (tenth) turn, the player gets one or two more bonus tries, respectively. These bonus tries are taken as part of the same turn. If the bonus tries knock over all the frogs, the process does not repeat: the bonus tries are only used to calculate the score of the final turn._
- _The game score is the total of all turn scores._

_Here are some things that the program will not do:_

- _it will not check for valid tries._
- _it will not check for correct number of tries and turns._
- _it will not provide scores for intermediate turns. »_

_Le code produit doit être entièrement en anglais. Génère également les tests unitaires associés aux éléments de code produits en utilisant la syntaxe de la librairie Vitest (describe, it, expect)._

La trame est la même que le tout premier : des phrases de consignes en français au début et à la fin, mais un énoncé en anglais.

En revanche, **plus question de bowling&nbsp;! Cette fois, on joue au "Frog Tumble"**, un jeu qui consiste à dégommer des grenouilles. Les notions de "spare" et "strike" existent à l'identique, à l'exception qu'elles s'appellent "snake" et "heron" (les prédateurs des grenouilles, selon l'inspiration du moment...).

> Pourquoi des grenouilles&nbsp;?!...

Je ne sais pas&nbsp;😅

### Et c'est le drame...

Et voilà ce que me répond ChatGPT&nbsp;:

```ts
type Turn = number[];

export function calculateScore(turns: Turn[]): number {
  let totalScore = 0;

  for (let i = 0; i < turns.length; i++) {
    const turn = turns[i];
    const firstTry = turn[0];
    const secondTry = turn[1] || 0; // Default to 0 if no second try

    const frogsKnockedOver = firstTry + secondTry;
    const allFrogsKnockedOver = frogsKnockedOver === 10;

    if (allFrogsKnockedOver) {
      // Snake case
      totalScore += 10;
      if (i + 1 < turns.length) {
        totalScore += turns[i + 1].reduce((a, b) => a + b, 0);
      }
    } else {
      // Not all knocked over
      totalScore += frogsKnockedOver;

      if (firstTry === 10) {
        // Heron case
        totalScore += 10;
        if (i + 1 < turns.length) {
          totalScore += turns[i + 1].reduce((a, b) => a + b, 0);
        }
      }
    }

    // Handle bonus tries for the last turn
    if (i === 9) {
      if (allFrogsKnockedOver) {
        // Bonus try for snake
        if (turn.length > 2) {
          totalScore += turn[2] || 0;
        }
      } else if (firstTry === 10) {
        // Bonus try for heron
        if (turn.length > 2) {
          totalScore += turn[2] || 0;
        }
      }
    }
  }

  return totalScore;
}
```

Et les tests unitaires qui vont avec&nbsp;:

```ts
describe("Frog Tumble Score Calculation", () => {
  it("should calculate score for a game with no snakes or herons", () => {
    const turns = [
      [3, 4],
      [2, 3],
      [1, 2],
      [0, 5],
      [4, 1],
      [3, 2],
      [5, 0],
      [2, 2],
      [1, 1],
      [0, 0],
    ];
    expect(calculateScore(turns)).toBe(30); // ❌ La fonction renvoie 41
  });

  it("should calculate score with a snake", () => {
    const turns = [
      [5, 5],
      [10], // Snake
      [2, 3],
      [1, 2],
      [0, 5],
      [4, 1],
      [3, 2],
      [5, 0],
      [2, 2],
      [1, 1],
    ];
    expect(calculateScore(turns)).toBe(40); // ❌ La fonction renvoie 69
  });

  it("should calculate score with a heron", () => {
    const turns = [
      [10], // Heron
      [2, 3],
      [1, 2],
      [0, 5],
      [4, 1],
      [3, 2],
      [5, 0],
      [2, 2],
      [1, 1],
      [0, 0],
    ];
    expect(calculateScore(turns)).toBe(42); // ❌ La fonction renvoie 49
  });

  it("should handle bonus tries correctly", () => {
    const turns = [
      [10], // Heron
      [10], // Snake
      [2, 3],
      [1, 2],
      [0, 5],
      [4, 1],
      [3, 2],
      [5, 0],
      [2, 2],
      [1, 1, 2], // Bonus try
    ];
    expect(calculateScore(turns)).toBe(62); // ❌ La fonction renvoie 69
  });
});
```

Bon... on peut dire que c'est bien nul, non&nbsp;?&nbsp;😣

Pour commencer, les tests sont tout aussi faux que précédemment&nbsp;: ils ne valident ni les règles de l'énoncé, ni le comportement du code fourni.

Le code lui-même est **purement procédural**. Une pseudo-traduction des consignes en structure `if/else`. **Aucune modélisation, aucune extraction de notion métier**, comme les fonctions `isSpare` ou `isStrike` dans le premier résultat. On peut décompter jusqu'à **3 niveaux d'imbrication** de conditions...

Et, surtout, le code ne fonctionne pas&nbsp;:

- Le "strike, enfin "heron", est mal géré (un "heron" est considéré comme un "snake" à cause du mauvais ordre des conditions dans l'algo...)
- Les règles du dernier tour sont également mal gérées

Bref, je pense qu'**on peut dire que tout est à jeter**. Même en corrigeant manuellement le code ET les tests, le résultat serait moins bon que ce qu'on pourrait produire manuellement en suivant une méthode de dev incrémentale bien connue...

### Déterminisme

J'en ai profité pour tester à nouveau le déterminisme, en fournissant à nouveau le même prompt.

Et, surprise&nbsp;! (ou pas...) Lorsque les ressources sont moins abondantes sur le sujet, il y a beaucoup plus de variation dans les résultats&nbsp;!&nbsp;😅

Les algorithmes proposés pour le "Frog Tumble" sont beaucoup plus variables. Mais tous aussi mauvais les uns que les autres. En particulier, le travers de traiter les spécifications dans l'ordre et d'occulter ainsi les strikes avec les spares est présent à chaque fois.

Ce n'est pas le produit d'une logique algorithmique, c'est simplement une transcription d'un langage humain vers un langage informatique...

# Conclusion

Que puis-je conclure de mes petits essais&nbsp;?

«&nbsp;**_l'IA n'est capable de résoudre efficacement que les problèmes qui ont déjà été résolus par des nombreuses autres "sources"_**&nbsp;»

Je pense que cela résume assez bien mon opinion... Si le problème qu'on cherche a résoudre a déjà été poncé, vu et revu, publié partout sur Internet, alors l'IA peut aider à le résoudre rapidement. C'est le cas du score du bowling ici, car c'est un kata de code très populaire.

Mais nous avons bien constaté que, **lorsque j'ai opacifié suffisamment le sujet** pour que l'IA ne puisse pas identifier le jeu du "bowling", **la qualité du résultat s'est grandement dégradée** (et encore, je trouve que j'ai été très "gentil" dans mon opacification... il a suffi de changer quelques termes clés pour que l'IA soit perdue). Les consignes ont alors été traitées de façon très basique dans l'ordre des spécifications, causant d'ailleurs un bug dans l'implémentation fournie (spare/strike).

Mon interprétation est la suivante&nbsp;: faute d'exemple de code propre répondant au problème à disposition, l'IA a simplement tenté de traduire les phrases dans le langage TypeScript, _sans réfléchir_.

C'est une conclusion alternative&nbsp;: **l'IA ne réfléchit pas, elle ne fait qu'agréger ce qui existe déjà**. Elle est capable de faire l'exercice de recherche documentaire extrêmement rapidement. En revanche, comprendre une logique et la traduire en un algorithme performant et maintenable n'est pas encore de son niveau.

_L'IA peut également réaliser des tâches "bêtes", automatiques, plus rapidement qu'un humain. Mais ce n'était pas le sujet ici._

Dans tous les cas, je suis **très déçu par les tests unitaires générés par l'IA**, peu importe la forme de l'énoncé. Ils étaient systématiquement médiocres (en grande partie car ils ne validaient pas le bon comportement). Et cette incapacité à interpréter la logique pour la traduire en cas de test l'empêche de vérifier le résultat qu'elle fournit...&nbsp;🫤

Il existe des solutions basées sur l'IA plus performantes qu'un ChatGPT 4o-mini... Mais je pense tout de même que leurs forces et faiblesses sont similaires.

Si vous travaillez sur un sujet "obscur" (au sens&nbsp;: un sujet sur lequel peu d'humains ont publié du code accessible aux IA), il y a de fortes probabilités que l'IA ne vous aide pas beaucoup... Et qu'il soit, pour le moment, plus prudent d'utiliser votre cerveau.

C'est tout pour aujourd'hui. À bientôt&nbsp;!&nbsp;😉
