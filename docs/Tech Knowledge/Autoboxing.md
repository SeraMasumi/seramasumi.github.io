---
layout: default
title: Autoboxing
nav_order: 3
---



##Autoboxing

Autoboxing: Primitive types to object wrapper classes. Java compiler does this automatically.

```java
public class AutoboxingTest {
    public static void main(String [] args){
        // autoboxing
        Integer integer1 = new Integer(-1);
        Integer integer2 = new Integer(-1);
        System.out.println(integer1 * integer2); // 1
        System.out.println(Math.abs(integer1)); // 1
        
        // unboxing
        int int1 = integer1;
        int int2 = integer2;
        System.out.println(int1 * int2); // 1
        
        System.out.println(integer1 == integer2); // false, not the same reference
        System.out.println(int1 == int2); //  true
        System.out.println(integer1.compareTo(integer2)); // 0
        // System.out.println(int1.compareTo(int2)); // error: int cannot be dereferenced
        
        Integer integer3 = null;
        System.out.println(integer3); // null
        // int int3 = integer3; // java.lang.NullPointerException
    }
}
```



