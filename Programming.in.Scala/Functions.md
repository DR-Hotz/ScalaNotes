# Functions && Closure

+   Method : a function as a member of some object

+   Function Literals : compiled into a class that when instantiated at runtime is a function value
    *   exist in the source code
+   Function value :
    *   exist as objects at runtime

+   Partially applied function
    *   a way to transform a def into a function value
    *   partially applied arguments
```scala
def sum(a: Int, b: Int, c: Int): Int = a + b + c

/**
 * An underscore is used to represent an entire parameter list
 * [s description] refers to a function value object
 * @type {[type]}
 */
val s = sum _

//short for s.apply(1,2,3), defined in the class generated
//automatically by the Scala compiler from the expression sum _
s(1,2,3)

//partially applied
val s1 = sum(1, _: Int, 3)

def funcApp(f: (Int, Int, Int) => Int)(x: Int, y: Int, z: Int) = f(x, y, z)
val fa1 = funcApp(_: (Int, Int, Int) => Int)(1,2,3)
fa1(sum)
fa1(sum _)
```

+   Placeholder : Only works if each parameter appears in the function literal exactly once, Multiple underscores mean multiple parameters

```scala
val lst = List(1,2,3,4,5)
lst.filter(_>3)

val f = (_: Int) + (_: Int)
```


+   Repeated parameters

```scala
//String* is actually Array[String]
def echo(args: String*) = for(arg <- args) println(arg)

val arr = Array("one", "two", "three")
echo(arr)  //Wrong!!!

//pass each element of arr as its own argument to echo, rather than all
of it as a single argument
echo(arr: _*)  //OK!!

```

+   Named arguments

+   Default parameter values

+   Closures :  the function value is the end product of the act of closing the open term
    *   captured parameter lives out on the heap, instead of the stack, and thus can outlive the method call that created it
    *   closed term : already "closed" as written, such as (x: Int) => x > 0
    *   open term :  requires that a binding for its free variable to be captured at runtime

```scala
var freeVar = 100

val closure = (x: Int) => x + freeVar

//200
closure(100)

freeVar = 200
//300
closure(100)

val nums = List(1,2,3)
var sum = 0
//sum would be 6
nums.foreach(sum += _)
```


+   tail recursion :  calling themselves as their last action are tail recursive
    *   A tail-recursive function will not build a new stack frame for each call; all calls will execute in a single frame
    *   limits(impossible to be optimized)
        -   indirect recursion
        -   the final call goes to a function value
```scala
//impossible to be optimized
val func = nestedFunc _
def nestedFunc(x: Int): Unit = {
    if(x != 0) {println(x); func(x - 1)}
}

```


+   By-name parameter
    *   No such thing as a by-name variable/field
    *   function value

```scala
def byNameAssert(predicate: => Boolean) = if(!predicate) throw new AssertionError

byNameAssert(5 > 3)
```
