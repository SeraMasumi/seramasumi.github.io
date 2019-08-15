---
layout: default
title: Java Mock Framework — Mockito
nav_order: 2
---



# Mockito

## Introduction

Mockito is a mocking framework for unit testing in Java. Mocking is a way to test the functionality of a class in isolation, without database/server/file connection. A mock objects takes in dummy data and returns dummy input. A mock object is just aproxy for actual implementations.

Mockito uses Java reflection to create mock objects. Mockito is usually used with Junit integration.

## Usage

The class used in MathApplication.  We are going to mock this class to test MathApplication class:

```Java
public interface CalculatorService {
   public double add(double input1, double input2);
   public double subtract(double input1, double input2);
   public double multiply(double input1, double input2);
   public double divide(double input1, double input2);
}
```

The class to be tested:

```java
public class MathApplication {
   private CalculatorService calcService;

   public void setCalculatorService(CalculatorService calcService){
      this.calcService = calcService;
   }
   
   public double add(double input1, double input2){
      return calcService.add(input1, input2);
   }
   
   public double subtract(double input1, double input2){
      return calcService.subtract(input1, input2);
   }
   
   public double multiply(double input1, double input2){
      return calcService.multiply(input1, input2);
   }
   
   public double divide(double input1, double input2){
      return calcService.divide(input1, input2);
   }
}
```

The test class:

```java
import static org.mockito.Mockito.when;

import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.runners.MockitoJUnitRunner;

// @RunWith attaches a runner with the test class to initialize the test data
@RunWith(MockitoJUnitRunner.class)
public class MathApplicationTester {
	
   //@InjectMocks annotation is used to create and inject the mock object
   @InjectMocks 
   MathApplication mathApplication = new MathApplication();

   //@Mock annotation is used to create the mock object to be injected
   @Mock
   CalculatorService calcService;

   @Test
   public void testAdd(){
      //add the behavior of calc service to add two numbers
      when(calcService.add(10.0,20.0)).thenReturn(30.00);
		
      //test the add functionality
      Assert.assertEquals(mathApplication.add(10.0, 20.0),30.0,0);
   }
}
```

Class to execute the test class:

```java
import org.junit.runner.JUnitCore;
import org.junit.runner.Result;
import org.junit.runner.notification.Failure;

public class TestRunner {
   public static void main(String[] args) {
      Result result = JUnitCore.runClasses(MathApplicationTester.class);
      
      for (Failure failure : result.getFailures()) {
         System.out.println(failure.toString());
      }
      
      System.out.println(result.wasSuccessful());
   }
}  
```

### Adding behavior to mock

Use `when(...).then...` syntax.

```java
//add the behavior of calc service to add two numbers
when(calcService.add(10.0,20.0)).thenReturn(30.00);
```

Then the tester will be:

```java
import static org.mockito.Mockito.when;

import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.runners.MockitoJUnitRunner;

// @RunWith attaches a runner with the test class to initialize the test data
@RunWith(MockitoJUnitRunner.class)
public class MathApplicationTester {
	
   //@InjectMocks annotation is used to create and inject the mock object
   @InjectMocks 
   MathApplication mathApplication = new MathApplication();

   //@Mock annotation is used to create the mock object to be injected
   @Mock
   CalculatorService calcService;

   @Test
   public void testAdd(){
      //add the behavior of calc service to add two numbers
      when(calcService.add(10.0,20.0)).thenReturn(30.00);
		
      //test the add functionality
      Assert.assertEquals(mathApplication.add(10.0, 20.0),30.0,0);
   }
}
```

### Verifying behavior

To verify if a method is called with required arguments or not, use `verify(mock).method` syntax.

```java
//test the add functionality
Assert.assertEquals(calcService.add(10.0, 20.0),30.0,0);

//verify call to calcService is made or not with same arguments.
verify(calcService).add(10.0, 20.0);
```

The tester will be:

```java
import static org.mockito.Mockito.verify;
import static org.mockito.Mockito.when;

import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.runners.MockitoJUnitRunner;

// @RunWith attaches a runner with the test class to initialize the test data
@RunWith(MockitoJUnitRunner.class)
public class MathApplicationTester {
	
   //@InjectMocks annotation is used to create and inject the mock object
   @InjectMocks 
   MathApplication mathApplication = new MathApplication();

   //@Mock annotation is used to create the mock object to be injected
   @Mock
   CalculatorService calcService;

   @Test
   public void testAdd(){
      //add the behavior of calc service to add two numbers
      when(calcService.add(10.0,20.0)).thenReturn(30.00);
		
      //test the add functionality
      Assert.assertEquals(calcService.add(10.0, 20.0),30.0,0);

      //verify the behavior
      verify(calcService).add(10.0, 20.0);
   }
}
```

