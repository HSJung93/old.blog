---
title: "Java Inheritance vs Composition"
date: 2022-07-26T09:08:00+09:00
categories:
  - Memo
tags:
  - Inheritance 
  - Composition
  - design pattern
# header:
#   teaser: /assets/images/code.jpg
---

Inheritance and composition are two programming techniques developers use to establish relationships between classes and objects. Whereas inheritance derives one class from another, composition defines a class as the sum of its parts.

Classes and objects created through inheritance are tightly coupled because changing the parent or superclass in an inheritance relationship risks breaking your code. Classes and objects created through composition are loosely coupled, meaning that you can more easily change the component parts without breaking your code.

Because loosely coupled code offers more flexibility, many developers have learned that composition is a better technique than inheritance, but the truth is more complex. 

## When to use inheritance in Java

In object-oriented programming, we can use inheritance when we know there is an "is a" relationship between a child and its parent class. 

```java
class Vehicle {

    String brand;
    String color;
    double weight;
    double speed;

    void move() {
        System.out.println("The vehicle is moving");
    }
}

public class Car extends Vehicle {
    String licensePlateNumber;
    String owner;
    String bodyStyle;

    public static void main(String... inheritanceExample) {
        System.out.println(new Vehicle().brand);
        System.out.println(new Car().brand);
        new Car().move();
    }
}
```

When you are considering using inheritance, ask yourself whether the subclass really is a more specialized version of the superclass. In this case, a car is a type of vehicle, so the inheritance relationship makes sense.

## When to use composition in Java

In object-oriented programming, we can use composition in cases where one object "has" (or is part of) another object.

```java
public class CompositionExample {

    public static void main(String... houseComposition) {
        new House(new Bedroom(), new LivingRoom());
        // The house now is composed with a Bedroom and a LivingRoom
    }

    static class House {

        Bedroom bedroom;
        LivingRoom livingRoom;

        House(Bedroom bedroom, LivingRoom livingRoom) {
            this.bedroom = bedroom;
            this.livingRoom = livingRoom;
        }
    }
    static class Bedroom { }
    static class LivingRoom { }
}
```

## Inheritance vs composition: Two examples

Consider the following code. Is this a good example of inheritance?

```java
import java.util.HashSet;

public class CharacterBadExampleInheritance extends HashSet<Object> {

    public static void main(String... badExampleOfInheritance) {
        BadExampleInheritance badExampleInheritance = new BadExampleInheritance();
        badExampleInheritance.add("Homer");
        badExampleInheritance.forEach(System.out::println);
    }
```

In this case, the answer is no. The child class inherits many methods that it will never use, resulting in tightly coupled code that is both confusing and difficult to maintain. If you look closely, it is also clear that this code does not pass the "is a" test.

Now let's try the same example using composition:

```java
import java.util.HashSet;
import java.util.Set;

public class CharacterCompositionExample {
    static Set<String> set = new HashSet<>();

    public static void main(String... goodExampleOfComposition) {
        set.add("Homer");
        set.forEach(System.out::println);
    }
```

Using composition for this scenario allows the  CharacterCompositionExample class to use just two of HashSet's methods, without inheriting all of them.

## Does Java have multiple inheritance?

Unlike some languages, such as C++, Java does not allow multiple inheritance with classes. You can use multiple inheritance with interfaces, however. The difference between a class and an interface, in this case, is that interfaces don't keep state.

## Using ‘super' to access parent classes methods

```java
public class SuperWordExample {
    class Character {
        Character() {
            System.out.println("A Character has been created");
        }
        void move() {
            System.out.println("Character walking...");
        }
    }
    class Moe extends Character {
        Moe() {
            super();
        }
        void giveBeer() {
            super.move();
            System.out.println("Give beer");
        }
    }
}
```

In this example, `Character` is the parent class for Moe.  Using `super`, we are able to access `Character`'s  `move()` method in order to give Moe a beer.

## Using constructors with inheritance

When one class inherits from another, the superclass's constructor always will be loaded first, before loading its subclass. In most cases, the reserved word `super` will be added automatically to the constructor.  However, if the superclass has a parameter in its constructor, we will have to deliberately invoke the `super` constructor, as shown below:

```java
public class ConstructorSuper {
    class Character {
        Character() {
            System.out.println("The super constructor was invoked");
        }
    }
    class Barney extends Character {
        // No need to declare the constructor or to invoke the super constructor
        // The JVM will to that
    }
}
```

If the parent class has a constructor with at least one parameter, then we must declare the constructor in the subclass and use super to explicitly invoke the parent constructor. The super reserved word won't be added automatically and the code won't compile without it.  For example:

```java
public class CustomizedConstructorSuper {
    class Character {
        Character(String name) {
            System.out.println(name + "was invoked");
        }
    }
    class Barney extends Character {
        // We will have compilation error if we don't invoke the constructor explicitly
        // We need to add it
        Barney() {
            super("Barney Gumble");
        }
    }
}
```