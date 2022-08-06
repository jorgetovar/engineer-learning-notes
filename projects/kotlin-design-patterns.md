# Why Design patterns?

We as software developers don't want to reinvent the wheel, that's one of the reasons why I love Desing Patterns, But I think that we can go a step further and embrace functional programming to deliver simple solutions.

If you are a Software Developer who loves making great things and implements the best practices in the industry, probably you have heard about Design patterns. 

Design patterns provided a shared vocabulary and general solutions to design problems. You apply them to your specific application.

In the current state of software and the Object-Oriented Paradigm ruling the world, I started my journey a few years ago by reading [Head First Design Patterns](https://learning.oreilly.com/library/view/head-first-design/9781492077992/), but after that, I started to learn about Functional Programming and the new capabilities of languages like Java - 1.8 Release- and Kotlin and realized that: with Functional Programming, you can implement Design Patterns in a better and more concise way.  

### Strategy
For example, you can use the Strategy pattern in the OOP way to implement the four basic mathematical operations.

```kotlin
package com.jetprogramming.designpatterns.headfirst.strategy

interface Strategy {
    fun doOperation(num1: Int, num2: Int): Int
}

class OperationAdd : Strategy {
    override fun doOperation(num1: Int, num2: Int): Int {
        return num1 + num2
    }
}

class OperationMultiply : Strategy {
    override fun doOperation(num1: Int, num2: Int): Int {
        return num1 * num2
    }
}

class Context(private val strategy: Strategy) {

    fun execute(num1: Int, num2: Int): Int {
        return strategy.doOperation(num1, num2)
    }
}
```
With this design is easy to add new Operations only by extending the current code.

```kotlin
package com.jetprogramming.designpatterns.headfirst.strategy

fun main() {
    val contextAdd = Context(OperationAdd())
    val contextMultiply = Context(OperationMultiply())

    println(contextAdd.execute(1, 2))
    println(contextMultiply.execute(3, 2))
}
```
But..., I need a new class for every new Operation, can we do it better?. Maybe using Functions. Let's give it a try.

```kotlin
package com.jetprogramming.designpatterns.headfirst.strategy

class ContextFn(private val fn: (a: Int, b: Int) -> Int) {

    fun execute(n1: Int, n2: Int) = fn.invoke(n1, n2)
}
```
Kotlin is a powerful language that allows you to write code in functional style or OOP, it's up to you to decide the best solution for your context. 

In the Functional world, I only need to add a `subtraction function` to extend the current code, no new classes! no more ceremony.

```kotlin
package com.jetprogramming.designpatterns.headfirst.strategy

fun main() {
    val contextFnSum = ContextFn { x: Int, y: Int -> x + y }
    val contextFnMultiply = ContextFn { x: Int, y: Int -> x * y }
    val contextFnSubtraction = ContextFn { x: Int, y: Int -> x - y }

    println(contextFnSum.execute(1, 2))
    println(contextFnMultiply.execute(3, 2))
    println(contextFnSubtraction.execute(3, 2))
}
```
Wow, for my this was open eyer, working with pure functions, and composition is a clean way to design solutions! But what about full Functional Programming Languages like Clojure, Can you implement a solution like that without classes and objects?. Of course.

```clojure
(defn context [f n1 n2]
      (f n1 n2))

(defn -main
      "I don't do a whole lot ... yet."
      [& args]
      (println (context + 1 2))
      (println (context * 1 2))
      (println (context - 1 2))
      )
```
You know you don't want to reinvent the wheel, I think that by having the shared vocabulary of Design Patterns and making your solutions with the Business Logic in the core, with pure functions and immutable data structures, You can separate cleanly, what changes from what stays the same and deliver better outcomes with simple solutions.

### Command:

This is another important pattern in OOP when we can encapsulate method invocation so the object invoking the computation doesn’t need to worry about how to do things.

```kotlin
interface Command {
    fun execute()
}

class LoginCommand(private val user: String, private val password: String) : Command {
    override fun execute() {
        DB.login(user, password)
    }
}

class LogoutCommand(private val user: String) : Command {
    override fun execute() {
        DB.logout(user)
    }
}

fun main() {
    val loginCommand = LoginCommand("clojure", "kotlin").execute()
    val logoutCommand = LogoutCommand("clojure").execute()
    
}
```
But after all, we know that we can do it better with functions.

```clojure
(defn execute [command & args]
      (apply command args))

(execute login "clojure" "kotlin")
(execute login "clojure")

```
In **conclusion:** the code written with Functions and Design patterns is very modular, composable, reusable, and easy to reason about.