### Check for number of calls made to a particular method

Pass time as a parameter to `verify()` method:

```java
//limit the method call to 1, no less and no more calls are allowed
verify(calcService, times(1)).add(10.0, 20.0);
```

The tester will be:

```java
import static org.mockito.Mockito.verify;
import static org.mockito.Mockito.when;
import static org.mockito.Mockito.times;
import static org.mockito.Mockito.never;

import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.runners.MockitoJUnitRunner;

// @RunWith attaches a runner with the test class to initialize the test data
@RunWith(MockitoJUnitRunner.class)
public class MathApplicationTester {
	
   //@InjectMocks annotation is used to create and inject the mock object
   @InjectMocks 
   MathApplication mathApplication = new MathApplication();

   //@Mock annotation is used to create the mock object to be injected
   @Mock
   CalculatorService calcService;

   @Test
   public void testAdd(){
      //add the behavior of calc service to add two numbers
      when(calcService.add(10.0,20.0)).thenReturn(30.00);
		
      //add the behavior of calc service to subtract two numbers
      when(calcService.subtract(20.0,10.0)).thenReturn(10.00);
      
      //test the add functionality
      Assert.assertEquals(mathApplication.add(10.0, 20.0),30.0,0);
      Assert.assertEquals(mathApplication.add(10.0, 20.0),30.0,0);
      Assert.assertEquals(mathApplication.add(10.0, 20.0),30.0,0);
      
      //test the subtract functionality
      Assert.assertEquals(mathApplication.subtract(20.0, 10.0),10.0,0.0);
      
      //default call count is 1 
      verify(calcService).subtract(20.0, 10.0);
      
      //check if add function is called three times
      verify(calcService, times(3)).add(10.0, 20.0);
      
      //verify that method was never called on a mock
      verify(calcService, never()).multiply(10.0,20.0);
   }
```

### Check for number of calls made to a particular method (check for limits)

```java
atLeast(int min)
atLeastOnce()
atMost(int max)
```

The tester wil be:

```java
import static org.mockito.Mockito.verify;
import static org.mockito.Mockito.when;
import static org.mockito.Mockito.atLeastOnce;
import static org.mockito.Mockito.atLeast;
import static org.mockito.Mockito.atMost;

import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.runners.MockitoJUnitRunner;

// @RunWith attaches a runner with the test class to initialize the test data
@RunWith(MockitoJUnitRunner.class)
public class MathApplicationTester {
	
   //@InjectMocks annotation is used to create and inject the mock object
   @InjectMocks 
   MathApplication mathApplication = new MathApplication();

   //@Mock annotation is used to create the mock object to be injected
   @Mock
   CalculatorService calcService;

   @Test
   public void testAdd(){
      //add the behavior of calc service to add two numbers
      when(calcService.add(10.0,20.0)).thenReturn(30.00);
		
      //add the behavior of calc service to subtract two numbers
      when(calcService.subtract(20.0,10.0)).thenReturn(10.00);
      
      //test the add functionality
      Assert.assertEquals(mathApplication.add(10.0, 20.0),30.0,0);
      Assert.assertEquals(mathApplication.add(10.0, 20.0),30.0,0);
      Assert.assertEquals(mathApplication.add(10.0, 20.0),30.0,0);
      
      //test the subtract functionality
      Assert.assertEquals(mathApplication.subtract(20.0, 10.0),10.0,0.0);
      
      //check a minimum 1 call count
      verify(calcService, atLeastOnce()).subtract(20.0, 10.0);
      
      //check if add function is called minimum 2 times
      verify(calcService, atLeast(2)).add(10.0, 20.0);
      
      //check if add function is called maximum 3 times
      verify(calcService, atMost(3)).add(10.0,20.0);     
   }
}
```

### Test exception handling

Let a mock throw expected exceptions, then verify the exception:

```java
//add the behavior to throw exception
doThrow(new Runtime Exception("divide operation not implemented"))
   .when(calcService).add(10.0,20.0);
```

The tester will be:

```java
import static org.mockito.Mockito.doThrow;

import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.runners.MockitoJUnitRunner;

// @RunWith attaches a runner with the test class to initialize the test data
@RunWith(MockitoRunner.class)
public class MathApplicationTester {
	
   // @TestSubject annotation is used to identify class 
      which is going to use the mock object
   @TestSubject
   MathApplication mathApplication = new MathApplication();

   //@Mock annotation is used to create the mock object to be injected
   @Mock
   CalculatorService calcService;

   @Test(expected = RuntimeException.class)
   public void testAdd(){
      //add the behavior to throw exception
      doThrow(new RuntimeException("Add operation not implemented"))
         .when(calcService).add(10.0,20.0);

      //test the add functionality
      Assert.assertEquals(mathApplication.add(10.0, 20.0),30.0,0); 
   }
}
```

