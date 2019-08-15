---
layout: default
title: Java Override/Overload, 4 Principles of OOP
nav_order: 2
---

# Java Override/Overload

###4 Principles of OOP

### Encapsulation

Encapsulation is the mechanism of hiding of data implementation in a class by restricting access to its public methods. Through encapsulation, the internal

### Abstraction

An abstraction denotes the essential characteristics of an object that distinguish it from all other kinds of objects and thus provide crisply defined conceptual boundaries, relative to the perspective of the viewer. Abstraction meas a concept which is not associated with any particular instance. Using abstract class/interface we express the intent of the class rather than the actual implementation. 

### Inheritance

Inheritances expresses "is a" relationship between two objects. Using inheritance, the subclass can reuse the code in its super class.

### Polymorphism

Polymorphism refers to the ability of different objects to respond to the same message in different ways. Static polymorphism is achieved using method overloading and dynamic polymorphism using method overriding.

Two types:

**Override**: dynamic binding, run-time

**Overload**: static binding, compile-time

```java
class Dog {
    public void bark() {
        System.out.println("dog bark");
    }
    
    // Overloading
    public void bark(int num) {
        for(int i = 0; i < num; i++) {
            System.out.println("dog bark");
        }
    }
}
```

**Binding**: association of method definition to the method call.

**Static binding**: The binding of static, private and final methods. These methods cannot be overridden, type of class is determined at compile time.

**Dynamic binding**: Determined at run time. Methods are not static, private, nor final.

```java
class Dog {
    public void bark(){
        System.out.println("dog bark");
    }
}
class Hound extends Dog {
    public void bark(){
        System.out.println("hound bark");
    }
}
public class OverridingTest {
    public static void main(String [] args){
        Dog dog = new Hound();
        dog.bark(); // hound bark
    }
}
=================================================
class Dog {
    public static void bark(){ // static method
        System.out.println("dog bark");
    }
}
class Hound extends Dog {
    public static void bark(){ // static method
        System.out.println("hound bark");
    }
}
public class OverridingTest {
    public static void main(String [] args){
        Dog dog = new Hound();
        dog.bark(); // dog bark
    }
}
```

