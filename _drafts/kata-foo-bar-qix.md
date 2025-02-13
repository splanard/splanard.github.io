---
layout: post
title: "Kata : FooBarQix"
tags: article dev kata
---

Je ne vous refais pas un laïus sur ce que sont les katas de code et leur intérêt&nbsp;: vous trouverez ces informations très facilement via une recherche Internet.

Lorsque je cherche à découvrir des approches de katas différentes, d'autres personnes, je suis souvent frustré de trouver des contenus sous 2 formes&nbsp;:

- une **vidéo**&nbsp;: je n'aime pas les vidéos... Le rythme y est toujours soit trop lent, soit trop rapide. On ne peut pas réfléchir et analyser à son rythme. Généralement, j'ai toujours tendance à les écouter en vitesse 1,5 ou à sauter entièrement certains passages. Et je suis obligé de faire _pause_ à certains moment pour visualiser le code, qui est rarement très lisible au format vidéo. Bref, j'aime paaaaaaaaaas les vidéos&nbsp;😑 (pour ce type de contenu&nbsp;: je regarde des films comme tout le monde...).

- le **code final**, sur un dépôt GitHub par exemple&nbsp;: on perd alors toute la démarche&nbsp;! Or, dans ce genre d'exercice, c'est la démarche qui est, à mes yeux, la plus importante.

Donc, ici, je posterai de temps en temps des déroulements de kata... **entièrement à l'écrit&nbsp;!** Vous pourrez ainsi entrer, pour un temps, dans ma tête, mais à votre rythme&nbsp;😉.

_Et pour ceux qui n'aimeraient pas lire... allez regarder des vidéos&nbsp;!!_&nbsp;😅

# FooBarQix