### Ways to create and inject mock

1. 不使用注释，使用被测试类的setter传进去。

```java
package com.tutorialspoint.mock;

import static org.mockito.Mockito.mock;
import static org.mockito.Mockito.verify;
import static org.mockito.Mockito.when;

import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.runners.MockitoJUnitRunner;

// @RunWith attaches a runner with the test class to initialize the test data
@RunWith(MockitoJUnitRunner.class)
public class MathApplicationTester {
	
   private MathApplication mathApplication;
   private CalculatorService calcService;

   @Before
   public void setUp(){
      mathApplication = new MathApplication();
      calcService = mock(CalculatorService.class);
      mathApplication.setCalculatorService(calcService);
   }

   @Test
   public void testAddAndSubtract(){

      //add the behavior to add numbers
      when(calcService.add(20.0,10.0)).thenReturn(30.0);

      //subtract the behavior to subtract numbers
      when(calcService.subtract(20.0,10.0)).thenReturn(10.0);

      //test the subtract functionality
      Assert.assertEquals(mathApplication.subtract(20.0, 10.0),10.0,0);

      //test the add functionality
      Assert.assertEquals(mathApplication.add(20.0, 10.0),30.0,0);

      //verify call to calcService is made or not
      verify(calcService).add(20.0,10.0);
      verify(calcService).subtract(20.0,10.0);
   }
}
```

2. 使用mock注释（参见更上面的例子）

### Verify the order which methods are called

```java
//create an inOrder verifier for a single mock
InOrder inOrder = inOrder(calcService);

//following will make sure that add is first called then subtract is called.
inOrder.verify(calcService).add(20.0,10.0);
inOrder.verify(calcService).subtract(20.0,10.0);
```

The tester will be:

```java
import static org.mockito.Mockito.mock;
import static org.mockito.Mockito.verify;
import static org.mockito.Mockito.when;
import static org.mockito.Mockito.inOrder;

import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.InOrder;
import org.mockito.runners.MockitoJUnitRunner;

// @RunWith attaches a runner with the test class to initialize the test data
@RunWith(MockitoJUnitRunner.class)
public class MathApplicationTester {
	
   private MathApplication mathApplication;
   private CalculatorService calcService;

   @Before
   public void setUp(){
      mathApplication = new MathApplication();
      calcService = mock(CalculatorService.class);
      mathApplication.setCalculatorService(calcService);
   }

   @Test
   public void testAddAndSubtract(){

      //add the behavior to add numbers
      when(calcService.add(20.0,10.0)).thenReturn(30.0);

      //subtract the behavior to subtract numbers
      when(calcService.subtract(20.0,10.0)).thenReturn(10.0);

      //test the add functionality
      Assert.assertEquals(mathApplication.add(20.0, 10.0),30.0,0);

      //test the subtract functionality
      Assert.assertEquals(mathApplication.subtract(20.0, 10.0),10.0,0);

      //create an inOrder verifier for a single mock
      InOrder inOrder = inOrder(calcService);

      //following will make sure that add is first called then subtract is called.
      inOrder.verify(calcService).subtract(20.0,10.0);
      inOrder.verify(calcService).add(20.0,10.0);
   }
}
```

### Spying

Implement the class we need to mock, then the class to be tested will use the overrided methods.

Ex: create spy class (csSpy) on CalculatorService, then impliment methods in csSpy class. When MathApplication is calling CalculatorService, it's actually calling csSpy's method.

```java
//create a spy on actual object
calcService = spy(calculator);

//perform operation on real object
//test the add functionality
Assert.assertEquals(mathApplication.add(20.0, 10.0),30.0,0);
```

The tester will be:

```java
import static org.mockito.Mockito.spy;

import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.runners.MockitoJUnitRunner;

// @RunWith attaches a runner with the test class to initialize the test data
@RunWith(MockitoJUnitRunner.class)
public class MathApplicationTester {
	
   private MathApplication mathApplication;
   private CalculatorService calcService;

   @Before
   public void setUp(){
      mathApplication = new MathApplication();
      Calculator calculator = new Calculator();
      calcService = spy(calculator);
      mathApplication.setCalculatorService(calcService);	     
   }

   @Test
   public void testAdd(){

      //perform operation on real object
      //test the add functionality
      Assert.assertEquals(mathApplication.add(20.0, 10.0),30.0,0);
   }

   class Calculator implements CalculatorService {
      @Override
      public double add(double input1, double input2) {
         return input1 + input2;
      }

      @Override
      public double subtract(double input1, double input2) {
         throw new UnsupportedOperationException("Method not implemented yet!");
      }

      @Override
      public double multiply(double input1, double input2) {
         throw new UnsupportedOperationException("Method not implemented yet!");
      }

      @Override
      public double divide(double input1, double input2) {
         throw new UnsupportedOperationException("Method not implemented yet!");
      }
   }
}
```

