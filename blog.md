En Java, l'héritage est un concept clé de la programmation orientée objet (POO). Il permet à une classe, appelée sous-classe ou classe dérivée, d'hériter des attributs et des méthodes d'une autre classe, appelée superclasse ou classe de base. L'héritage en Java permet de créer une hiérarchie de classes, où les sous-classes peuvent étendre ou spécialiser le comportement des superclasses, tout en réutilisant leur code. Cela favorise la réutilisabilité, la modularité et la maintenabilité du code. En utilisant l'héritage, les développeurs peuvent organiser et structurer leur code de manière logique, en évitant la duplication et en favorisant la cohérence et la flexibilité.


### Scénario

Pour un Archer, voici les règles :

Un Archer peut tirer une flèche de précision.
Un Archer inflige 8 points de dégâts avec sa flèche de précision.
Un Archer inflige 4 points de dégâts avec une attaque normale.
Pour un Guerrier Mystique, voici les règles :

Un Guerrier Mystique peut canaliser l'énergie mystique.
Un Guerrier Mystique devient vulnérable lorsqu'il canalise l'énergie mystique.
Un Guerrier Mystique inflige 15 points de dégâts lorsqu'il canalise l'énergie mystique.
Un Guerrier Mystique inflige 5 points de dégâts lorsqu'il n'a pas canalisé l'énergie mystique.


Créez une nouvelle classe appelée Archer. Cette classe devrait hériter de la classe Player existante.

``` java
public class Player {
    boolean isVulnerable() {
        return true;
    }

    int getDamagePoints(Player player) {
        return 1;
    }
}

```

:red_circle: Test Red



### 1er test
La classe Archer hérite de la classe Player

```java
@Test
    @Tag("test:1")
    @DisplayName("The Archer class inherits from the Player class")
    void testArcherIsInstanceOfFighter() throws ClassNotFoundException {
        assertThat(Player.class).isAssignableFrom(Archer.class);
    }
```
```java
package org.example;

public class Archer extends Player{

}

```
:green_circle: Test Green

### 2eme Test
Mettez à jour la classe Archer afin que sa méthode toString() décrive de quel type de combattant il s'agit. La méthode doit renvoyer la chaîne « Archer is a Player ».
```java
@Test
    @Tag("test:2")
    @DisplayName("The Archer class overrides the toString method")
    void testArcherOverridesToStringMethod() {
        Archer archer = new Archer();
        assertEquals("Archer is a Player", archer.toString());
   }
```:red_circle: Test Red

```java
public class Archer extends Player{
    @Override
    public String toString() {
        return "Archer is a Player";
    }
}
```
:green_circle: Test Green
### 3eme Test
Mettez à jour la classe Archer pour que sa méthode isVulnerable() renvoie toujours false.

```java
@Test
    @Tag("test:3")
    @DisplayName("The Archer class overrides the isVulnerable method")
    void testArcherOverridesIsVulnerableMethod() {
        Archer archer = new Archer();
        assertFalse(archer.isVulnerable());}
```
:red_circle: Test Red

```java
public class Archer extends Player{
    @Override
    public String toString() {
        return "Archer is a Player ";
    }
    @Override
    public boolean isVulnerable() {
        return false;
    }
}
```:green_circle: Test Green
### 4eme Test
Mettez à jour la classe Archer afin que sa méthode getDamagePoints(Player) calcule les dégâts infligés par un Archer selon les règles ci-dessus.

```java
@Test
    @Tag("test:4")
    @DisplayName("A Archer deals 8 damage to a vulnerable target")
    void testArcherOverridesGetDamagePointsMethod() {
        Archer archer = new Archer();
        Player archerVulnerable = new Player();
        assertEquals(8, archer.getDamagePoints(archerVulnerable));

    }

```
:red_circle: Test Red

```java
public class Archer extends Player{
    @Override
    public String toString() {
        return "Archer is a Player";
    }
    @Override
    public boolean isVulnerable() {
        return false;
    }
    @Override
    public int getDamagePoints(Player player) {
        return 8;
    }
}

```
:green_circle: Test Green

```java
@Test
    @Tag("test:5")
    @DisplayName("A Archer deals 4 damage to an invulnerable target")
    void testArchersDamagePointsWhenTargetNotVulnerable() {
        Archer archer = new Archer();
        Archer archerInVulnerable = new Archer();
        assertEquals(4, archer.getDamagePoints(archerInVulnerable));

    }

```
:red_circle: Test Red

```java
public class Archer extends Player{
    @Override
    public String toString() {
        return "Archer is a Player";
    }
    @Override
    public boolean isVulnerable() {
        return false;
    }
    @Override
    public int getDamagePoints(Player player) {
        if (player.isVulnerable()) {
            return 8;
        }
        return 4;
    }
}

```
:green_circle: Test Green

### 5 éme Test
Créez une autre nouvelle classe appelée Mystic. Cette classe devrait également hériter de la classe Player existante.
Mettez à jour la classe Mystic pour ajouter une méthode appelée prepareSpell(). La classe doit se souvenir du moment où cette méthode est appelée et s'assurer que sa méthode isVulnerable() renvoie false uniquement lorsqu' l'énergie mystique est canalisé.

