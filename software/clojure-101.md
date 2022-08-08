>"We should aim for simplicity because simplicity is a prerequisite for reliability"
— Edsger W. Dijkstra

How to become a 10x Developer?. What's the secret to being one of the top performers in the industry?. The Software world is changing our environment and allowing us as developers, to have different challenges day to day, defying our imagination and skills of **problem-solving**.

Investing in knowledge has one of the biggest ROI, mastering the craft of software, and the tools that we use to build our world is an essential part of our career. The best way to solve software problems is with pragmatism, designing simple and maintainable solutions. To be effective we need the best tools (PC, IDE, terminal, programming language, cloud services, text editors, etc).

Talking about programming languages: I believe that **Clojure** is maybe the best solution for problem-solving with simplicity, data-driven, and functional programming in mind, Clojure makes emphasis information, concurrency, and business logic without all the ceremony and unnecessary complexity imposed by the traditional languages, that lead us to a world of bad abstractions, rot and verbose code.


```java
// Hello world in Java
public class Main {
    public static void main(final String[] args) {
        System.out.println("Hello world!");
    }
}
```
```clojure
; Hello world in Clojure
(println "Hello world!")
```

One of the most important characteristics of Clojure is to be a dialect of Lisp; in this article, we are going to explore one of the most important elements of this language, the *Syntax*. -pending functions and data-.

##Syntax

###Forms

Clojure code has a uniform structure: Literal representations of data structures and operations.

> `;` Comment in clojure
```clojure
;(operator operand1 operand2)
(+ 1 2)
; => 3

(str "My fist article " "in DEV")
; => "My fist article in DEV"
```

Clojure syntax is consistent in all the data structures, unlike Java where the syntax depends on the structure, class or data to be processed.

```java
System.out.println(1 + 2);
// => 3
System.out.println("My first article ".concat("in DEV"));
// => "My fist article in DEV"
```

###Control Flow

Clojure operators are expressions, because they return values, unlike sentences that have side effects and occasionally return values.

```clojure
(if (> 3 1)
  "3 is greater than 1"
  "???")
; =>  "3 is greater than 1"
```
In languages like Java, we can return values if we use the *ternary*, the traditional way is a sentence.

```java
public String isGreaterThan() {
    return (3 > 1) ? "3 is greater than 1" :
            "???";
}
```

###Immutability

Clojure is a functional language with immutable data structures that allow us to share data without problems between threads and functions.

```clojure
; Domain information, Clojure is data-driven!!
(def person 
  {:first-name "Jorge"
   :last-name "Tovar"
   :age 30
   :occupation "Programmer"})
; Query
(:occupation person) 
; => "Programmer"
; Update over immutable structures don't change the initial definition
(assoc person :occupation "DevOps")
; => {:first-name "Jorge", :last-name "Tovar", :age 30, :occupation  "Devops"}
; Query the original structure
person
; => {:first-name "Jorge", :last-name "Tovar", :age 30, :occupation "Programmer"}
```

###Values with Name
Clojure Functions are first-class elements and with his REPL is possible to test and reduce the feedback loop.

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/22x42ibk279loegjjqxb.png)

Clojure is data-driven and promotes the use of immutable values, unlike OOP languages where variables and mutability are the default.

```clojure
; Clojure functions that return values
(def x (user-message :simplicity)); => "Clojure loves simplicity"
(def y (user-message :other)); => "Clojure loves functional programming!"
```
In Python the common way is to use variables:

```python
def user_message(message):
    if message == "simplicity":
        x = "Python loves " + "dynamic types"
    else:
        x = "Python " + "loves OOP"
    return x
```
###Conclusion

The Clojure way focuses on simplicity, pure functions, and expressiveness, key elements to building reliable software.

The Functional Paradigm is one of the most interesting and powerful, it empowers us to create clean systems. If you love to learn like many of the developers of the world I suggest you try this language, which is a different point of view to solving problems and minimizing the complexity and code lines.

### References 
[Clojure for the brave](https://www.braveclojure.com/introduction/)


