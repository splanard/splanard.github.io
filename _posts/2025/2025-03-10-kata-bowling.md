---
layout: post
title: "Kata : Bowling"
date: 2025-03-10 13:30:00 +0100
tags: article dev kata
---

Toute personne qui a joué au bowling une fois dans sa vie se sera peut-être aperçue qu'il n'est pas évident de comprendre le score affiché sur le tableau de marque 😁.

Les règles de décompte des points sont pourtant relativement simples. La seule subtilité : le score d'un "tour" de jeu peut impacter les tours de jeu suivants...

Ce kata de code, qu'un collègue m'a récemment proposé de faire avec lui, est [disponible sur Coding Dojo dans sa forme initiale](https://codingdojo.org/kata/Bowling/).

La semaine dernière, je l'ai refait, seul. Je vous décris ici l'approche que j'ai eue, telle qu'elle s'est construite au fur et à mesure du déroulement du kata. J'aurais pu optiminser certaines choses après coup, corriger certaines erreurs que j'ai faites en cours de route, pour paraître "meilleur" dès la première approche&nbsp;✨. Mais j'aurais sacrifié la démarche intellectuelle (ainsi que l'honnêteté&nbsp;🙄) sur l'autel de l'image finale. Et c'est l'inverse de ce que je souhaite proposer, ici, lorsque je décris mes katas.

C'est parti.

⚠️ _Attention, c'est (très) long..._&nbsp;😅

# Revue de l'énoncé

Une chose dont on s'aperçoit rapidement en déroulant l'énoncé, c'est qu'il est **très imprécis vis-à-vis des termes employés**. Pour la même notion, de multiples termes différents sont employés (ex&nbsp;: « roll », « throw », « try »). Je ne sais pas si c'est volontaire ou pas. C'est peut-être pour embrouiller les _karatékas_ (🤔... les personnes qui réalisent le kata, quoi&nbsp;!) et les forcer à se poser des questions. Ou c'est un simple manque de rigueur de la personne qui l'a écrit. Mais la première hypothèse me paraît plus plausible. Passons.

Dans tous les cas, la première chose qu'on a envie de faire en abordant ce kata, c'est de **le reformuler** avec des termes précis.

J'en profite pour le traduire en français, par la même occasion (même si le code, lui, sera en anglais, par habitude...). _J'en profite aussi pour glisser un mot sur la notion de "ubiquitous language", issue du Domain Driven Design_ 😉. _Renseignez-vous, c'est super intéressant&nbsp;!_

## Énoncé reformulé

Le but est de créer un programme qui, **lorsqu'on lui fournit une séquence valide de lancers** pour un joueur d'une partie de bowling américain à 10 quilles, **calcule le score total** du joueur pour la partie.

Ce que le programme **ne fera pas**&nbsp;:

- Vérifier que les lancers sont valides (que le nombre de quilles tombées est inférieur ou égal à 10, par exemple)
- Vérifier le nombre de lancers et/ou de carreaux fournis
- Fournir le score intermédiaire pour chaque carreau (seul le score final est calculé)

L'objectif du kata est de **se concentrer sur l'implémentation des règles de calcul** (même si, dans une application réelle, les fonctionnalités listées ci-dessus pourraient s'avérer nécessaires).

Voici le résumé des règles de calculs du score de ce style de bowling, qui nous servira de spécifications pour ce kata&nbsp;:

- Chaque **partie** contient **10 carreaux**

- Lors de chaque carreau, le joueur possède **jusqu'à 2 lancers** pour tenter de faire tomber toutes les quilles

- Si, après 2 lancers, il n'a pas réussi à faire tomber les 10 quilles, son score pour ce carreau est égal au nombre de quilles tombées après les 2 lancers.

- Si, après 2 lancers, les 10 quilles sont tombées, on appelle cela un "**spare**" (parce que je trouve le terme "réserve", trouvé sur la page Wikipédia française, vraiment trop moche...&nbsp;😅). Le score pour ce carreau est alors de 10 plus le nombre de quilles qui tomberont **lors de son prochain lancer**.

- Si, après le premier lancer du carreau, les 10 quilles sont tombées, on appelle cela un "**strike**" (parce que je refuse d'appeler cela un "abat"...). Le carreau est alors terminé, il n'y a pas de second lancer. Le score pour ce carreau est alors de 10 plus la somme du nombre de quilles qui tomberont **lors de ses deux prochains lancers**.

- Si le joueur obtient **un spare ou un strike lors du dernier (10e) carreau**, il obtient respectivement 1 ou 2 lancers supplémentaires. Ces lancers additionnels font partie du même carreau. Peu importe le nombre de quilles renversées lors de ces lancers additionnels, aucun autre lancer ne sera octroyé. Ils sont simplement utilisé pour tenir compte de l'effet du spare ou du strike sur le score final.

- Le score total de la partie est la somme du score de chaque carreau.

Et, enfin, la traduction des termes employés en anglais, qui fera référence pour la durée de ce kata&nbsp;:

| Français | Anglais |
| -------- | ------- |
| partie   | line    |
| carreau  | frame   |
| lancer   | roll    |
| quille   | pin     |

# Stack technique

J'ai déjà partiellement réalisé ce kata une première fois, assez récemment, en Java.

Pour me forcer à changer d'environnement et d'approche, je vais donc basculer cette fois-ci sur **TypeScript**&nbsp;! Langage que j'apprécie de plus en plus... L'outil de build sera `Vite`, la librairie de test sera `Vitest`.

Après quelques commandes pour initialiser mon environnement, je suis prêt à démarrer.

# À l'attaque&nbsp;!

Après ce prologue tout à fait conséquent (😅), on va peut-être pouvoir se lancer, non&nbsp;?!

## Par où commencer&nbsp;?

Je réalise le kata avec une approche TDD, donc il me faut mon premier cas de test. Par quoi commencer&nbsp;?&nbsp;🤔

Si je reprends l'énoncé, la phrase suivante me donne mes entrants et sortants&nbsp;:

> Le but est donc de créer un programme qui, **lorsqu'on lui fournit une séquence valide de lancers** pour un joueur d'une partie de bowling américan à 10 quilles, **calcule le score total** du joueur pour la partie.

- **En entrée**, je vais fournir une séquence de lancers (donc, de façon basique, **le nombre de quilles tombées lors de chacun de ces lancers**).
- **En sortie**, je vais vérifier que **le score total du joueur** en fin de partie est correct

Mais je n'ai toujours pas mon cas de test...

Je vais chercher à valider un comportement, donc un calcul dans le cas présent. Si je continue à dérouler les règles mentionnées dans l'énoncé&nbsp;:

> - Chaque **partie** contient **10 carreaux**
> - Lors de chaque carreau, le joueur possède **jusqu'à 2 lancers** pour tenter de faire tomber toutes les quilles

Aucune de ces deux affirmations ne traduit un réel comportement de mon moteur de calcul de score. Et mon programme n'est pas censé valider les données. Donc ces deux affirmations ne m'intéressent pas pour le moment...

En revanche, si j'analyse la troisième&nbsp;:

> Si, après 2 lancers, il n'a pas réussi à faire tomber les 10 quilles, **son score pour ce carreau est égale au nombre de quilles tombées** après les 2 lancers.

Là, j'ai une vraie règle de calcul, quelque chose que je vais pouvoir vérifier&nbsp;! (En somme, j'ai le contenu de mon assertion, qui me manquait jusqu'à présent).

## Premier test

Et voilà donc mon premier cas de test&nbsp;:

```ts
import { describe, expect, it } from "vitest";

describe("when calculating the score of a bowling line", () => {
  it("adds up the number of pins knocked down", () => {
    expect(scoreFor(3, 4)).toBe(7);
  });
});

const scoreFor = (firstRoll: number, secondRoll: number): number => {
  return -1;
};
```

Déjà, j'exploite dans mon test le fait que mon code ne validera pas les données fournies (comme spécifié dans l'énoncé). Je peux donc me permettre de ne lui passer que 2 lancers. Ce qui, en soit, n'est pas une partie de bowling complète, mais cela ne devrait pas gêner mon algorithme pour le moment. Est-ce une bonne idée&nbsp;? Ça peut se discuter. Dans tous les cas, la portion d'énoncé que j'ai choisie de traiter en premier ne concerne qu'un seul carreau, donc 2 lancers. Pas plus.

J'explique la fonction `scoreFor`. C'est quelque chose qui m'a été suggéré par mon collègue lorsque nous avons fait ce kata ensemble. Par habitude, j'aurais plutôt appelé la classe réelle directement dans le test.

**Cependant**... il y a deux choses intéressantes à noter concernant cette pratique&nbsp;:

- D'une part, je vais procéder de façon itérative. Donc **l'utilisation que je fais des classes/fonctions que je vais produire pourra évoluer dans le temps**. Auquel cas, je ne souhaite pas forcément devoir repasser sur tous mes tests précédents.

Ici, la fonction `scoreFor` me permet d'encapsuler la partie "_comment passer les informations au code que je vais produire_". Ainsi, si cette façon de fournir les entrants change, je n'aurai que cette fonction à modifier.

- D'autre part, cette pratique permet de dissocier&nbsp;:
  - la définition de mon cas de test
  - la réflexion concernant l'implémentation

Et traiter ces deux problématiques séparément me semble une bonne idée. C'est la première fois que je procède de la sorte&nbsp;: je verrai bien ce que ça donne&nbsp;!&nbsp;😉

## Première implémentation

S'en suit donc une réflexion sur l'orientation que je vais choisir pour l'implémentation...

Lors de ma précédente réalisation de ce kata, j'avais très rapidement modélisé un carreau. Quelque chose de ce genre&nbsp;: `new BowlingScore( new Frame(3, 4) ).getValue()`. Si l'apparition de cette `Frame` dans le code avait apporté certains avantages, elle avait aussi posé quelques problèmes. Et nous nous étions posé la question «&nbsp;_A-t-elle un réel intérêt&nbsp;?_&nbsp;».

Donc, cette fois, je choisis de ne pas la faire apparaître pour le moment. Et de voir où cela me mène...

Et, comme je suis en TypeScript, j'ai toujours le choix de partir sur de la POO ou des fonctions exportées. N'ayant pas d'avis tranché sur l'un ou l'autre pour le moment, je choisis d'utiliser la POO. Il sera toujours temps de changer si je trouve une bonne raison de le faire.

Voici donc la version finale de mon premier test&nbsp;:

```ts
describe("when calculating the score of a bowling line", () => {
  it("adds up the number of pins knocked down", () => {
    expect(scoreFor(3, 4)).toBe(7);
  });
});

const scoreFor = (firstRoll: number, secondRoll: number): number => {
  return new BowlingScore(firstRoll, secondRoll).calculate();
};
```

Pour pouvoir exécuter mon test, je crée ma classe et la méthode&nbsp;:

```ts
class BowlingScore {
  constructor(firstRoll: number, secondRoll: number) {}

  calculate(): number {
    return -1;
  }
}
```

Je vérifie que mon test échoue&nbsp;❌, puis l'implémentation est très rapide&nbsp;✅.

```ts
class BowlingScore {
  private firstRoll: number;
  private secondRoll: number;

  constructor(firstRoll: number, secondRoll: number) {
    this.firstRoll = firstRoll;
    this.secondRoll = secondRoll;
  }

  calculate(): number {
    return this.firstRoll + this.secondRoll;
  }
}
```

# On déroule...

Maintenant que j'ai des tests fonctionnels et une première base de code, l'itération va être plus fluide. C'est toujours le premier le plus long à écrire.&nbsp;😅

En revanche, comme souvent dans un processus itératif, il faut maintenant se poser la question de "la prochaine étape".

## Ordre des spécifications VS ordre d'implémentation

Si on prend l'affirmation suivante dans l'énoncé&nbsp;:

> Si, après 2 lancers, les 10 quilles sont tombées, on appelle cela un "**spare**". Le score pour ce carreau est alors de 10 plus le nombre de quilles qui tomberont lors de **son prochain lancer**.

On s'aperçoit vite que ce n'est pas la prochaine étape logique. Pas la plus simple, du moins. Cette portion de règles contient déjà la notion que plusieurs carreaux s'enchaînent dans une partie + introduit la notion de "spare" et l'impact qu'il aura sur le score des prochains lancers. Or, dans mon implémentation actuelle, je ne traite qu'un seul carreau...

Il existe une étape intermédiaire. Mais, pour la voir, il faut lire l'énoncé jusqu'au bout.&nbsp;😊

> Le score total de la partie est la somme du score de chaque carreau.

**C'est celle-là, la prochaine étape&nbsp;!**

Une fois que mon code sera capable de gérer plusieurs carreaux, et seulement à ce moment-là, je pourrai introduire la notion de "spare". Pas avant.

_De façon générale, l'ordre dans lequel sont écrites les spécifications n'est pas forcément l'ordre dans lequel les développeurs doivent implémenter la solution. C'est de la responsabilité des développeurs de découper un problème en plusieurs sous-problèmes, puis d'identifier dans quel ordre les traiter, pour être le plus efficace possible, ne pas se "marcher dessus", apporter de la valeur plus rapidement, etc._

## Gestion d'une partie complète

Voilà donc quel pourrait être mon prochain cas de test&nbsp;:

```ts
it("adds up the score of all the line's frames", () => {
  expect(scoreFor(3, 4, 1, 6, 0, 0, 5, 0, 4, 1, 7, 2, 3, 3, 8, 0, 1, 2, 5, 3)).toBe(58);
});
```

Rien qu'à l'écrire la première fois, en faisant bien attention à ce qu'aucun des carreaux ne donne un spare, j'avais déjà mal à la tête...&nbsp;😫

J'aurais pu le laisser en l'état et passer à l'implémentation, mais j'ai préféré améliorer d'abord la lisibilité de mes cas de test. Parce que je sais que je devais devoir en écrire d'autres, très bientôt...

Sans trop me casser la tête, j'en arrive à ce format alternatif&nbsp;:

```ts
it("adds up the score of all the line's frames", () => {
  expect(scoreFor("3.4 1.6 0.0 5.0 4.1 7.2 3.3 8.0 1.2 5.3")).toBe(58);
});
```

J'ai bien imaginé, au début, omettre le `.`. Mais comment aurais-je différencié, à terme, 1 puis 0 (donc un lancer bien nul) d'un strike&nbsp;? C'est de l'anticipation, mais c'est sur mon code utilitaire de test, donc je m'octroie ce droit à l'anticipation...

Voilà ce que donne le test complet&nbsp;:

```ts
describe("when calculating the score of a bowling line", () => {
  it("adds up the number of pins knocked down", () => {
    expect(scoreFor("3.4")).toBe(7);
  });

  it("adds up the score of all the line's frames", () => {
    expect(scoreFor("3.4 1.6 0.0 5.0 4.1 7.2 3.3 8.0 1.2 5.3")).toBe(58);
  });
});

const scoreFor = (scoreAsString: string): number => {
  const rollsScores = rawStringToRolls(scoreAsString);
  return new BowlingScore(rollsScores).calculate();
};

const rawStringToRolls = (scoreAsString: string): number[] => {
  return scoreAsString
    .split(" ")
    .flatMap((frameScore) => frameScore.split("."))
    .map((rollScoreAsString) => parseInt(rollScoreAsString));
};
```

La fonction `rawStringToRolls` n'est pas des plus élégantes. Mais cette fonction, couplée à `scoreFor` n'ont aucune autre vocation que de rendre mes tests plus lisibles. Et, pour le moment, j'estime qu'elles remplissent ce rôle.

Passons à l'implémentation.

```ts
class BowlingScore {
  private rolls: number[];

  constructor(rolls: number[]) {
    this.rolls = rolls;
  }

  calculate(): number {
    return this.rolls.reduce((currentLineScore, rollScore) => currentLineScore + rollScore, 0);
  }
}
```

Sur ce coup-là, les tests m'ont pris bien plus de temps à écrire que le code lui-même&nbsp;😅. C'est la vie, on enchaîne.

# Spare&nbsp;!

Je peux maintenant attaquer cette section de l'énoncé&nbsp;!

> Si, après 2 lancers, les 10 quilles sont tombées, on appelle cela un "**spare**". Le score pour ce carreau est alors de 10 plus le nombre de quilles qui tomberont lors de **son prochain lancer**.

Étant donné que faire des additions de chiffres ne me passionne pas plus que ça, je rebascule sur un cas beaucoup plus simple (toujours en exploitant le fait que mon code n'est pas censé faire de contrôle de validité des données).

```ts
it("adds the next roll in case of a spare", () => {
  expect(scoreFor("7.3 4.2")).toBe(20);
});
```

Mon test est bien cassant&nbsp;: mon code reçoit 16 au lieu de 20, ce qui est tout à fait normal. Et ça me confirme dans l'idée que les tests sont plus visuels et plus faciles à écrire comme ceci.

Côté implémentation, ça se complique&nbsp;😅.

```ts
class BowlingScore {
  private rolls: number[];

  constructor(rolls: number[]) {
    this.rolls = rolls;
  }

  calculate(): number {
    let lineScore = 0;
    let spareActive = false;
    this.rolls.forEach((roll: number, index: number) => {
      lineScore += roll;
      if (spareActive) {
        lineScore += roll;
      }
      spareActive = this.isSpare(index);
    });
    return lineScore;
  }

  private isSpare(rollIndex: number): boolean {
    return this.rolls[rollIndex] + this.rolls[rollIndex - 1] === 10;
  }
}
```

Ça fonctionne. Mais je n'en suis pas pleinement satisfait... J'ai le sentiment que ma classe porte maintenant plusieurs responsabilités. La détection du "spare", notamment, pourrait être déléguée à une autre. J'aurais envie de pouvoir écrire la chose suivante&nbsp;:

```ts
calculate(): number {
  let lineScore = 0;
  let spareActive = false;
  this.rolls.forEach((roll: number, index: number) => {
    lineScore += roll;
    if (spareActive) {
      lineScore += roll;
    }
    spareActive = frames.isSpare(index); // ne compile pas, car "frames" n'existe pas...
  });
  return lineScore;
}
```

Eh bah... y'a plus qu'à&nbsp;!&nbsp;😝 C'est à ça que sert la phase de refactoring après tout...

Cette fois-ci, je vais développer une autre classe, avec sa propre responsabilité&nbsp;: identifier les résultats de chaque frame. En particulier, les résultats spéciaux comme le "spare". Mais j'ai déjà en tête que je m'en resservirai plus tard pour le "strike"...

Je vais donc procéder en TDD avec cette classe aussi. Et voilà ce que ça donne à la fin.

**Frames.spec.ts**

```ts
describe("when analyzing the frames from separate rolls", () => {
  const frames = new Frames(3, 4, 5, 5);

  it("identifies a standard frame", () => {
    expect(frames.isSpare(1)).toBe(false);
  });

  it("identifies a spare frame", () => {
    expect(frames.isSpare(3)).toBe(true);
  });
});
```

**Frames.ts**

```ts
class Frames {
  private rolls: number[];

  constructor(...rolls: number[]) {
    this.rolls = rolls;
  }

  isSpare(rollIndex: number): boolean {
    return this.rolls[rollIndex] + this.rolls[rollIndex - 1] == 10;
  }
}
```

Je n'ai plus qu'à l'intégrer dans mon code principal, et le tour est joué&nbsp;:

```ts
class BowlingScore {
  private frames: Frames;
  private rolls: number[];

  constructor(rolls: number[]) {
    this.rolls = rolls;
    this.frames = new Frames(...rolls);
  }

  calculate(): number {
    let lineScore = 0;
    let spareActive = false;
    this.rolls.forEach((roll: number, index: number) => {
      lineScore += roll;
      if (spareActive) {
        lineScore += roll;
      }
      spareActive = this.frames.isSpare(index);
    });
    return lineScore;
  }
}
```

## Trop de "spare"...

Je vous entends d'ici venir avec vos gros sabots&nbsp;: «&nbsp;_Tu n'as pas bien géré les spares&nbsp;! Tu vas avoir des faux positifs_&nbsp;».

Je sais&nbsp;🙂. Mais chaque chose en son temps.

Voilà le cas qui met en défaut l'implémentation actuelle&nbsp;:

```ts
expect(scoreFor("2.7 3.6")).toBe(18); // Mon implémentation actuelle renvoie 24
```

Du fait de mon implémentation assez naïve de la détection du "spare", elle en détecte trop. Ici, le 6 est compté en double dans le score final à cause du manque de séparation des carreaux&nbsp;: le 7 du premier carreau additionné au 3 du second carreau active la détection du "spare".

Pour savoir où ajouter mon test, je me pose la question&nbsp;: qui porte cette reponsabilité&nbsp;?

La classe `Frames`, effectivement.

Je vais donc compléter les tests...

```ts
describe("when analyzing the frames from separate rolls", () => {
  const frames = new Frames(3, 4, 6, 4);

  it("identifies a standard frame", () => {
    expect(frames.isSpare(1)).toBe(false);
  });

  it("doesn't identify a spare on the first roll of a frame", () => {
    expect(frames.isSpare(2)).toBe(false);
  });

  it("identifies a spare frame", () => {
    expect(frames.isSpare(3)).toBe(true);
  });
});
```

... puis l'implémentation.

```ts
class Frames {
  private rolls: number[];

  constructor(...rolls: number[]) {
    this.rolls = rolls;
  }

  isSpare(rollIndex: number): boolean {
    return this.isSecondRoll(rollIndex) && this.rolls[rollIndex] + this.rolls[rollIndex - 1] == 10;
  }

  private isSecondRoll(rollIndex: number): boolean {
    return rollIndex % 2 === 1;
  }
}
```

Et tout est rentré dans l'ordre&nbsp;!&nbsp;😊

# Strike&nbsp;!

Le "spare" me semble géré (pour le moment...). J'attaque le "strike"&nbsp;!

> Si, après le premier lancer du carreau, les 10 quilles sont tombées, on appelle cela un "**strike**" (parce que je refuse d'appeler ça un "abat"...). Le carreau est alors terminé, il n'y a pas de second lancer. Le score pour ce carreau est alors de 10 plus la somme du nombre de quilles qui tomberont lors de **ses deux prochains lancers**.

Juste après avoir codé la détection du spare sur le second lancer uniquement, en relisant cette nouvelle section de spécification, je sais déjà que je vais avoir des problèmes... Mais je les gèrerai en temps voulu.

## Cas nominal

J'ajoute mon nouveau cas de test, toujours le plus simple possible&nbsp;:

```ts
describe("when calculating the score of a bowling line", () => {
  // [...]

  it("adds the two following rolls in case of a strike", () => {
    expect(scoreFor("10 2.3")).toBe(20);
  });
});
```

Je commence par vérifier si mes fonctions utilitaires de test traitent correctement ce nouveau cas (oui&nbsp;😎). Puis je me dis que ma nouvelle classe `Frames` est toute désignée pour identifier un strike.

**Frames.spec.ts**

```ts
describe("when analyzing the frames from separate rolls", () => {
  const frames = new Frames(3, 4, 6, 4, 10);

  // [...]

  it("identifies a strike frame", () => {
    expect(frames.isStrike(4)).toBe(true); // ❌ nouveau test cassant
  });
});
```

**Frames.ts**

```ts
class Frames {
  private rolls: number[];

  constructor(...rolls: number[]) {
    this.rolls = rolls;
  }

  isSpare(rollIndex: number): boolean {
    return this.isSecondRoll(rollIndex) && this.rolls[rollIndex] + this.rolls[rollIndex - 1] == 10;
  }

  // Je n'ai qu'à ajouter cette méthode, et c'est réglé ! ✅
  isStrike(rollIndex: number): boolean {
    return this.rolls[rollIndex] === 10;
  }

  private isSecondRoll(rollIndex: number): boolean {
    return rollIndex % 2 === 1;
  }
}
```

Sauf qu'en ajoutant ce cas, je me rends bien compte que j'ai cassé quelque chose. Même si mes tests actuels ne le reflètent pas... Le voyez-vous&nbsp;?

## Meilleure détection des spare/strike

Ma détection du fait que le lancer courant soit le second d'un carreau (pour détecter le cas "spare") se base sur l'index. Sauf que, via le "strike", je viens d'introduire le carreau à 1 seul lancer... 🫤 Donc ça ne fonctionne plus du tout, mon histoire&nbsp;!

J'aurais pu passer à côté. Je pourrais aussi continuer la gestion du "strike", qui n'est pas terminée, et revenir à ce problème plus tard.

Mais non.

Je vais le gérer maintenant. Pourquoi&nbsp;?

Parce que je pourrais oublier en cours de route. J'ai de toute façon un test cassant qui me rappelle que je dois gérer les points bonus liés au "strike". Je n'ai aucun test cassant pour me rappeler de gérer ce nouveau problème de détection du "spare". Je vais donc, au moins, créer ce cas de test.

**Frames.spec.ts**

```ts
describe("when analyzing the frames from separate rolls", () => {
  const frames = new Frames(3, 4, 6, 4, 10, 7, 3); // J'ajoute un nouveau spare, après le strike

  it("identifies a standard frame", () => {
    expect(frames.isSpare(1)).toBe(false);
  });

  it("doesn't identify a spare on the first roll of a frame", () => {
    expect(frames.isSpare(2)).toBe(false);
  });

  it("identifies a spare frame", () => {
    expect(frames.isSpare(3)).toBe(true);
    expect(frames.isSpare(6)).toBe(true); // ❌ ça casse ici !
  });

  it("identifies a strike frame", () => {
    expect(frames.isStrike(4)).toBe(true);
  });
});
```

Maintenant, libre à moi de choisir ce que je veux traiter en premier&nbsp;: terminer la gestion des points attribués par le "strike" ou corriger la détection du "spare" après un "strike".

Je choisis de corriger d'abord le comportement de la classe que je viens de modifier. Je suis dedans, je veux laisser quelque chose de propre et fonctionnel en "partant".

Et voilà ma première solution fonctionnelle...

```ts
class Frames {
  private rolls: number[];

  constructor(...rolls: number[]) {
    this.rolls = rolls;
  }

  isSpare(rollIndex: number): boolean {
    return this.isSecondRoll(rollIndex) && this.rolls[rollIndex] + this.rolls[rollIndex - 1] == 10;
  }

  isStrike(rollIndex: number): boolean {
    return this.rolls[rollIndex] === 10;
  }

  private isSecondRoll(rollIndex: number): boolean {
    const indexOfLastPreviousStrike = this.indexOfLastPreviousStrike(rollIndex);
    if (indexOfLastPreviousStrike > 0) {
      return (rollIndex - indexOfLastPreviousStrike) % 2 === 0;
    }
    return rollIndex % 2 === 1;
  }

  private indexOfLastPreviousStrike(rollIndex: number): number {
    for (let i = rollIndex - 1; i >= 0; i--) {
      if (this.isStrike(i)) {
        return i;
      }
    }
    return 0;
  }
}
```

Ça commence à devenir compliqué...&nbsp;😣

Je profite donc de ma phase de _refactoring_ pour tenter une approche plus simple. Étant donné que mes tests existants m'assurent la non-régression fonctionnelle, je peux tout casser si j'en ai envie&nbsp;! Et c'est ce que je décide de faire, pour adopter une approche différente&nbsp;: parcourir les lancers à la création de la classe, pour stocker progressivement les indices de tous les spare/strike de la partie.

Le résultat&nbsp;:

```ts
class Frames {
  private spares: number[] = [];
  private strikes: number[] = [];

  constructor(...rolls: number[]) {
    let rollIndex = 0;
    while (rollIndex < rolls.length) {
      if (rolls[rollIndex] === 10) {
        this.strikes.push(rollIndex);
        rollIndex++;
        continue;
      }

      if (rolls[rollIndex] + rolls[rollIndex + 1] === 10) {
        this.spares.push(rollIndex + 1);
      }
      rollIndex += 2;
    }
  }

  isSpare(rollIndex: number): boolean {
    return this.spares.includes(rollIndex);
  }

  isStrike(rollIndex: number): boolean {
    return this.strikes.includes(rollIndex);
  }
}
```

Tous mes tests passent ✅. Et je trouve que, niveau complexité, j'ai quand même gagné au change&nbsp;! Donc je décide de conserver cette nouvelle version, en attendant les prochains ennuis&nbsp;😅.

## Retour sur les bonus du strike

Allez, j'ai fini de corriger ma classe `Frames`&nbsp;: je retourne m'occuper de l'effet de mon "strike" sur les points en fin de partie. Vous savez, le sujet sur lequel j'étais parti il y looooooongtemps...&nbsp;🙄

```ts
describe("when calculating the score of a bowling line", () => {
  // [...]

  it("adds the two following rolls in case of a strike", () => {
    expect(scoreFor("10 2.3")).toBe(20); // ❌ ça casse toujours ici...
  });
});
```

Et maintenant que ma classe `Frames` fait correctement son travail, l'implémentation est très rapide.

```ts
class BowlingScore {
  private frames: Frames;
  private rolls: number[];

  constructor(rolls: number[]) {
    this.rolls = rolls;
    this.frames = new Frames(...rolls);
  }

  calculate(): number {
    let lineScore = 0;
    this.rolls.forEach((roll: number, index: number) => {
      lineScore += roll;

      if (index > 0 && (this.frames.isSpare(index - 1) || this.frames.isStrike(index - 1))) {
        lineScore += roll;
      }
      if (index > 1 && this.frames.isStrike(index - 2)) {
        lineScore += roll;
      }
    });
    return lineScore;
  }
}
```

Ça me paraît encore suffisamment simple pour le moment. Donc je laisse en l'état.

Par acquis de conscience, je me pose tout de même la question des cas aux limites. Si le joueur obtient 2 "strike" de suite, par exemple, est-ce que mon code actuel le gère&nbsp;?

```ts
describe("when calculating the score of a bowling line", () => {
  // [...]

  it("adds the two following rolls in case of a strike", () => {
    expect(scoreFor("10 2.3")).toBe(20);
    expect(scoreFor("10 10 2.3")).toBe(42); // ça passe ! ✅
  });
});
```

Là, j'ai ajouté un test qui passe directement. Je n'avais pourtant pas anticipé ce cas en codant mon algo, mais il se trouve que ma version actuelle le gère déjà.&nbsp;😎

Si on suit la méthode TDD à la lettre, ce nouveau test ne sert à rien. Mais j'estime qu'il permet tout de même de sécuriser ma non-régression en couvrant un cas aux limites, donc je décide de conserver cette nouvelle assertion.

# Spare ou strike en fin de partie

Il me reste une dernière partie d'énoncé à traiter.

> Si le joueur obtient un spare ou un strike lors du dernier (10e) carreau, il obtient respectivement 1 ou 2 lancers supplémentaires. Ces lancers additionnels font partie du même carreau. Peu importe le nombre de quilles renversé lors de ces lancers additionnels, aucun autre lancer ne sera octroyé. Ils sont simplement utilisé pour tenir compte de l’effet du spare ou du strike sur le score final.

Et en la lisant, je me demande si j'ai vraiment quoi que ce soit à modifer dans mon code...&nbsp;🤔

Il était mentionné dans l'énoncé que le programme n'avait pas besoin de valider les données en entrée. Donc que je reçoive des lancers supplémentaires ne devrait pas l'impacter. Il les traitera exactement de la même façon. Et ce n'est pas non plus mon programme qui "octroie" les lancers, il se contente de calculer le score à la fin. Non&nbsp;?

Pour en avoir le cœur net, je reprends les cas de test suggérés dans l'énoncé initial&nbsp;:

> - X X X X X X X X X X X X (12 lancers&nbsp;: 12 strikes) = 10 carreaux \* 30 points = 300
> - 9- 9- 9- 9- 9- 9- 9- 9- 9- 9- (20 lancers&nbsp;: 10 fois 9 quilles tombées puis 0) = 10 carreaux \* 9 points = 90
> - 5/ 5/ 5/ 5/ 5/ 5/ 5/ 5/ 5/ 5/5 (21 lancers&nbsp;: 10 fois 5 quilles tombées suivies d'un spare, avec un lancer final à 5) = 10 carreaux \* 15 points = 150

Ces cas de test utilisent la notation standard du bowling&nbsp;: `X` pour un strike, `/` pour un spare, `-` pour zéro.

Je les traduis en cas de test et j'éprouve la version actuelle de mon code dessus&nbsp;:

```ts
describe("when calculating the score of a bowling line", () => {
  // [...]

  test("edge case #1", () => {
    expect(scoreFor("10 10 10 10 10 10 10 10 10 10 10 10")).toBe(300); // ❌ Mon programme renvoie 330
  });

  test("edge case #2", () => {
    expect(scoreFor("9.0 9.0 9.0 9.0 9.0 9.0 9.0 9.0 9.0 9.0")).toBe(90); // ✅
  });

  test("edge case #3", () => {
    expect(scoreFor("5.5 5.5 5.5 5.5 5.5 5.5 5.5 5.5 5.5 5.5 5")).toBe(150); // ❌ Mon programme renvoie 155
  });
});
```

Pour le deuxième, en fait, c'est tout à fait normal que le test passe&nbsp;: ce n'est pas du tout un cas limite. C'est un cas tout à fait nominal. Je vais donc le supprimer.

Mais les deux autres me montrent bien qu'il me reste du travail...&nbsp;😅 À ce moment-là, je n'ai aucune idée de ce qui ne va pas. Ni de s'il existe un ordre logique pour traiter ces deux cas. Donc, arbitrairement, je reprends le premier.

Première étape, pour formuler un énoncé de test correct (parce qu'on est d'accord pour dire que `edge case #1`, c'est nul)&nbsp;: comprendre le problème&nbsp;!

Après une petite analyse, mon problème est double&nbsp;:

- L'avant-dernier strike génère un bonus pour le dernier lancer, ce qui fait donc +10 points par rapport à l'attendu
- Mon programme ne devrait compter qu'une seule fois les quilles tombées pour les 2 derniers lancers. Actuellement, il les compte en double&nbsp;: une fois comme si c'était n'importe quel lancer, et une seconde fois via le bonus apporté par le dixième "strike". L'énoncé n'est pas très clair là-dessus... Mais ça expliquerait les +20 points supplémentaires par rapport à l'attendu.

Et je pense que le problème est exactement le même pour le cas #3. Voici donc mon nouveau test&nbsp;:

```ts
describe("when calculating the score of a bowling line", () => {
  // [...]

  it("counts only once the pins knocked down on additional rolls", () => {
    expect(scoreFor("10 10 10 10 10 10 10 10 10 10 10 10")).toBe(300); // ❌
    expect(scoreFor("5.5 5.5 5.5 5.5 5.5 5.5 5.5 5.5 5.5 5.5 5")).toBe(150); // ❌
  });
});
```

## Les "frames" disparaissent

Je dois donc commencer par identifier si un lancer fait partie des lancers additionnels. À ce moment-là, je me dis «&nbsp;_J'ai déjà une classe qui me permet d'identifier des **lancers "spéciaux"**_&nbsp;». Et là, ça me frappe&nbsp;: «&nbsp;_Mais pourquoi elle s'appelle `Frames`&nbsp;?!_&nbsp;».

Je ne sais pas...&nbsp;😐

Initialement, cette classe avait pour but de répondre au besoin d'identification des "spare" et "strike". Dans ma tête, j'avais associé ces notions à la notion de carreau. D'où le nommage. Sauf que, lorsqu'on y réfléchit un peu, y a-t-il vraiment un lien&nbsp;? Dans le déroulement du jeu, oui. Mais dans le décompte des points finaux, pas tellement... D'ailleurs, nulle part je n'ai fait émergé la notion de carreau dans le code, ni regroupé des lancers en groupes de lancers d'un même carreau (je l'ai fait à un moment, pendant un refacto, pour m'apercevoir finalement que cela complexifiait inutilement l'algorithme).

Il y aurait plus de sens à appeler cette classe `Rolls`.

Je désactive donc, pour le moment, mon test cassant nouvellement écrit. Retour au vert&nbsp;✅. Et c'est parti pour un petit _refactoring_, qui m'emmènera finalement un peu plus loin que je ne l'aurais pensé&nbsp;♻️...

_10 minutes later..._

Voici ce qui a changé&nbsp;

**Rolls.ts**

```ts
class Rolls {
  private rolls: number[]; // Apparition de ce stockage interne des lancers bruts
  private sparesIndexes: number[] = []; // Renommage pour clarification
  private strikesIndexes: number[] = []; // Renommage pour clarification

  constructor(...rolls: number[]) {
    this.rolls = rolls; // J'enregistre les lancers tels qu'ils sont passés au constructeur
    let rollIndex = 0;
    while (rollIndex < rolls.length) {
      if (rolls[rollIndex] === 10) {
        this.strikesIndexes.push(rollIndex);
        rollIndex++;
        continue;
      }

      if (rolls[rollIndex] + rolls[rollIndex + 1] === 10) {
        this.sparesIndexes.push(rollIndex + 1);
      }
      rollIndex += 2;
    }
  }

  // La classe peut restituer les lancers bruts, au besoin, à l'identique...
  asArray(): number[] {
    return this.rolls;
  }

  isSpare(rollIndex: number): boolean {
    return this.sparesIndexes.includes(rollIndex);
  }

  isStrike(rollIndex: number): boolean {
    return this.strikesIndexes.includes(rollIndex);
  }
}
```

**Rolls.spec.ts**

```ts
// Exclusivement des modifications de termes employés dans les intentions de test
describe("when retrieving informations about special rolls", () => {
  const rolls = new Rolls(3, 4, 6, 4, 10, 7, 3);

  it("identifies a standard roll", () => {
    expect(rolls.isSpare(1)).toBe(false);
  });

  it("doesn't identify a spare on the first roll of a frame", () => {
    expect(rolls.isSpare(2)).toBe(false);
  });

  it("identifies a spare roll", () => {
    expect(rolls.isSpare(3)).toBe(true);
    expect(rolls.isSpare(6)).toBe(true);
  });

  it("identifies a strike roll", () => {
    expect(rolls.isStrike(4)).toBe(true);
  });
});
```

**BowlingScore.ts**

```ts
class BowlingScore {
  private rolls: Rolls;

  // Composition : la classe Rolls, anciennement Frames, n'est plus instanciée ici
  constructor(rolls: Rolls) {
    this.rolls = rolls;
  }

  calculate(): number {
    let lineScore = 0;
    // Je dois récupérer les lancers bruts ici pour les besoins de mon algo actuel...
    this.rolls.asArray().forEach((roll: number, index: number) => {
      lineScore += roll;

      if (index > 0 && (this.rolls.isSpare(index - 1) || this.rolls.isStrike(index - 1))) {
        lineScore += roll;
      }
      if (index > 1 && this.rolls.isStrike(index - 2)) {
        lineScore += roll;
      }
    });
    return lineScore;
  }
}
```

Et enfin mes tests d'acceptation&nbsp;:

```ts
describe("when calculating the score of a bowling line", () => {
  it("adds up the number of pins knocked down", () => {
    expect(scoreFor("3.4")).toBe(7);
  });

  it("adds up the score of all the line's frames", () => {
    expect(scoreFor("3.4 1.6 0.0 5.0 4.1 7.2 3.3 8.0 1.2 5.3")).toBe(58);
  });

  it("adds the next roll in case of a spare", () => {
    expect(scoreFor("7.3 4.2")).toBe(20);
  });

  it("adds the two following rolls in case of a strike", () => {
    expect(scoreFor("10 2.3")).toBe(20);
    expect(scoreFor("10 10 2.3")).toBe(42);
  });

  // Temporairement désactivé pour faire mon refacto en mode "tests passants"
  it.skip("counts only once the pins knocked down on additional rolls", () => {
    expect(scoreFor("10 10 10 10 10 10 10 10 10 10 10 10")).toBe(300);
    expect(scoreFor("5.5 5.5 5.5 5.5 5.5 5.5 5.5 5.5 5.5 5.5 5")).toBe(150);
  });
});

const scoreFor = (scoreAsString: string): number => {
  const rollsScores = rawStringToRolls(scoreAsString);
  return new BowlingScore(new Rolls(...rollsScores)).calculate(); // Seule cette ligne a réellement changé !
  // Merci la fonction scoreFor !
};

const rawStringToRolls = (scoreAsString: string): number[] => {
  return scoreAsString
    .split(" ")
    .flatMap((frameScore) => frameScore.split("."))
    .map((rollScoreAsString) => parseInt(rollScoreAsString));
};
```

Tous mes tests passent, refacto terminé. J'enchaîne.

## Retour à nos lancers de fin de partie

Je mets à nouveau à contribution ma classe permettant d'identifier les spécificités de certains lancers : `Rolls`.

```ts
describe("when retrieving informations about special rolls", () => {
  const rolls = new Rolls(
    3, // #0
    4, // #1, F1 over
    6, // #2
    4, // #3 spare, F2 over
    10, // #4 strike, F3 over
    7, // #5
    3, // #6 spare, F4 over
    0, // #7
    0, // #8, F5 over
    0, // #9
    0, // #10, F6 over
    0, // #11
    0, // #12, F7 over
    0, // #13
    0, // #14, F8 over
    0, // #15
    0, // #16, F9 over
    10, // #17 strike, F10 over
    1, // #18 additional
    2 // #19 additional
  );

  // [...]

  it("identifies an additional roll", () => {
    expect(rolls.isAdditional(7)).toBe(false);
    expect(rolls.isAdditional(17)).toBe(false);
    expect(rolls.isAdditional(18)).toBe(true);
    expect(rolls.isAdditional(19)).toBe(true);
  });
});
```

J'ai dû compléter le jeu de lancers pour avoir des lancers additionnels. J'ai favorisé la lisibilité à l'économie de lignes de code. Je ne suis pas pleinement satisfait du résultat, mais c'est ce que j'ai pu faire de mieux pour le moment (avec le formatage automatique qui, même s'il m'aide beaucoup d'habitude, m'a un peu mis des bâtons dans les roues ici...).

Et l'implémentation&nbsp;:

```ts
class Rolls {
  private rolls: number[];
  private firstAdditionalIndex: number;
  private sparesIndexes: number[] = [];
  private strikesIndexes: number[] = [];

  constructor(...rolls: number[]) {
    this.rolls = rolls;
    let rollIndex = 0;
    for (let frameCount = 0; frameCount < 10; frameCount++) {
      if (rolls[rollIndex] === 10) {
        this.strikesIndexes.push(rollIndex);
        rollIndex++;
        continue;
      }

      if (rolls[rollIndex] + rolls[rollIndex + 1] === 10) {
        this.sparesIndexes.push(rollIndex + 1);
      }
      rollIndex += 2;
    }
    this.firstAdditionalIndex = rollIndex;
  }

  // [asArray]

  isAdditional(rollIndex: number): any {
    return rollIndex >= this.firstAdditionalIndex;
  }

  // [isSpare, isStrike]
}
```

Bon, ça me chagrine un peu de voir réapparaître le notion de carreau via la variable `frameCount` juste après le refacto précédent&nbsp;😅... Mais il faut reconnaître que, pour identifier les lancers additionnels, je n'ai pas franchement d'autre choix que de compter les carreaux.

## Oups...

Pendant ma phase de _refacto_, je jette un dernier regard sur ma classe de test. Et je me rends compte que j'ai oublié de vérifier certains cas&nbsp;:

- Si le premier lancer additionnel vaut 10, est-il considéré comme un "strike" par mon code&nbsp;? Réponse&nbsp;: non&nbsp;😮‍💨
- Si les deux lancers additionnels cumulés valent 10, le dernier est-il considéré comme un "spare" par mon code&nbsp;? Réponse&nbsp;: non&nbsp;😮‍💨
- Dans le cas d'un carreau "0 puis 10", mon code le considère-t-il bien comme un "spare" et pas comme un "strike"&nbsp;? Réponse&nbsp;: non&nbsp;😫

Je dois donc reprendre mes tests pour ajouter le cas `0.10`.

```ts
describe("when retrieving informations about special rolls", () => {
  const rolls = new Rolls(
    // [...]

    0, // #7
    10, // #8 spare, F5 over

    // [...]

    8, // #18 additional
    2 // #19 additional
  );

  // [...]

  it("identifies a spare roll", () => {
    expect(rolls.isSpare(3)).toBe(true);
    expect(rolls.isSpare(6)).toBe(true);
    expect(rolls.isSpare(8)).toBe(true); // ✅
    expect(rolls.isSpare(19)).toBe(false); // ✅
  });

  it("identifies a strike roll", () => {
    expect(rolls.isStrike(4)).toBe(true);
    expect(rolls.isStrike(8)).toBe(false); // ❌
  });

  // [...]
});
```

```ts
class Rolls {
  // [...]

  constructor(...rolls: number[]) {
    this.rolls = rolls;
    let rollIndex = 0;
    for (let frameCount = 0; frameCount < 10; frameCount++) {
      if (rolls[rollIndex] < 10 && rolls[rollIndex] + rolls[rollIndex + 1] === 10) {
        this.sparesIndexes.push(rollIndex + 1);
        rollIndex += 2;
      } else if (rolls[rollIndex] === 10) {
        this.strikesIndexes.push(rollIndex);
        rollIndex++;
      } else {
        rollIndex += 2;
      }
    }
    this.firstAdditionalIndex = rollIndex;
  }

  // [...]
}
```

J'en ai profité pour retirer le `continue` qui me froissait. Une structure `if/else if/else` est plus accessible.

Malgré tout, je me dis que mon constructeur commence à être difficile à comprendre. Mais, même en y réfléchissant 5 minutes, je ne vois pas de façon évidente de le simplifier pour le moment. Je décide donc de le laisser en l'état pour le moment.

## The End

Pour faire passer au vert mon dernier cas de test, dans mon test d'acceptation, je tente naïvement d'ajouter un `if` dans mon code `BowlingScore`...

```ts
class BowlingScore {
  private rolls: Rolls;

  constructor(rolls: Rolls) {
    this.rolls = rolls;
  }

  calculate(): number {
    let lineScore = 0;
    this.rolls.asArray().forEach((knockedDownPins: number, index: number) => {
      // ICI !
      if (!this.rolls.isAdditional(index)) {
        lineScore += knockedDownPins;
      }

      if (index > 0 && (this.rolls.isSpare(index - 1) || this.rolls.isStrike(index - 1))) {
        lineScore += knockedDownPins;
      }
      if (index > 1 && this.rolls.isStrike(index - 2)) {
        lineScore += knockedDownPins;
      }
    });
    return lineScore;
  }
}
```

Et tous mes tests passent&nbsp;😎.

...

Sauf que... («&nbsp;_Aaaah, non, il continue&nbsp;!!_&nbsp;😨&nbsp;»), lorsque je regarde mon code, je n'en suis pas satisfait...

Déjà, cette condition négative que je viens d'ajouter ne me plaît pas. Je préfère toujours les conditions positives (donc sans le `!` devant, au cas où "condition positive/négative" ne soit pas clair).

Et puis, en relisant l'énoncé, j'ai quand même l'impression d'avoir pris le problème à l'envers. Dans l'énoncé, la notion de bonus s'applique systématiquement aux «&nbsp;**_prochains_**&nbsp;» lancers. Ce n'est pas ce que traduit mon code. Avec mes `index - 1` et `index - 2`, je traduis un fonctionnement rétroactif... Je ne sais pas d'où c'est sorti d'ailleurs... Même en reprenant l'historique écrit ci-dessus, je n'arrive pas à identifier la raison qui m'a incité à procéder de la sorte.&nbsp;🤔

Bref, maintenant que ma classe `Rolls` est bien musclée, testée, solide, je pourrais reprendre l'algo pour retranscrire exactement l'énoncé.

Je tente donc un ultime _refactoring_ en ce sens. Mais je suis couvert par mes tests. Donc je fais un commit, j'essaie, et si je ne suis pas convaincu du résultat, je pourrai toujours revenir en arrière&nbsp;! (Merci Git, l'ami du _refactorer_...)

```ts
class BowlingScore {
  private rolls: Rolls;

  constructor(rolls: Rolls) {
    this.rolls = rolls;
  }

  calculate(): number {
    let lineScore = 0;
    for (let rollIndex = 0; rollIndex < this.rolls.length; rollIndex++) {
      if (this.rolls.isAdditional(rollIndex)) {
        return lineScore;
      }

      lineScore += this.rolls.get(rollIndex) + this.bonusPoints(rollIndex);
    }
    return lineScore;
  }

  private bonusPoints(rollIndex: number): number {
    if (this.rolls.isSpare(rollIndex)) {
      return this.rolls.get(rollIndex + 1);
    }
    if (this.rolls.isStrike(rollIndex)) {
      return this.rolls.get(rollIndex + 1) + this.rolls.get(rollIndex + 2);
    }
    return 0;
  }
}
```

Bon, il se trouve que je préfère la version après&nbsp;😄. Et vous&nbsp;?

Tests&nbsp;✅. Aurais-je terminé&nbsp;?&nbsp;😯

«&nbsp;_Oui, pitié, on n'en peut plus de lire là !!_&nbsp;😣&nbsp;»

Ok.

# Conclusion

Quelles conclusions tirer de ce kata&nbsp;?

Pour commencer, je l'aime bien, ce kata&nbsp;😊. Il est suffisamment complexe pour qu'il y ait matière à réflexion, tant sur l'implémentation technique que sur le découpage fonctionnel.

Peut-être à réserver à des profils un minimum expérimentés. Car il pourrait vite devenir chronophage pour des profils juniors.

On pourra noter **l'intérêt d'appréhender la totalité d'un problème complexe** (ou de spécifications fonctionnelles) avant de se jeter sur le code&nbsp;! Cela permet de découper les choses, de traiter unitairement des problèmes plus petits, dans un ordre logique (pas forcément celui dans lequel la spec a été écrite).

Je vous laisse là-dessus. À la prochaine&nbsp;!&nbsp;😉

# Annexes

## Code complet

Je vous remets ici la version finale du code et des tests.

```ts
class BowlingScore {
  private rolls: Rolls;

  constructor(rolls: Rolls) {
    this.rolls = rolls;
  }

  calculate(): number {
    let lineScore = 0;
    for (let rollIndex = 0; rollIndex < this.rolls.length; rollIndex++) {
      if (this.rolls.isAdditional(rollIndex)) {
        return lineScore;
      }

      lineScore += this.rolls.get(rollIndex) + this.bonusPoints(rollIndex);
    }
    return lineScore;
  }

  private bonusPoints(rollIndex: number): number {
    if (this.rolls.isSpare(rollIndex)) {
      return this.rolls.get(rollIndex + 1);
    }
    if (this.rolls.isStrike(rollIndex)) {
      return this.rolls.get(rollIndex + 1) + this.rolls.get(rollIndex + 2);
    }
    return 0;
  }
}
```

```ts
describe("when calculating the score of a bowling line", () => {
  it("adds up the number of pins knocked down", () => {
    expect(scoreFor("3.4")).toBe(7);
  });

  it("adds up the score of all the line's frames", () => {
    expect(scoreFor("3.4 1.6 0.0 5.0 4.1 7.2 3.3 8.0 1.2 5.3")).toBe(58);
  });

  it("adds the next roll in case of a spare", () => {
    expect(scoreFor("7.3 4.2")).toBe(20);
  });

  it("adds the two following rolls in case of a strike", () => {
    expect(scoreFor("10 2.3")).toBe(20);
    expect(scoreFor("10 10 2.3")).toBe(42);
  });

  it("counts only once the pins knocked down on additional rolls", () => {
    expect(scoreFor("10 10 10 10 10 10 10 10 10 10 10 10")).toBe(300);
    expect(scoreFor("5.5 5.5 5.5 5.5 5.5 5.5 5.5 5.5 5.5 5.5 5")).toBe(150);
  });
});

const scoreFor = (scoreAsString: string): number => {
  const rollsScores = rawStringToRolls(scoreAsString);
  return new BowlingScore(new Rolls(...rollsScores)).calculate();
};

const rawStringToRolls = (scoreAsString: string): number[] => {
  return scoreAsString
    .split(" ")
    .flatMap((frameScore) => frameScore.split("."))
    .map((rollScoreAsString) => parseInt(rollScoreAsString));
};
```

```ts
class Rolls {
  private rolls: number[];
  private firstAdditionalIndex: number;
  private sparesIndexes: number[] = [];
  private strikesIndexes: number[] = [];

  constructor(...rolls: number[]) {
    this.rolls = rolls;
    let rollIndex = 0;
    for (let frameCount = 0; frameCount < 10; frameCount++) {
      if (rolls[rollIndex] < 10 && rolls[rollIndex] + rolls[rollIndex + 1] === 10) {
        this.sparesIndexes.push(rollIndex + 1);
        rollIndex += 2;
      } else if (rolls[rollIndex] === 10) {
        this.strikesIndexes.push(rollIndex);
        rollIndex++;
      } else {
        rollIndex += 2;
      }
    }
    this.firstAdditionalIndex = rollIndex;
  }

  get length(): number {
    return this.rolls.length;
  }

  get(rollIndex: number): number {
    return this.rolls[rollIndex];
  }

  isAdditional(rollIndex: number): any {
    return rollIndex >= this.firstAdditionalIndex;
  }

  isSpare(rollIndex: number): boolean {
    return this.sparesIndexes.includes(rollIndex);
  }

  isStrike(rollIndex: number): boolean {
    return this.strikesIndexes.includes(rollIndex);
  }
}
```

```ts
describe("when retrieving informations about special rolls", () => {
  const rolls = new Rolls(
    3, // #0
    4, // #1, F1 over
    6, // #2
    4, // #3 spare, F2 over
    10, // #4 strike, F3 over
    7, // #5
    3, // #6 spare, F4 over
    0, // #7
    10, // #8 spare, F5 over
    0, // #9
    0, // #10, F6 over
    0, // #11
    0, // #12, F7 over
    0, // #13
    0, // #14, F8 over
    0, // #15
    0, // #16, F9 over
    10, // #17 strike, F10 over
    8, // #18 additional
    2 // #19 additional
  );

  it("identifies a standard roll", () => {
    expect(rolls.isSpare(1)).toBe(false);
  });

  it("doesn't identify a spare on the first roll of a frame", () => {
    expect(rolls.isSpare(2)).toBe(false);
  });

  it("identifies a spare roll", () => {
    expect(rolls.isSpare(3)).toBe(true);
    expect(rolls.isSpare(6)).toBe(true);
    expect(rolls.isSpare(8)).toBe(true);
    expect(rolls.isSpare(19)).toBe(false);
  });

  it("identifies a strike roll", () => {
    expect(rolls.isStrike(4)).toBe(true);
    expect(rolls.isStrike(8)).toBe(false);
  });

  it("identifies an additional roll", () => {
    expect(rolls.isAdditional(7)).toBe(false);
    expect(rolls.isAdditional(17)).toBe(false);
    expect(rolls.isAdditional(18)).toBe(true);
    expect(rolls.isAdditional(19)).toBe(true);
  });
});
```

## L'Ours VS l'IA

Dans le kata précédent, j'avais fourni le même énoncé de départ à ChatGPT pour analyser ce qu'il produisait.

Pour cette fois-ci, j'estime que l'article est assez long comme ça. Mais je ferai l'exercice dans un post séparé. _Stay tuned_&nbsp;😉.