```java
public class Mystic extends Player{
    @Override
    public String toString() {
        return "Mystic is a Player";
    }
    @Override
    public boolean isVulnerable() {
        return false;
    }
    @Override
    public int getDamagePoints(Player player) {
        if (player.isVulnerable()) {
            return 5;
        }
        return 15;
    }
}

```

```java
@Test
    @Tag("test:6")
    @DisplayName("A Mystic is vulnerable when not prepared with a spell")
    void testMysticVulnerableByDefault() {
        Mystic mystic = new Mystic();
        assertThat(mystic.isVulnerable()).isTrue();
    }
@Test
    @Tag("test:7")
    @DisplayName("A Mystic is not vulnerable when prepared with a spell")
    void testWizardVulnerable() {
        Mystic mystic = new Mystic();
        mystic.prepareSpell();
        assertThat(mystic.isVulnerable()).isFalse();
    }
```
:red_circle: Test Red

```java
public class Mystic extends Player{
    private boolean vulnerable=true;

    @Override
    public String toString() {
        return "Archer is a Player ";
    }
    @Override
    public boolean isVulnerable() {
        return vulnerable;
    }
    @Override
    public int getDamagePoints(Player player) {
        if (player.isVulnerable()) {
            return 5;
        }
        return 15;
    }
    public void prepareSpell() {
    vulnerable = false;
    }
}
```
:green_circle: Test Green

### 6 éme Test
Mettez à jour la classe Mystic afin que sa méthode getDamagePoints(Player) calcule les dégâts infligés par un Mystic selon les règles ci-dessus.

```java
@Test
    @Tag("test:8")
    @DisplayName("A Mystic deals 5 damage when no spell has been prepared")
    void testWizardsDamagePoints() {
        Mystic mystic = new Mystic();
        assertThat(mystic.getDamagePoints(new Mystic())).isEqualTo(5);
    }
    @Test
    @Tag("task:9")
    @DisplayName("A Mystic deals 15 damage after a spell has been prepared")
    void testWizardsDamagePointsAfterPreparingSpell() {
        Mystic mystic = new Mystic();
        mystic.prepareSpell();
        assertThat(mystic.getDamagePoints(new Mystic())).isEqualTo(15);
    }

```
:red_circle: Test Red

```java
public class Mystic extends Player{
    private boolean vulnerable=true;

    @Override
    public String toString() {
        return "Archer is a Player ";
    }
    @Override
    public boolean isVulnerable() {
        return vulnerable;
    }
    @Override
    public int getDamagePoints(Player player) {
        if (isVulnerable()) {
            return 5;
        }
        return 15;
    }
    public void prepareSpell() {
    vulnerable = false;
    }
}

```
:green_circle: Test Green

### Critique
La critique de l'héritage en Java, bien que subjective, peut porter sur plusieurs aspects :

Rigidité de la hiérarchie de classes: L'utilisation intensive de l'héritage peut conduire à une hiérarchie de classes rigide et difficile à maintenir. Les modifications apportées à la classe de base peuvent avoir un impact sur toutes les classes dérivées, ce qui peut rendre le code fragile.

Couplage fort: L'héritage crée un couplage fort entre les classes de base et dérivées, ce qui peut rendre le code difficile à comprendre et à modifier. Les modifications dans la classe de base peuvent potentiellement affecter toutes les classes dérivées, ce qui rend le code moins flexible et plus difficile à maintenir.

Violation du principe de substitution de Liskov (LSP): L'héritage peut parfois violer le principe de substitution de Liskov, ce qui signifie que les sous-classes doivent pouvoir être substituées à leur classe de base sans altérer la cohérence du programme. Si les sous-classes ne respectent pas ce principe, cela peut conduire à des comportements inattendus et des erreurs difficiles à déboguer.

Complexité accrue: Une hiérarchie de classes complexe avec de multiples niveaux d'héritage peut rendre le code difficile à comprendre et à maintenir. Cela peut également conduire à des problèmes de performance, car l'appel de méthodes dans une chaîne d'héritage peut entraîner une surcharge de travail pour le système.

Alternative préférable: Dans de nombreux cas, la composition ou l'utilisation d'interfaces peut être une alternative préférable à l'héritage. La composition permet une meilleure encapsulation et réduit le couplage entre les classes, tandis que les interfaces permettent une meilleure flexibilité en permettant à une classe de mettre en œuvre plusieurs comportements.

En résumé, bien que l'héritage soit un concept fondamental de la programmation orientée objet, son utilisation excessive peut conduire à des problèmes de conception et de maintenance. Il est important de considérer les avantages et les inconvénients de l'héritage dans chaque cas et d'opter pour la meilleure approche de conception en fonction des besoins spécifiques du projet.



Inspiration=>[https://exercism.org/tracks/java/exercises/wizards-and-warriors/edit]

Bien à vous