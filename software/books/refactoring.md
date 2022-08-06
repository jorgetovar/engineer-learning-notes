# Refactoring 

[Refactoring Book](https://www.amazon.com/Refactoring-Improving-Design-Existing-Code/dp/0201485672)

Refactoring is the process of changing the software system in a way that does not alter the external behavior of code yet improves its internal structure

When you have to add a feature to a program but the code is not structured conveniently, first refactor the program to make it easy to add the feature, then add the feature.

Whenever I do refactoring, the first step is always the same. I need to ensure I have a solid set of tests for that section of code. The tests are essential because even though I will follow a refactoring structured to avoid most of the opportunities for introducing bugs

Refactor starts from the deepest branch because you need less context
Testing starts from the shortest branch and specialized in the test

add understanding through functions that reveal the intention

small changes and testing after each change

It’s my coding standard to always call the return value from a function “result”. That way I always know its role. Again, I compile, test, and commit. Then I move on to the first argument.

Any fool can write code that a computer can understand. Good programmers write code that humans can understand.

Naming is both important and tricky. Breaking a large function into smaller ones only adds value if the names are good

When programming, follow the camping rule: Always leave the code base healthier than when you found it.

The true test of good code is how easy it is to change it.

## Principles in refactoring

Refactoring (noun): a change made to the internal structure of software to make it easier to understand and cheaper to modify without changing its observable behavior.

- Two hats
    * Refactoring
    * New Features

- Refactoring improves the design of the software
- Refactoring makes the software easy to understand
- Refactoring helps me find bugs
- Refactoring helps me program faster
- Refactoring in code review

### When should we refactor?

- Rule of three (Duplication)
- Making it easy to add a feature
- Making code easier to understand
- Opportunistic refactoring (is part of programming tasks)

Refactoring is part of our job as software professionals are not necessary to tell the managers

### When should I not refactor?

- **If I run across code that is a mess, but I don’t need to modify it, then I don’t need to refactor it.**
- **It's easy to rewrite**

### Problems with refactors

- Slow down the features? : Producing more value with less effort
- Code ownership
- Continuous integration to slow down the complexity of the merge 
- Testing
- Architecture and Design process must be flexible and refactored constantly

## Bad Smells in Code

- Mysterious name
- Duplicated code
- Long function
- Long parameter list
- Global data
- Mutable data
- Shotgun surgery: Orthogonality
- feature envy: The fundamental rule of thumb is to put things together that change together
- Data clumps: group data
- Primitive obsession: use value objects when applying
- Repeated switches: Use polymorphism
- Loops
- Lazy element
- Speculative generality: "Oh, I think we’ll need the ability to do this kind of thing someday"
- Temporary field
- Message chains: You see message chains when a client asks one object for another object, which the client then asks for yet another object, which the client then asks for yet another object, and so on
- Middle man
- Large classes
- Data classes: setter?
- Comments: often a consequence of bad code

## Building Test

Refactoring is a valuable tool, but it can’t come alone. To do refactoring properly, I need a solid suite of tests to spot my inevitable mistakes. Even with automated refactoring tools, many of my refactorings will still need checking via a test suite.

I don’t find this to be a disadvantage. Even without refactoring, writing good tests increases my effectiveness as a programmer. This was a surprise for me and is counterintuitive for most programmers—so it’s worth explaining why.

### The value of self-testing code

Make sure all tests are fully automatic and that they check their results.
A suite of tests is a powerful bug detector that decapitates the time it takes to find bugs.

Run tests frequently. Run those exercising the code you’re working on at least every few minutes; run all tests at least daily.

Think of the boundary conditions under which things might go wrong and concentrate on your tests there.


## Catalog

### Extract function

I accepted this principle, I developed a habit of writing very small functions

### Inline the function

A function in which the body is as clear as the name, avoid too much indirection

### Extract variable

Add some context

### Inline variable

Inline when the name don't communicate something useful

### Change function declaration

Functions represent the primary way we break a program down into parts. Function declarations represent how these parts fit together—effectively, they represent the joints in our software systems

aka: Rename Function
formerly: Rename Method
formerly: Add Parameter
formerly: Remove Parameter
aka: Change Signature

### Encapsulate variable

### Rename variable
Naming things well is the heart of clear programming

### Introduce a parameter object

I often see groups of data items that regularly travel together, appearing in function after function. Such a group is a data clump, and I like to replace it with a single data structure.

### Combine functions into class

### Split phase
When I run into code that’s dealing with two different things, I look for a way to split it into separate modules

## Encapsulation

Perhaps the most important criteria to be used in decomposing modules is to identify secrets that modules should hide from the rest of the system

## Moving features

So far, the refactoring has been about creating, removing, and renaming program elements. Another important part of refactoring is moving elements between contexts

## Organizing data

Data structures play an important role in our programs, so it’s no great shock that I have a clutch of refactoring that focus on them

## Simplifying conditional logic

## Refactoring APIs
- Separate queries from APIs

