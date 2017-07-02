# Classes && Objects

+   Scala has just two namespaces for definitions in place of Java's four
    *   Java
        -   fields, methods, types, and packages
    *   Scala
        -   values (fields, methods, packages, and singleton objects)
        -   types (class and trait names)
+   Procedure : A method that is executed only for its side effects is known as a procedure
+   fields and methods belong to the same namespace
    *   it is possible for a field to override a parameterless method
    *   it is forbidden to define a field and method with the **same name in the same class**

+   Parametric Field
```scala
//This is a shorthand that defines at the same time a parameter and field with the same name
class ArrayElement(val contents: Array[String]) extends Element{

    ...
}
```

## Singleton Object

+   singleton objects don't have any subclasses
+   Define a singleton object does **NOT** define a type
+   Each singleton object is **an instance** of its superclasses and mixed-in traits
+   A singleton object is initialized the first time some code accesses it
+   Any standalone object with a main method of the proper signature can be used as the entry point into an application
```scala
def main(args: Array[String]): Unit

```



## Implicit Conversions

```scala
implicit def intToRational(x: Int) = new Rational(x)

```


