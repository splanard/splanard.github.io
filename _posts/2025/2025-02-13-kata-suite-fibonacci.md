---
layout: post
title: "Kata : la suite de Fibonacci"
date: 2025-02-13 22:00:00 +0100
tags: article dev kata
---

Je ne vous refais pas un laïus sur ce que sont les katas de code et leur intérêt&nbsp;: vous trouverez ces informations très facilement via une recherche Internet.

Lorsque je cherche à découvrir des approches de katas différentes, d'autres personnes, je suis souvent frustré de trouver des contenus sous 2 formes&nbsp;:

- une **vidéo**&nbsp;: je n'aime pas les vidéos... Le rythme y est toujours soit trop lent, soit trop rapide. On ne peut pas réfléchir et analyser à son rythme. Généralement, j'ai toujours tendance à les écouter en vitesse 1,5 ou à sauter entièrement certains passages. Et je suis obligé de faire _pause_ à certains moment pour visualiser le code, qui est rarement très lisible au format vidéo. Bref, j'aime paaaaaaaaaas les vidéos&nbsp;😑 (pour ce type de contenu&nbsp;: je regarde des films comme tout le monde...).

- le **code final**, sur un dépôt GitHub par exemple&nbsp;: on perd alors toute la démarche&nbsp;! Or, dans ce genre d'exercice, c'est la démarche qui est, à mes yeux, la plus importante.

Donc, ici, je posterai de temps en temps des déroulements de kata... **entièrement à l'écrit&nbsp;!** Vous pourrez ainsi entrer, pour un temps, dans ma tête, mais à votre rythme&nbsp;😉.

_Et pour ceux qui n'aimeraient pas lire... allez regarder des vidéos&nbsp;!!_&nbsp;😅

# La suite de Fibonacci

Pour celles et ceux qui ne connaîtraient pas cette suite, qui est tout de même un grand classique, [Wikipédia](https://fr.wikipedia.org/wiki/Suite_de_Fibonacci) vous dira tout ce que vous avez besoin/envie de savoir.

Mais, pour résumer, c'est **une suite de nombre entiers dans laquelle chaque nombre est la somme des deux nombres qui le précèdent**.

Le but de ce kata est de construire une méthode qui renvoie un terme donné de la suite en lui fournissant l'indice souhaité. Voici les premiers termes :

|                                       Indice | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   |
| -------------------------------------------: | --- | --- | --- | --- | --- | --- | --- | --- |
| Terme de la suite de Fibonacci correspondant | 0   | 1   | 1   | 2   | 3   | 5   | 8   | 13  |

Dans ce post, lorsque je mentionnerai `F(n)` ce sera pour parler du terme d'indice `n` de la suite de Fibonacci. Par exemple, la valeur de `F(6)` est `8`.

# Stack technique

Pour ce kata, j'utiliserai le langage **Java** (ça ne sera pas toujours le cas&nbsp;: j'aime bien varier les langages), **JUnit** comme framework de test et **AssertJ** pour les assertions.

# C'est parti !

Par habitude, je réalise le kata en TDD. Je crée donc ma classe de test, et j'écris mon premier cas de test&nbsp;:

```java
public class FibonacciSequenceTest {

  private FibonacciSequence fibonacciSequence = new FibonacciSequence();

  @Test
  public void returnZero(){
    assertThat(fibonacciSequence.getNumberWithIndex(0)).isEqualTo(0);
  }

}
```

Pour que le code compile, je crée ma classe `FibonacciSequence`, avec une méthode `getNumberWithIndex`. Et, pour respecter le TDD à la lettre, je fais tout d'abord renvoyer `-1` à ma méthode.

Je lance mon test, KO. J'ai mon test cassant, je peux passer à l'implémentation&nbsp;:

```java
public class FibonacciSequence {

  public int getNumberWithIndex(int index) {
    return 0;
  }

}
```

Test OK.

