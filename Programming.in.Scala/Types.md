# Types && Operations


## Literals

+   String literals
    *   raw string : three double quotes """

```scala
println("""Welcom to Ultamix 3000.
Type "Help" for help""")

println("""|Welcom to Ultamix 3000.
           |Type "Help" for help """.stripMargin)
```

+   Symbol literals : symbols are interned. If you write the same symbol literal twice, both expressions will refer to the exact same Symbol object

```scala
def updateRecord(s: Symbol, v: Any) = {
    println(s.name)
    //other codes
}

```


## String Interpolation

+   The s, f, and raw string interpolators are implemented via this
general mechanism.

```scala
val name = "Desmond"
val title = "Manager"
s"Hello, $title $name! you got ${6 * 7} mails"
raw"No\\\\\\\\\escape"
f"${math.Pi}%.5f"
```


## Operations

+   Precedence : Scala decides precedence based on the first character of the methods used in operator notation, **but** If an operator ends in an equals character (=), and the operator is not one of the comparison operators <=, >=, ==, or !=, then the precedence of the operator is the same as that of simple assignment (=);
    *   * % /
    *   + -
    *   :
    *   = !
    *   <>
    *   &
    *   ^
    *   |
    *   all letters
    *   all assignment operators
+   Associativity : The associativity of an operator in Scala is determined by its last character, any method that ends in a `:' character is invoked on its right operand

+   Any method can be an operator
    *   infix : 7 + 2
    *   prefix(unary) :  - 7
        -   identifiers: + - ! ~
        -   def unary_-(num: Int): Int
    *   postfix(unary) : 7 toLong

+   ==(value equality)

```scala
List(1,2,3) == null    //false
1 == null              //false


```

+   eq/ne(reference equality)