C'est un kata dont j'ai entendu _parler_ sur LinkedIn et publié sur [CodingDojo.org](https://codingdojo.org/kata/FooBarQix/).

L'objectif : implémenter une fonction/méthode `compute`, qui prend en paramètre un nombre entier et renvoie une chaîne de caractères suivant les règles suivantes.

- Si le nombre reçu est divisible par 3, ajouter `Foo` à la fin de la chaîne
- Si le nombre reçu est divisible par 5, ajouter `Bar` à la fin de la chaîne
- Si le nombre reçu est divisible par 7, ajouter `Qix` à la fin de la chaîne
- Pour chaque chiffre 3, 5 ou 7 dans le nombre reçu, ajouter `Foo`, `Bar` ou `Qix` à la fin de la chaîne, dans l'ordre des chiffres

Exemples&nbsp;:

```
1  => 1
2  => 2
3  => FooFoo (divisible par 3, contient 3)
4  => 4
5  => BarBar (divisible par 5, contient 5)
6  => Foo (divisible par 3)
7  => QixQix (divisible par 7, contient 7)
8  => 8
9  => Foo
10 => Bar
13 => Foo
15 => FooBarBar (divisible par 3, divisible par 5, contient 5)
21 => FooQix
33 => FooFooFoo (divisible par 3, contient deux 3)
51 => FooBar
53 => BarFoo
```

Puis, traiter cette règle additionnelle :

- Il faut garder une trace de chaque 0 (zéro) dans le nombre reçu. Chaque zéro doit donc être remplacé par une étoile `*`.

Exemples&nbsp;:

```
101   => 1*1
303   => FooFoo*Foo
105   => FooBarQix*Bar
10101 => FooQix**
```

À la lecture de l'énoncé, on réalise assez rapidement qu'il est incomplet. Typiquement, dans le cas de `83`, qui n'est divisible ni par 3, ni par 5, ni par 7, mais qui contient un 3. Faut-il écrire `8Foo` ou `8` ou `Foo`&nbsp;?

Il est possible que l'énoncé ne soit pas exhaustif pour se forcer à tester beaucoup et/ou prendre des hypothèses...

# C'est parti !

```java
public class FooBarQixTest {

  private final FooBarQix fooBarQix = new FooBarQix();

  @Test
  public void noTransformationApplied() {
    assertThat(fooBarQix.compute(1)).isEqualTo("1");
  }

}
```

```java
public class FooBarQix {

  public String compute(int number) {
    return Integer.toString(number);
  }

}
```

Next

```java
@Test
public void multipleOfThree() {
  assertThat(fooBarQix.compute(6)).isEqualTo("Foo");
}
```

```java
public class FooBarQix {

  public String compute(int number) {
    if( isNumberMultipleOf(number, 3) ){
      return "Foo";
    }
    return Integer.toString(number);
  }

  private boolean isNumberMultipleOf(int number, int divider) {
    return number % divider == 0;
  }

}
```

Refacto

```java
public class Number {

  public static Number isNumber(int number) {
    return new Number(number);
  }

  private final int number;

  private Number(int number) {
    this.number = number;
  }

  public boolean multipleOf(int divider) {
    return this.number % divider == 0;
  }

}
```

```java
public class FooBarQix {

  public String compute(int number) {
    if( isNumber(number).multipleOf(3) ){
      return "Foo";
    }
    return Integer.toString(number);
  }

}
```

Next (x2)

```java
@Test
public void multipleOfFive() {
  assertThat(fooBarQix.compute(20)).isEqualTo("Bar");
}

@Test
public void multipleOfSeven() {
  assertThat(fooBarQix.compute(28)).isEqualTo("Qix");
}
```

```java
public String compute(int number) {
  if( isNumber(number).multipleOf(3) ){
    return "Foo";
  }
  if( isNumber(number).multipleOf(5) ){
    return "Bar";
  }
  if( isNumber(number).multipleOf(7) ){
    return "Qix";
  }
  return Integer.toString(number);
}
```

Next

```java
@Test
public void combineTwoMultipleRules() {
  assertThat(fooBarQix.compute(21)).isEqualTo("FooQix");
}
```

```java
public String compute(int number) {
  StringBuilder result = new StringBuilder();

  if( isNumber(number).multipleOf(3) ){
    result.append("Foo");
  }
  if( isNumber(number).multipleOf(5) ){
    result.append("Bar");
  }
  if( isNumber(number).multipleOf(7) ){
    result.append("Qix");
  }

  return result.isEmpty() ? Integer.toString(number) : result.toString();
}
```

Next

```java
@Test
public void containsThree() {
  assertThat(fooBarQix.compute(3)).isEqualTo("FooFoo");
}
```

```java
public class Number {

  private final Integer number;

  public Number(int number) {
    this.number = number;
  }

  public boolean contains(int digit) {
    return this.number.toString().contains(Integer.toString(digit));
  }

  public boolean isMultipleOf(int divider) {
    return this.number % divider == 0;
  }

  public String toString() {
    return this.number.toString();
  }

}
```

```java
public class FooBarQix {

  public String compute(int number) {
    Number inputNumber = new Number(number);
    StringBuilder stringBuilder = this.applyMultipleRules(inputNumber);
    if( inputNumber.contains(3) ){
      stringBuilder.append("Foo");
    }
    return stringBuilder.toString();
  }

  private StringBuilder applyMultipleRules(Number number) {
    StringBuilder result = new StringBuilder();

    if( number.isMultipleOf(3) ){
      result.append("Foo");
    }
    if( number.isMultipleOf(5) ){
      result.append("Bar");
    }
    if( number.isMultipleOf(7) ){
      result.append("Qix");
    }

    if( result.isEmpty() ){
      result.append(number);
    }
    return result;
  }

}
```

Next

```java
@Test
public void replaceMultipleOfThreeByFoo() {
  assertThat(fooBarQix.compute(6)).isEqualTo("Foo");
}

@Test
public void replaceMultipleOfFiveByBar() {
  assertThat(fooBarQix.compute(20)).isEqualTo("Bar");
}

@Test
public void replaceMultipleOfSevenByQix() {
  assertThat(fooBarQix.compute(28)).isEqualTo("Qix");
}

@Test
public void combineTwoMultipleRules() {
  assertThat(fooBarQix.compute(21)).isEqualTo("FooQix");
}

@Test
public void addFooForEachThreeInTheInitialNumber() {
  assertThat(fooBarQix.compute(333)).isEqualTo("FooFooFooFoo");
}
```

```java
public class Number {

  private final Integer number;

  public Number(int number) {
    this.number = number;
  }

  public boolean isMultipleOf(int divider) {
    return this.number % divider == 0;
  }

  public char[] toDigits() {
    return this.number.toString().toCharArray();
  }

  public String toString() {
    return this.number.toString();
  }

}
```

```java
public class FooBarQix {

  public String compute(int number) {
    Number inputNumber = new Number(number);
    StringBuilder stringBuilder = new StringBuilder();

    this.applyMultipleRules(stringBuilder, inputNumber);
    this.applyDigitsRules(stringBuilder, inputNumber);

    return stringBuilder.toString();
  }

  private void applyDigitsRules(StringBuilder stringBuilder, Number initialNumber) {
    char[] digits = initialNumber.toDigits();
    for(char digit: digits){
      if( digit == '3' ){
        stringBuilder.append("Foo");
      }
    }
  }

  private void applyMultipleRules(StringBuilder stringBuilder, Number number) {
    if( number.isMultipleOf(3) ){
      stringBuilder.append("Foo");
    }
    if( number.isMultipleOf(5) ){
      stringBuilder.append("Bar");
    }
    if( number.isMultipleOf(7) ){
      stringBuilder.append("Qix");
    }

    if( stringBuilder.isEmpty() ){
      stringBuilder.append(number);
    }
  }

}
```

Next

```java
@Test
public void addBarForEachFiveInTheInitialNumber() {
  assertThat(fooBarQix.compute(555)).isEqualTo("FooBarBarBarBar");
}

@Test
public void addQixForEachSevenInTheInitialNumber() {
  assertThat(fooBarQix.compute(735)).isEqualTo("FooBarQixQixFooBar");
}
```

```java
private void applyDigitsRules(StringBuilder stringBuilder, Number initialNumber) {
  char[] digits = initialNumber.toDigits();
  for(char digit: digits){
    if( digit == '3' ){
      stringBuilder.append("Foo");
    }
    if( digit == '5' ){
      stringBuilder.append("Bar");
    }
    if( digit == '7' ){
      stringBuilder.append("Qix");
    }
  }
}
```

Next

```
...
11 => 11
12 => Foo
13 => 13Foo ❌
14 => Qix
...
```

```java
@Test
public void printTheInitialNumberOnlyIfThereAreNoTransformationsOfAnyKind() {
  assertThat(fooBarQix.compute(1357)).isEqualTo("FooBarQix");
}
```

```java
public class FooBarQix {

  public String compute(int number) {
    Number inputNumber = new Number(number);
    StringBuilder stringBuilder = new StringBuilder();

    this.applyMultipleRules(stringBuilder, inputNumber);
    this.applyDigitsRules(stringBuilder, inputNumber);

    return stringBuilder.isEmpty() ? inputNumber.toString() : stringBuilder.toString();
  }

  private void applyDigitsRules(StringBuilder stringBuilder, Number initialNumber) {
    char[] digits = initialNumber.toDigits();
    for(char digit: digits){
      if( digit == '3' ){
        stringBuilder.append("Foo");
      }
      if( digit == '5' ){
        stringBuilder.append("Bar");
      }
      if( digit == '7' ){
        stringBuilder.append("Qix");
      }
    }
  }

  private void applyMultipleRules(StringBuilder stringBuilder, Number number) {
    if( number.isMultipleOf(3) ){
      stringBuilder.append("Foo");
    }
    if( number.isMultipleOf(5) ){
      stringBuilder.append("Bar");
    }
    if( number.isMultipleOf(7) ){
      stringBuilder.append("Qix");
    }
  }

}
```

Refacto

```java
public enum Digit {
  THREE(3, "Foo"),
  FIVE(5, "Bar"),
  SEVEN(7, "Qix");

  private final int value;
  private final String word;

  Digit(int value, String word) {
    this.value = value;
    this.word = word;
  }

  public int getValue() {
    return value;
  }

  public String getWord() {
    return word;
  }

  public char toChar() {
    return Integer.toString(value).toCharArray()[0];
  }
}
```

```java
public class FooBarQix {

  public String compute(int number) {
    Number inputNumber = new Number(number);
    StringBuilder stringBuilder = new StringBuilder();

    this.applyMultipleRules(stringBuilder, inputNumber);
    this.applyDigitsRules(stringBuilder, inputNumber);

    return stringBuilder.isEmpty() ? inputNumber.toString() : stringBuilder.toString();
  }

  private void applyDigitsRules(StringBuilder stringBuilder, Number initialNumber) {
    for(char charDigit: initialNumber.toDigits()){
      for(Digit searchedDigit: Digit.values()){
        if(charDigit == searchedDigit.toChar()) {
          stringBuilder.append(searchedDigit.getWord());
        }
      }
    }
  }

  private void applyMultipleRules(StringBuilder stringBuilder, Number number) {
    for(Digit searchedDigit: Digit.values()){
      if(number.isMultipleOf(searchedDigit.getValue())) {
        stringBuilder.append(searchedDigit.getWord());
      }
    }
  }

}
```

Next

```java
@Test
public void addStarForEachZeroInTheInitialNumber() {
  assertThat(fooBarQix.compute(303)).isEqualTo("FooFoo*Foo");
  assertThat(fooBarQix.compute(105)).isEqualTo("FooBarQix*Bar");
  assertThat(fooBarQix.compute(10101)).isEqualTo("FooQix**");
}
```

```java
public enum Digit {
  ZERO(0, "*"),
  THREE(3, "Foo"),
  FIVE(5, "Bar"),
  SEVEN(7, "Qix");
  // ...
}
```

```java
private void applyMultipleRules(StringBuilder stringBuilder, Number number) {
  for(Digit searchedDigit: Digit.values()){
    if(searchedDigit.getValue() != 0 && number.isMultipleOf(searchedDigit.getValue())) {
      stringBuilder.append(searchedDigit.getWord());
    }
  }
}
```

```java
@Test
public void replaceMultipleOfFiveByBar() {
  assertThat(fooBarQix.compute(20)).isEqualTo("Bar*"); 😢
}
```

Next

```java
@Test
public void replaceZerosByStarsEvenInTheInitialNumber() {
  assertThat(fooBarQix.compute(101)).isEqualTo("1*1");
}
```

😱