Le test suivant vient de façon assez naturelle&nbsp;:

```java
@Test
public void returnOne(){
  assertThat(fibonacciSequence.getNumberWithIndex(1)).isEqualTo(1);
}
```

Je lance, test KO, j'implémente, mon test passe (_je ne vais pas vous refaire tout le cycle à chaque fois, vous avez compris l'idée..._).

```java
public class FibonacciSequence {

  public int getNumberWithIndex(int index) {
    if( index == 0 ){
      return 0;
    }
    return 1;
  }

}
```

Puis je réfléchis, et je me rends compte qu'il existe une implémentation plus simple. Je profite donc de la phase de _refactoring_ pour simplifier mon code&nbsp;:

```java
public class FibonacciSequence {

  public int getNumberWithIndex(int index) {
    return index;
  }

}
```

Je pourrais continuer sur la lancée et écrire le prochain test de la même façon...

```java
@Test
public void returnOneAgain(){
  assertThat(fibonacciSequence.getNumberWithIndex(2)).isEqualTo(1);
}
```

Mais je me rends compte de deux choses sur mes tests existants&nbsp;:

1. Le nommage des tests va vite me poser problème si je continue comme ça...&nbsp;😅
2. De toute façon, mon nommage actuel ne reflète pas un comportement&nbsp;!

Je décide donc de reprendre d'abord mes 2 premiers tests, pour les fusionner.

```java
@Test
public void arbitraryValuesForTheFirstTwoNumbers(){
  assertThat(fibonacciSequence.getNumberWithIndex(0)).isEqualTo(0);
  assertThat(fibonacciSequence.getNumberWithIndex(1)).isEqualTo(1);
}
```

J'en profite pour évacuer le terme «&nbsp;return&nbsp;» du nom du test, parce que je me rends compte que tous mes tests risquent de commencer de la même façon... autant éviter la répétition.

> _Attention, **le refactoring des tests est une opération délicate**. Elle est parfois nécessaire, pour simplifier les tests ou les rendre plus lisibles. Mais les tests représentent également mon filet de sécurité anti-régression. Je dois donc veiller à ne pas perdre de fonctionnalité en cours de route&nbsp;! Ne pas altérer la couverture fonctionnnelle de mes tests, ce qui reviendrait à "trouer" mon filet de sécurité._

J'en reviens maintenant à l'ajout du prochain test. Et je vais essayer de le formuler, lui aussi, de façon fonctionnelle. Pour le moment, la fonctionnalité implémentée est «&nbsp;renvoyer des nombres arbitaires&nbsp;». Maintenant, je veux ajouter la nouvelle fonctionnalité «&nbsp;renvoyer la somme des 2 termes précédents&nbsp;».

```java
@Test
public void sumOfTheTwoPreviousNumbers(){
  assertThat(fibonacciSequence.getNumberWithIndex(2)).isEqualTo(1);
  assertThat(fibonacciSequence.getNumberWithIndex(5)).isEqualTo(5);
  assertThat(fibonacciSequence.getNumberWithIndex(8)).isEqualTo(21);
}
```

J'ajoute volontairement plusieurs assertions, pour deux raisons. D'une part, **je veux que mon test me force à implémenter la logique de Fibonacci**. Je choisis des valeurs de façon à éviter les faux positifs&nbsp;: si j'avais pris uniquement `F(2)=1` et `F(3)=2`, l'implémentation `return index - 1;` aurait passé ces cas de test. Et `F(4)=3` ne m'aurait pas aidé à sortir de ce travers...&nbsp;😅

D'autre part, je sais que mes tests, une fois écrits, vont assurer la non-régression. Et je souhaite que cette non-régression soit robuste&nbsp;! Si, lors d'une opération de _refactoring_, j'introduis des erreurs par inadvertance, j'aimerais bien que mes tests me le signalent. Ayant identifié très rapidement le biais possible du `return index - 1`, j'évite de choisir des cas de test qui ne détecteraient pas cette régression.

