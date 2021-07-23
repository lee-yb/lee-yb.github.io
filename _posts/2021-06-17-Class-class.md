# Class class

```java
Instances of the class Class represent classes and interfaces in a running Java application. An enum type is a kind of class and an annotation type is a kind of interface. Every array also belongs to a class that is reflected as a Class object that is shared by all arrays with the same element type and number of dimensions. The primitive Java types (boolean, byte, char, short, int, long, float, and double), and the keyword void are also represented as Class objects.
Class has no public constructor. Instead a Class object is constructed automatically by the Java Virtual Machine when a class loader invokes one of the defineClass methods and passes the bytes of a class file.
The methods of class Class expose many characteristics of a class or interface. Most characteristics are derived from the class file that the class loader passed to the Java Virtual Machine. A few characteristics are determined by the class loading environment at run time, such as the module returned by getModule().
Some methods of class Class expose whether the declaration of a class or interface in Java source code was enclosed within another declaration. Other methods describe how a class or interface is situated in a nest. A nest is a set of classes and interfaces, in the same run-time package, that allow mutual access to their private members. The classes and interfaces are known as nestmates. One nestmate acts as the nest host, and enumerates the other nestmates which belong to the nest; each of them in turn records it as the nest host. The classes and interfaces which belong to a nest, including its host, are determined when class files are generated, for example, a Java compiler will typically record a top-level class as the host of a nest where the other members are the classes and interfaces whose declarations are enclosed within the top-level class declaration.
The following example uses a Class object to print the class name of an object:
       void printClassName(Object obj) {
           System.out.println("The class of " + obj +
                              " is " + obj.getClass().getName());
       }
   
It is also possible to get the Class object for a named type (or for void) using a class literal. See Section 15.8.2 of The Java™ Language Specification. For example:
System.out.println("The name of class Foo is: "+Foo.class.getName());
Since:
1.0
See Also:
ClassLoader.defineClass(byte[], int, int)
Author:
unascribed
Type parameters:
<T> – the type of the class modeled by this Class object. For example, the type of String.class is Class<String>. Use Class<?> if the class being modeled is unknown.
```

