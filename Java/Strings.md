# Java Strings

Strings in Java are objects that represent sequences of characters.

## Creating Strings

```java
String text = "Hello, world!";
String emptyString = "";
String multilineString = "This is a\nmultiline\nstring";
```

## String Concatenation

```java
String firstName = "John";
String lastName = "Doe";
String fullName = firstName + " " + lastName;
System.out.println(fullName); // "John Doe"
```

### String Interpolation

Java does not support string interpolation directly like JavaScript, but you can use the `String.format` method or concatenation.

```java
String firstName = "John";
String lastName = "Doe";
String fullName = String.format("%s %s", firstName, lastName);
System.out.println(fullName); // "John Doe"
```

## String Methods

String methods are used to manipulate strings.

```java
String text = "Hello, world!";
System.out.println(text.length()); // 13
System.out.println(text.toUpperCase()); // "HELLO, WORLD!"
System.out.println(text.toLowerCase()); // "hello, world!"
System.out.println(text.indexOf("world")); // 7
System.out.println(text.substring(7, 12)); // "world"
System.out.println(text.replace("world", "Java")); // "Hello, Java!"
System.out.println(text.contains("world")); // true

```

### Comparing Strings

You can compare two strings to check if they are equal using the `.equals` method.

```java
String str1 = "Hello";
String str2 = "Hello";
System.out.println(str1.equals(str2)); // true
```

### Accessing a String Character

You can access a string character using the `charAt()` method.

```java
String text = "Hello, world!";
System.out.println(text.charAt(0)); // 'H'
```

## Converting Strings to Numbers

You can convert a string to a number using the `Integer.parseInt` and `Double.parseDouble` methods.

```java
String text = "123";
System.out.println(Integer.parseInt(text)); // 123
System.out.println(Double.parseDouble(text)); // 123.0
```