Je prends quelques raccourcis par rapport au TDD "officiel"...

Une fois mes tests relancés, et mon second test KO, je passe à l'implémentation. Et voilà le résultat&nbsp;:

```java
public int getNumberWithIndex(int index) {
  if( index <= 1 ) {
    return index;
  }
  return this.getNumberWithIndex(index - 1) + this.getNumberWithIndex(index - 2);
}
```

Ma méthode se rappelle elle-même pour obtenir les 2 termes d'indices inférieurs. J'en arrive naturellement à utiliser la **récursivité**, c'est l'implémentation la plus simple.

Mes tests passent, ma solution est simple et lisible.&nbsp;✅

Bon, bah... j'ai fini, non&nbsp;?&nbsp;😁

Pas tout à fait...

# Analyse des performances

Je sais que **la récursivité est dangereuse pour les performances**. Je décide donc de faire un petit _benchmark_ de ma méthode (vous l'aurez deviné, lorsque je crée mon code de test des performances, dans le cadre de cet article, je sais déjà qu'il y a un problème&nbsp;😏. Mais vérifier les performances de méthodes de calcul, en particulier lorsque la récursivité est impliquée, ça peut être une bonne idée).

Grâce à une classe `Main` dont je vous épargne le code, je décide d'afficher les 51 premiers termes de la suite, de `F(0)` à `F(50)`, et d'enregistrer le temps de calcul pour chaque étape.

```
F(0)=0 (0ms)
F(1)=1 (0ms)
F(2)=1 (0ms)
F(3)=2 (0ms)
...
F(25)=75025 (0ms)
F(26)=121393 (0ms)
F(27)=196418 (0ms)
F(28)=317811 (4ms)
F(29)=514229 (2ms)
F(30)=832040 (4ms) 😎
F(31)=1346269 (5ms)
F(32)=2178309 (10ms)
F(33)=3524578 (17ms)
F(34)=5702887 (22ms)
F(35)=9227465 (49ms) 🤔
F(36)=14930352 (58ms)
F(37)=24157817 (112ms)
F(38)=39088169 (165ms)
F(39)=63245986 (266ms)
F(40)=102334155 (398ms)
F(41)=165580141 (616ms)
F(42)=267914296 (997ms) 😩
F(43)=433494437 (2,643s)
F(44)=701408733 (4,564s)
F(45)=1134903170 (6,839s)
F(46)=1836311903 (10,264s)
F(47)=2971215073 (17,421s)
F(48)=4807526976 (27,595s)
F(49)=7778742049 (43,492s)
F(50)=12586269025 (49,449s) 😱
```

Jusqu'à `F(30)`, on peut considérer que le calcul est instantané. Les quelques millisecondes enregistrées sont parfois juste du _bruit_ lié à l'activité de ma machine.

En revanche, pour les termes suivants, on constate que le temps de calcul augmente assez rapidement&nbsp;! À `F(42)` on avoisine la seconde. À `F(46)` on dépasse les 10 secondes. Et à `F(50)` **on s'approche de la minute de temps de calcul&nbsp;!**

Si je tentais `F(100)`, le ventilateur de mon PC s'emballerait. Et il est presque sûr que je doive terminer manuellement le process Java que je viens de lancer... (Je vous laisse essayer chez vous. Gardez l'extincteur à proximité quand même&nbsp;😅).

Pas cool.

D'ailleurs... constatez-vous quelque chose de rigolo (mais logique) concernant les temps de calculs&nbsp;?&nbsp;🙂

Ils suivent la même logique que la suite de Fibonacci&nbsp;! Le temps de calcul d'un terme correspond à peu près à la somme du temps de calcul des 2 précédents (mis à part pour le dernier terme où il semble y avoir eu un coup d'accélérateur...). Et, si on y réfléchit un peu, c'est parfaitement normal.

