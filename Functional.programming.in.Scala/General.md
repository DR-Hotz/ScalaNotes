# General Notes

+   Procedure : parameterized chunk of code that may have side effects
+   Pure functions : a function has no observable effect on the execution of the program other than to compute a result given its inputs

+   referential transparency (RT)
    *   An expression e is referentially transparent if, for all programs p, all occurrences of e in p can be replaced by the result of evaluating e without affecting the meaning of p
    *   forces the invariant that everything a function does is represented by the value that it returns

```scala
//Not referential transparent!!
def buyCoffee(cc: CreditCard): Coffee ={
    val cup = new Coffee()
    cc.charge(cup.price)
    cup
}
```