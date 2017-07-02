# Packages and Imports

+   Package object : Each package is allowed to have one package object. Any definitions placed in a package object are considered members of the package itself

```scala
//in playground/package.scala




```

+   Scala Import
    *   may appear anywhere
    *   may refer to objects
    *   let you rename and hide some of the imported members

```scala
import Fruits.{Apple, Orange}
//rename Apple to Mcintosh
import Fruits.{Apple => Mcintosh, Orange}

//import all members in Fruits and rename Apple
import Fruits.{Apple => Mcintosh, _}

//hiding Apple
import Fruits.{Apple => _, _}

```


+   Scope of Protection

```scala
package x {

  import com.cib.accperf.scala.playground.y._
  private[playground] class ProtectScope {

    //won't work
    //val ysc = new yScopeClass
    private[this] val withinClass = 2
    val withinPlayground = 3
  }
}

package y {

  private[y] class yScopeClass {

    import com.cib.accperf.scala.playground.x._
    val ps = new ProtectScope
    println(ps.withinPlayground)
  }
}

```