### Reset mock

```java
//reset mock
reset(calcService);
```

After resetting the mock, all mocked methods will not be available anymore.

The tester will be:

```java
package com.tutorialspoint.mock;

import static org.mockito.Mockito.mock;
import static org.mockito.Mockito.when;
import static org.mockito.Mockito.reset;

import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.runners.MockitoJUnitRunner;

// @RunWith attaches a runner with the test class to initialize the test data
@RunWith(MockitoJUnitRunner.class)
public class MathApplicationTester {
	
   private MathApplication mathApplication;
   private CalculatorService calcService;

   @Before
   public void setUp(){
      mathApplication = new MathApplication();
      calcService = mock(CalculatorService.class);
      mathApplication.setCalculatorService(calcService);
   }

   @Test
   public void testAddAndSubtract(){

      //add the behavior to add numbers
      when(calcService.add(20.0,10.0)).thenReturn(30.0);
  
      //test the add functionality
      Assert.assertEquals(mathApplication.add(20.0, 10.0),30.0,0);

      //reset the mock	  
      reset(calcService);

      //test the add functionality after resetting the mock
      Assert.assertEquals(mathApplication.add(20.0, 10.0),30.0,0);   //???
   }
}
```

### Behavior driven development

Behavior Driven Development is a style of writing tests uses **given**, **when** and **then** format as test methods. Mockito provides special methods to do so. Take a look at the following code snippet.

```java
//Given
given(calcService.add(20.0,10.0)).willReturn(30.0);

//when
double result = calcService.add(20.0,10.0);

//then
Assert.assertEquals(result,30.0,0);	     
```

Here we're using **given** method of BDDMockito class instead of **when** method of .

The tester will be:

```java
package com.tutorialspoint.mock;

import static org.mockito.BDDMockito.*;

import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.runners.MockitoJUnitRunner;

// @RunWith attaches a runner with the test class to initialize the test data
@RunWith(MockitoJUnitRunner.class)
public class MathApplicationTester {
	
   private MathApplication mathApplication;
   private CalculatorService calcService;

   @Before
   public void setUp(){
      mathApplication = new MathApplication();
      calcService = mock(CalculatorService.class);
      mathApplication.setCalculatorService(calcService);
   }

   @Test
   public void testAdd(){

      //Given
      given(calcService.add(20.0,10.0)).willReturn(30.0);

      //when
      double result = calcService.add(20.0,10.0);

      //then
      Assert.assertEquals(result,30.0,0);   // true
   }
}
```

### Timeouts

To test if a method is called within a certain time frame.

```java
//passes when add() is called within 100 ms.
verify(calcService,timeout(100)).add(20.0,10.0);
```

The tester will be:

```java
package com.tutorialspoint.mock;

import static org.mockito.Mockito.mock;
import static org.mockito.Mockito.verify;
import static org.mockito.Mockito.when;

import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.runners.MockitoJUnitRunner;

// @RunWith attaches a runner with the test class to initialize the test data
@RunWith(MockitoJUnitRunner.class)
public class MathApplicationTester {
	
   private MathApplication mathApplication;
   private CalculatorService calcService;

   @Before
   public void setUp(){
      mathApplication = new MathApplication();
      calcService = mock(CalculatorService.class);
      mathApplication.setCalculatorService(calcService);
   }

   @Test
   public void testAddAndSubtract(){

      //add the behavior to add numbers
      when(calcService.add(20.0,10.0)).thenReturn(30.0);

      //subtract the behavior to subtract numbers
      when(calcService.subtract(20.0,10.0)).thenReturn(10.0);

      //test the subtract functionality
      Assert.assertEquals(mathApplication.subtract(20.0, 10.0),10.0,0);

      //test the add functionality
      Assert.assertEquals(mathApplication.add(20.0, 10.0),30.0,0);

      //verify call to add method to be completed within 100 ms
      verify(calcService, timeout(100)).add(20.0,10.0);
	  
      //invocation count can be added to ensure multiplication invocations
      //can be checked within given timeframe
      verify(calcService, timeout(100).times(1)).subtract(20.0,10.0); // true
   }
}
```