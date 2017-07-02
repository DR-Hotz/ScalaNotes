# Trait

+   Defines a type
+   **cannot** have any "class" parameters
+   super calls are dynamically bound
    *   must mark such methods as **abstract override**, which is only allowed for members of traits and means that the trait must be mixed into some class that has a concrete definition of the method
    *   provide a way to modifiy parameters as many times as you want by "stacking traits" before "actually" calling the last concrete one.
+   The order of mixins is significant
    *    traits further to the right take effect first(from right to left)

```scala
package com.cib.accperf.scala.playground

import scala.collection.mutable.ArrayBuffer

abstract class IntQueue {
  
  def put(x: Int)
  def get(): Option[Int]
}

class BasicIntQueue extends IntQueue {

  private val buf = new ArrayBuffer[Int]
  
  def put(x: Int) = buf += x
  def get() = if(buf.isEmpty) None else Option(buf.remove(0))
}

trait Doubling extends IntQueue { 
  abstract override def put(x: Int) = super.put(x * 2)  
}

trait Increment extends IntQueue {
  abstract override def put(x: Int) = super.put(x + 1)
}

trait Filtering extends IntQueue {
  abstract override def put(x: Int) = if(x >= 0) super.put(x)
}

/**
 * Provide the trait Doubling with a concrete put implementation.
 */
class MixedInQueue extends BasicIntQueue with Doubling

class IncAndFilQueue extends BasicIntQueue with Filtering with Increment

class FilAndIncQueue extends BasicIntQueue with Increment with Filtering


/**
 * Trait extends a non-abstract class
 */
class StringQueue(val str: Array[String]) {

  protected var curIndex = 0;
  protected var getIndex = -1;

  def put(s: String): Unit = {
    str(curIndex) = s
    curIndex = curIndex + 1
  }

  def get(): Option[String] = {
    if(getIndex + 1 >= curIndex)
      None
    else {
      getIndex = getIndex + 1
      Option(str(getIndex))
    }
  }

  def ci = curIndex
  def gi = getIndex
}

trait DoublingString extends StringQueue{
  abstract override def put(s: String) = super.put(s * 2)
  abstract override def get() = {println("Doubling getting..."); super.get()}
}

class BasicStringQueue(s: Array[String]) extends StringQueue(s) {
  override def put(s: String) = {println(s"putting $s"); super.put(s)}
  override def get() = {println("Getting..."); super.get()}
}

class MixedStringQueue(s: Array[String]) extends BasicStringQueue(s) with DoublingString
```

+   linearization : when instantiating a class with new, Scala takes the class, and all of its inherited classes and traits, and puts them in a single, linear order, whenever you call super inside one of those classes, the invoked method is the **next one up the chain**

## To trait or not to trait

+   If it might be reused in multiple, unrelated classes, make it a trait
+   If you want to inherit from a trait in Java code, use an abstract class