À cause de la récursivité, en appelant `F(n)`, j'appelle 1 fois `F(n-1)` et 1 fois `F(n-2)`. Mais `F(n-1)` va également appeler `F(n-2)` (qui sera donc exécuté 2 fois) et `F(n-3)` (qui a déjà été appelé 2 fois auparavant par les 2 appels à `F(n-2)`), etc. Cela crée une chaîne d'appel pyramidale, dans laquelle le nombre d'appels suit également la suite de Fibonacci :

| Appel    | Nombre de fois |
| -------- | -------------- |
| `F(n)`   | 1              |
| `F(n-1)` | 1              |
| `F(n-2)` | 2              |
| `F(n-3)` | 3              |
| `F(n-4)` | 5              |
| `F(n-5)` | 8              |
| `F(n-6)` | 13             |
| ...      | ...            |

Il sera donc très compliqué pour ce code de fournir des termes d'indice élevé 🥺, car **des temps de traitements de plusieurs minutes** (ou bien plus que cela, si l'indice est très élevé) **sont rarement envisageables**...

Comment faire pour corriger cela&nbsp;?&nbsp;🤔

# Mémoïsation / programmation dynamique

Pour améliorer le temps de traitement, on va chercher à éviter de re-calculer plusieurs fois chaque terme. On va donc stocker les valeurs calculées au fur et à mesure&nbsp;!

