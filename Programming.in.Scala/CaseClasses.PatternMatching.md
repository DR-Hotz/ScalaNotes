# Case Class and Pattern Matching

+   Case classes are Scala's way to allow pattern matching on objects without requiring a large amount of boilerplate, Generally, all you need to do is add a
single case keyword to each class that you want to be pattern matchable

+   Using **case** modifier
    *   adds a factory method with the name of the class
    *   all arguments in the parameter list of a case class implicitly get a **val** prefix
    *   the compiler adds "natural" implementations of methods toString, hashCode, and equals to your class
    *   the compiler adds a copy method to your class for making modified copies

+   Pattern Matching : selector match { alternatives }
    *   A match expression is evaluated by trying each of the patterns in the order they are written
    *   constant pattern matches when == holds true
    *   variable pattern like e matches every value
    *   The wildcard pattern (_) also matches every value, but it does
not introduce a variable name to refer to that value

```scala
sealed abstract class Expr

case class Var(name: String) extends Expr
case class Number(num: Double) extends Expr
case class UniOp(operator: String, arg: Expr) extends Expr
case class BinOp(operator: String, left: Expr, right: Expr) extends Expr

object Expr{

  //@unchecked let compiler shut up if all cases are not covered in method. 
  def simplifyTop(expr: Expr): Expr = (expr: @unchecked) match {
    case UniOp("-", UniOp("-", e)) => simplifyTop(e) //double negation
    case BinOp("+", e, Number(0)) => simplifyTop(e)  //adding zero
    case BinOp("*", e, Number(1)) => simplifyTop(e)  //multiplying by 1

    //binding e to UniOp("abs", _)
    case UniOp("abs", e @ UniOp("abs", _)) => simplifyTop(e)
    case UniOp("abs", Number(e: Double)) => if(e >= 0) Number(e) else Number(-e)
    case BinOp("+", x , y) if x == y => BinOp("*", x, Number(2))
    case _ => expr
  }
}

val v = Var("age")
val op1 = BinOp("+", Number(1), v)
val op2 = op1.copy(operator = "*")

def dne(ex: Expr) = UniOp("-", UniOp("-",ex))
def add0(ex: Expr) = BinOp("+", ex, Number(0))
def mul1(ex: Expr) = BinOp("*", ex, Number(1))

//reduce to Var("age")
Expr.simplifyTop(dne(add0(mul1(v))))

```

+   sealed class : cannot have any new subclasses added except the ones in the same file
    *   only need to worry about the subclasses you already know about
    *   the compiler will flag missing combinations of patterns with a warning message

+   Option

```scala
val capitals = Map("France" -> "Paris", "Japan" -> "Tokyo")

def show(x: Option[String]) = x match{
    case Some(x) => x
    case None => "?"
}

show(capitals get "France")
show(capitals get "Tokyo")

```


+   case squence as partial functions
    *   is a function literal

```scala
val withDefault: Option[Int] => Int = {
    case Some(x) = > x
    case None => 0
}

val second: PartialFunction[List[Int],Int] = {
    case x :: y :: _ => y
}

second.isDefinedAt(Nil)
```