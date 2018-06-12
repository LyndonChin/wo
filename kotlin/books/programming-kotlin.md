# Programming Kotlin

## Gettting Started with Kotlin
### Command Line
```bash
$ kotlinc HelloWorld.kt -include-rumtime -d HelloWorld.jar
$ java -jar HelloWorld.jar
```

```bash
$ java -cp $KOTLIN_HOME/lib/kotlin-runtime.jar:HelloWorld.jar HelloWorldKt
```

### REPL
```bash
$ kotlinc-jvm -cp joda-time-2.9.4/joda-time-2.9.4.jar
```

### Scripting
```kotlin
import java.io.File
val purgeTime = System.currentTimeMillis() - args[1].toLong() * 24 * 60 * 60 * 1000
val folders = File(args[0]).listFiles { file -> file.isFile }
folders?.filter { 
    file -> file.lastModified() < purgeTime 
}?.forEach {
    file -> println("Deleting ${file.absolutePath}")
    file.delete()
}
```

```bash
$ kotlinc -script delete.kts . 5
```

### Gradle
> the modern open source polyglot build automation system

```bash
$ gradle help --task :init
$ gradle init --type java-library
```

[示例代码](https://github.com/PacktPublishing/Programming-Kotlin/blob/master/Chapter01/gradle_project/build.gradle)

## Kotlin Basics

### Vals and vars
### Type inference
**Type inference** reduces biolerplate whilst keeping the type safety we expect of a modern language.

### Basic types

* Primitive types
    * cannot be used as generic types
    * do not support method/function calls
    * cannot be assigned null

* Java introduced wrapper objects to offer a work around in which primitive types are wrapped in objects. 
* Kotlin removes this necessity entirely from the language by promoting the primitives to full objects.
* Whenever possible, the Kotlin compiler will map basic types back to JVM primitives for performance reasons.

#### Numbers

Type | Width
--- | ---
Long | 64
Int | 32
Short | 16
Byte | 8
Double | 64
Float | 32

* Kotlin does not support automatic widening of numbers

#### Booleans

#### Chars

#### Strings
* Strings are immutable

#### Arrays
* primitive type array
    * `ByteArray`
    * `CharArray`
    * `ShortArray`
    * `IntArray`
    * `LongArray`
    * `BooleanArray`
    * `FloatArray`
    * `DoubleArray`

### Comments

### Packages
* The package name is used to give us the **fully qualified name (FQN)** for a class, object, interface, or function.

### Imports

#### Wildcard imports
#### Import renaming

```kotlin
import com.packt.myproject.Foo
import com.packt.otherproject.Foo as Foo2

fun doubleFoo() {
    val foo1 = Foo()
    val foo2 = Foo2()
}
```

### String templates

### Ranges

```kotlin
val aToZ = "a".."z"
val isTure = "c" in aToZ
val oneToNine = 1..9
val isFalse = 11 in oneToNine
```

```kotlin
val countingDown = 100.downTo(0)
val rangeTo = 10.rangeTo(20)
```

```kotlin
val oneToFifty = 1..50
val oddNumbers = oneToFifty.step(2)
```

```kotlin
val countingDownEvenNumbers = (2..10).step(2).reversed()
```

### Loops

```kotlin
for (index in array.indices) {
    println("Element $index is ${array[index]}")
}
```
### Exception handling
### Instantiating classes
### Referential equality and structural equality
* `===`: Referential equality
* `==`: Structural equality (use the `equal` function)

The `==` operator is null safe.

### This expression
#### Scope

### Visibility modifiers
#### Private
#### Protected
#### Internal

### Control flow as expressions
* An expression is a statement that evaluates to a value.
* A statement, on the other hand, has no resulting value returned.

The `if..else` and `try..catch` control flow blocks are expressions.

```kotlin
val success = try {
    readFile()
    true
} catch (e: IOException) {
    false
}
```

### Null syntax
> Tony Hoare, the inventor of the quicksort algorithm, who introduced the concept of the null reference in 1965 called it his "billion dollar mistake".

#### Smart casts
> Which variables can be used in a smart cast is restricted to those that the compiler can guarantee do not change between the time when the variable is checked and the time when it is used.

```kotlin
fun isEmptyString(any: Any): Boolean = any is String && any.length == 0
}
```

```kotlin
fun isNotStringOrEmpty(any: Any): Boolean = any !is String || any.length == 0
```

#### Explicit casting
`ClassCastException`

### When expression
> The functional programming concept of pattern matching has become more mainstream.

#### When(value)

```kotlin
fun isZeroOrOne(x: Int) = when (x) {
        0, 1 -> true
        else -> false
    }
}

fun isAbs(x: Int) = when (x) {
    Math.abs(x) -> true
    else -> false
}

fun isSingleDigit(x: Int) = when (x) {
    in -9..9 -> true
    else -> false
}

fun isDieNumber(x: Int) = when (x) {
    in listOf(1, 2, 3, 4, 5, 6) -> true
    else -> false
}

fun startsWithFoo(any: Any) = when (any) {
    is String -> any.startsWith("Foo")
    else -> false
}
```

#### When without argument
```kotlin
fun whenWithoutArgs(x: Int, y: Int) {
    when {
        x < y -> println("x is less than y")
        x > y -> println("X is greater than y")
        else -> println("X must equal y")
    }
}
```

### Function Return
* By default, return returns from the nearest enclosing function or anonymous function.
* If we need to return a value from a closure, then we need to qualify the return with a label, otherwise the return would be for the outer function.

### Type hierarchy

* `Unit`
    * Having a Unit type is common in a functional programming language
    * Unit is a proper type, with a singleton instance, also referred to as `Unit` or `()`.
* `Nothing` : bottom type
    * `Nothing` can be used to inform the compiler that a function never completes normally
    * Empty mutable collections: `emptyList()`, `emptySet()`

## Object-Oriented Code in Kotlin
> Kotlin is an object-oriented programming (OOP) language with surpport for higher-order functions and lambdas.

* The first successful OOP language: SmallTalk created by Alan Key.
* *The Early History Of Smalltalk*
    * Everything is an object
    * Objects communicate by sending and receiving messages (in terms of objects)
    * Objects have their own memory (in terms of objects)
        * You can create an object by composing other objects
    * Every object is an instance of a class (which must be an object)
    * The class holds the shared behaviro for its instances (in the form of objects in a program list)

* The three pillars of nay modern OOP languages:
    * Encapsulation
    * Inheritance
    * Polymorphism

* Benefits of OOP
    * Simplicity
    * Modularity
    * Modifiability
    * Extensibility
    * Resusability

### Classes
> The concept of a class was first studied by **Aristotle**.

```kotlin
class Person(val firstName: String, val lastName: String, val age: Int?) {
    init {
        require(firstName.trim().length > 0) { "Invalid firstName argument." }
        require(lastName.trim().length > 0) { "Invalid lastName argument." }
        if (age != null) { 
            require(age >= 0 && age < 150) { "Invalid age argument" }
        }
    }
}
```

### Access levels

### Nested classes

### Data classes

### Enum classes
```kotlin
interface Printable {
    fun print()
}

public enum class World : Printable {
    HELLO {
        override fun print() {
            println("World is HELLO")
        }
    }

    BYE {
        override fun print() {
            println("World is BYE")
        }
    }
}

val w = World.HELLO
w.print()
```

### Static methods and companion objects

In Kotlin, it is advisable to define methods at the package level to achieve the functionality of static methods.

Class initializer is called only once, JVM will make sure this happens, before:
* An instance of the class created
* A static method of the class is invoked
* A static field of the class is assigned
* A non-constant static field is used
* An assert statement lexically nested within the class is executed for a top-level class

### Interfaces
* An interface is nothing more than an contract. 
* Unlike abstract classes, an interface cannot contain state; however, it can contain properties.

### Inheritance

### Visibility modifiers

[The Liskov Substitution Principle (LSP)](https://www.tomdalling.com/blog/software-design/solid-class-design-the-liskov-substitution-principle/)

### Abstract classes

## Functions in Kotlin
## Higher Order Functions and Functional Programming
## Properties
## Null Safety, Reflection, and Annotations
## Generics
## Data Classes
## Collections
## Testing in Kotlin
## Microservices in Kotlin
## Concurrency