Cette façon de faire s'apparente à une _mise en cache_ et on peut la retrouver sous le nom de [**mémoïsation**](https://fr.wikipedia.org/wiki/M%C3%A9mo%C3%AFsation). C'est une technique très utilisée en [**programmation dynamique**](https://fr.wikipedia.org/wiki/Programmation_dynamique).

Cette fois, je n'ajoute pas de test. Mon code n'aura aucune nouvelle fonctionnalité, je vais juste essayer de faire en sorte qu'il fasse la même chose, mais plus vite. Mes tests remplissent désormais leur **fonction de non-régression**&nbsp;: mon filet de sécurité.

Je reprends donc le code de ma classe, tout en étant guidé par l'exécution des tests, qui m'indique si la couverture fonctionnelle de mon code a été dégradée ou non.

Et j'en arrive à cette nouvelle implémentation&nbsp;:

```java
public class FibonacciSequence {

  private final ArrayList<Long> internalMemory = new ArrayList<>(Arrays.asList(0L, 1L));

  public long getNumberWithIndex(int index) {
    if( internalMemory.size() >= index + 1 ) {
      return internalMemory.get(index);
    }

    long result = this.getNumberWithIndex(index - 1) + this.getNumberWithIndex(index - 2);
    internalMemory.add(result);

    return result;
  }

}
```

J'ai choisi de stocker les valeurs intermédiaires dans une propriété de la classe. De cette façon, des appels successifs à la méthode d'une même instance bénéficieront d'un gain de temps additionnel, car certaines valeurs sont déjà calculées et stockées en mémoire.

Si on souhaite économier la mémoire, et la libérer dès que le calcul est terminé (moyennement le passage du _garbage collector_... mais on n'est pas là pour parler du fonctionnement de la JVM), on peut tout à fait choisir de stocker les valeurs intermédiaires dans une variable locale, à l'intérieur de la méthode.&nbsp;😉

Pour m'assurer que cette nouvelle implémentation résoud mon problème de performance, je relance mon script de diagnostic. Le résultat est sans appel&nbsp;:

```
F(0)=0 (0ms)
F(1)=1 (0ms)
...
F(25)=75025 (0ms)
F(26)=121393 (0ms)
F(27)=196418 (0ms)
F(28)=317811 (0ms)
F(29)=514229 (0ms)
F(30)=832040 (0ms)
F(31)=1346269 (0ms)
F(32)=2178309 (0ms)
F(33)=3524578 (0ms)
F(34)=5702887 (0ms)
F(35)=9227465 (0ms)
F(36)=14930352 (0ms)
F(37)=24157817 (0ms)
F(38)=39088169 (0ms)
F(39)=63245986 (0ms)
F(40)=102334155 (0ms)
F(41)=165580141 (0ms)
F(42)=267914296 (0ms)
F(43)=433494437 (0ms)
F(44)=701408733 (0ms)
F(45)=1134903170 (0ms)
F(46)=1836311903 (0ms)
F(47)=2971215073 (0ms)
F(48)=4807526976 (0ms)
F(49)=7778742049 (0ms)
F(50)=12586269025 (0ms)
```

L'exécution est maintenant quasi-instantané, même à l'indice 50&nbsp;!

Par curiosité, j'ai augmenté l'indice maximum, pour voir à quel moment le calcul commence à prendre un temps non négligeable. **Même à l'indice 10000, le temps d'exécution est encore de 3-4 millisecondes**. (Et, très rapidement, pour avoir des résultats cohérents, j'ai été obligé de modifier mon implémentation pour utiliser `BigInteger` au lieu de `long`...).

Par la suite, compte-tenu de la technique de mémoïsation utilisée, je me rends compte que **l'utilisation de la récursivité n'est même plus nécessaire**&nbsp;.

```java
public class FibonacciSequence {

  private final ArrayList<BigInteger> internalMemory = new ArrayList<>(
    Arrays.asList(BigInteger.ZERO, BigInteger.ONE)
  );

  public BigInteger getNumberWithIndex(int index) {
    this.fillInternalMemoryWithAllNumbersUpTo(index);
    return internalMemory.get(index);
  }

  private void fillInternalMemoryWithAllNumbersUpTo(int index) {
    for( int i = internalMemory.size(); i <= index; i++ ) {
      BigInteger newValue = internalMemory.get(i-1).add(internalMemory.get(i-2));
      internalMemory.add(newValue);
    }
  }

}
```

# Problème de mémoire&nbsp;!

Par acquis de conscience, je tente des valeurs d'indice très haut.

Le calcul de `F(20000)` prend environ 25ms, `F(50000)` environ 80ms, `F(100000)` entre 250ms et 300ms.

Pour la gloire, je tente `F(1000000)`...

```
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
```

Oups&nbsp;!&nbsp;😅

C'est le problème de la technique de mémoïsation&nbsp;: elle consomme de la mémoire. Ici, j'ai stocké tellement de nombres dans la mémoire interne de ma classe que le process Java n'a plus assez de mémoire disponible pour tout stocker.

Je pourrais ajouter de la mémoire au lancement du process, mais ça ne serait que repousser le problème.

Je reprends donc mon implémentation, toujours couvert par mes tests, pour trouver une solution viable même pour des indices très élevés.

```java
public class FibonacciSequence {

  public BigInteger getNumberWithIndex(int index) {
    if( index <= 1 ){
      return BigInteger.valueOf(index);
    }

    BigInteger[] previousValues = new BigInteger[] { BigInteger.ZERO, BigInteger.ONE };
    for(int i = 2; i <= index; i++) {
      BigInteger newValue = previousValues[0].add(previousValues[1]);
      previousValues[0] = previousValues[1];
      previousValues[1] = newValue;
    }

    return previousValues[1];
  }

}
```

Pour régler le problème de mémoire, je ne stocke que les deux valeurs précédentes, que j'écrase au fur et à mesure.

Est-ce que ça fonctionne&nbsp;?

```
(0ms) F(50)=12586269025
(18ms) F(20000)=2531162323...
(47ms) F(50000)=1077773489...
(190ms) F(100000)=2597406934...
(14,975s) F(1000000)=1953282128...
```

Il semblerait que oui&nbsp;😋

_Au passage,_ `F(1000000)` _est un nombre qui possède presque 209000 chiffres..._

On observe même un gain de performance sur les indices testés précédemment. Je ne sais pas l'expliquer avec certitude. C'est peut-être lié à la nature et la taille des objets Java manipulés : un tableau de taille 2 est peut-être plus rapide à manipuler qu'une liste de centaines de milliers de valeurs... Ça paraît plausible en tout cas.

Bon, cette fois, je crois que je vais m'arrêter là&nbsp;!&nbsp;✅

Il est possible qu'avec des indices encore plus élevés je rencontre de nouveaux problèmes. Mais ça sera pour une prochaine fois&nbsp;: ma machine fatigue&nbsp;🥵 (et moi aussi).

# Conclusion

Comme tout kata, l'exercice peut être abordé de différentes façons. Et tout est _criticable_ (au sens où on peut en débattre).

## Nommage

Initialement, j'ai choisi de créer une classe qui _représente_ la suite de Fibonacci, et qui peut me fournir à la demande un terme d'indice N. J'aurais d'ailleurs pu nommer la méthode `getNumber(N)`, `getTerm(N)` ou `getTermOfIndex(N)`... On peut discuter sans fin du nommage des méthodes, pour les rendre les plus explicites possible pour le plus grand nombre.

D'ailleurs, je ne l'ai pas fait dans le cadre de ce kata, mais dans certains cas complexes j'aime bien **sonder des personnes au hasard**, pas forcément des développeurs, **pour vérifier si elles comprennent ce que fait une méthode** en leur donnant simplement sa signature (nom, type de retour, paramètres).

## Stateless ?

J'aurais également pu (et j'ai hésité à le faire à un moment donné, mais... j'ai eu la flemme 😅) changer complètement le paradigme d'utilisation de ma classe et basculer dans un mode _stateless_ : `new FibonacciNumber(N).getValue()`. Un collègue me l'a d'ailleurs suggéré à la lecture des sections précédentes de cet article. J'aurais également pu décider de partir là-dessus dès la première itération.

Ce pourrait être une stratégie différente, à tester lors d'une prochaine itération de ce kata.&nbsp;🙂 Mais il y a de fortes chances que l'algo final ressemble beaucoup...

## L'aspect incrémental

Ce que j'apprécie en particulier dans ce kata, c'est le fait de découvrir des problématiques différentes, itération après itération.

Les premières itérations sont clairement centrées autour du TDD, du nommage des éléments, de faire apparaître des notions métier et la logique de la suite de Fibonacci. Comment parvenir à une solution fonctionnelle et élégante, en étant guidé par les tests&nbsp;?

Mais dans une deuxième phase, on laisse un peu de côté le TDD et on se frotte à des problèmes plus "terre à terre"&nbsp;: le problème de performances et les soucis de mémoire. On constate alors que **les tests, qui ont servi de guide lors de la première phase, changent de fonction pour devenir les garants de la non-régression**.

# Annexes

## Tests

Pour la forme, je vous remets la classe de test complète.

```java
public class FibonacciSequenceTest {

  private final FibonacciSequence fibonacciSequence = new FibonacciSequence();

  @Test
  public void arbitraryValuesForTheFirstTwoNumbers(){
    assertThat(fibonacciSequence.getNumberWithIndex(0)).isEqualTo(0);
    assertThat(fibonacciSequence.getNumberWithIndex(1)).isEqualTo(1);
  }

  @Test
  public void sumOfTheTwoPreviousNumbers(){
    assertThat(fibonacciSequence.getNumberWithIndex(2)).isEqualTo(1);
    assertThat(fibonacciSequence.getNumberWithIndex(5)).isEqualTo(5);
    assertThat(fibonacciSequence.getNumberWithIndex(8)).isEqualTo(21);
  }

  @Test
  public void handleVeryBigOutputNumbers() {
    assertThat(fibonacciSequence.getNumberWithIndex(94)).isEqualTo(new BigInteger("19740274219868223167"));
  }

}
```

## L'Ours _versus_ l'IA

À force de voir tout le monde parler d'IA, je me dis qu'il faudrait peut-être que je m'y mette. Pour le moment, les utilisations de ChatGPT que j'ai pu observer ne m'ont pas pleinement convaincu. Mais c'est dans l'ère du temps, donc il est de mon devoir en tant que développeur, de mettre mon nez dedans (même si, pour le moment, je n'aime pas l'odeur)&nbsp;😓.

J'ai donc testé le prompt suivant sur un modèle mini GPT-4o (gratuit, trouvé en ligne)&nbsp;:

> Je souhaite que tu me donnes l'implémentation, avec le langage Java, d'une classe FibonacciSequence, possédant une méthode nommée getNumberWithIndex, qui renvoie le terme de la suite de Fibonacci d'indice N lorsqu'on lui passe un entier N en paramètre.

Le résultat ci-dessous&nbsp;:

```java
public class FibonacciSequence {

    // Méthode pour obtenir le terme de la suite de Fibonacci à l'indice N
    public int getNumberWithIndex(int N) {
        if (N < 0) {
            throw new IllegalArgumentException("L'indice ne peut pas être négatif.");
        }
        if (N == 0) {
            return 0;
        }
        if (N == 1) {
            return 1;
        }

        int a = 0; // F(0)
        int b = 1; // F(1)
        int fib = 0;

        for (int i = 2; i <= N; i++) {
            fib = a + b; // F(n) = F(n-1) + F(n-2)
            a = b;      // Met à jour F(n-2)
            b = fib;    // Met à jour F(n-1)
        }

        return fib; // Retourne F(N)
    }

    public static void main(String[] args) {
        FibonacciSequence fibonacci = new FibonacciSequence();
        int index = 10; // Exemple d'indice
        System.out.println("Le terme de la suite de Fibonacci à l'indice " + index + " est : " + fibonacci.getNumberWithIndex(index));
    }
}
```

Plusieurs observations&nbsp;:

- 👍 Le problème de performance est géré, rien à redire là-dessus. L'algorithme fourni est, à peu de choses près, le même que celui de mon implémentation finale (et je vous assure que je n'avais rien demandé au chatbot avant de faire mon implémentation&nbsp;: lors de l'écriture de cet article, c'est la première fois que je demande quoi que ce soit à ChatGPT&nbsp;😅)

- 👍 Le cas à la marge du nombre négatif passé en paramètre est géré. Ce n'était pas demandé, mais on peut se dire «&nbsp;pourquoi pas&nbsp;»... Donc j'accorde ce point.

- 👎 La méthode `main` n'était pas demandée. Et elle n'a rien à faire là.

- 👎 Le nommage des variables est nul. Pour `N`, c'est de la faute de mon prompt, d'accord. Mais, pour le reste, c'est nul. J'aurais largement préféré des variables bien nommées, plutôt que des commentaires.

- 👎 La problématique de la taille des nombres renvoyés à partir d'un certain indice n'est pas gérée. Je ne sais pas précisément à partir de quel indice ça pose problème, mais très rapidement, le type `int` ne sera plus adapté. Et le programme va planter.

Bon, je débute avec cet outil. J'imagine qu'avec un prompt plus précis, des contraintes de qualité de code, etc. il doit être possible d'obtenir un meilleur résultat.

Mais ce qui me chagrine, c'est que **si on utilise une IA générative en première approche, on n'a aucune idée de pourquoi on en arrive à cette implémentation**, des problématiques rencontrées en cours de route. Donc on n'apprend pas à s'en méfier, à les identifier plus tard dans un autre contexte.

Je fais le bilan&nbsp;: l'IA a pensé à l'indice négatif, mais pas à la taille des nombres en sortie. L'algo reste bon. Pas de tests unitaires, mais je ne les ai pas demandés (j'y penserai la prochaine fois). Je concède une égalité pour cette fois, mais je me trouve clément.

> OURS 1 - 1 IA
