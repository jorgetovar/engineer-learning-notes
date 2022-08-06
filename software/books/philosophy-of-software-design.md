# Complexity
Writing software is a creative activity and our biggest limitation is our ability to understand the systems we are creating
Software is all about complexity, your responsibility as a software developer is to create code that others can easily understand and extend. 

**Complexity**: Anything related to the structure that makes it hard to understand and modify the system

**Symptoms of complexity**:
- Change amplification: Cascade side effect
- Cognitive Load: Needs to know a lot of things
- Unknown unknowns: The system, classes, names, and functions are not obvious

The complexity of the system is defined as *C = Sum (Cp + Tp)*, the sum of the complexity of the parts and the time that you work on this.

What makes your software **Complex** is:
- Obscurity: No obvious
- Dependencies: Low isolation

What makes your software **Simple** is:
- Deep modules
- Strategic Programming
- Good Names

# Strategic vs Tactical Programming
Working code is not enough 
**Tactical**: Get as many features as possible
**Strategical**: Focus on great design and working code, *Spend 10-20% of development time on investments*

# Deep Modules
Small and simple interfaces with low functionality. Good abstractions

- Information hiding: What's the simplest interface that will cover my current needs? 

# Shallow Modules
Complex interfaces related to the functionality it provides

# Different layers, different abstractions

Each layer must have a different purpose. The code of the deeper layers serves the external ones, you must be aware of passthrough classes and variables, for example, with decorator patterns in most cases, you add new classes, variables, elements, and not too much new functionality.

Remember that every piece of software that does not add functionality is only increasing the complexity of the system

# Pull complexity downwards 

It's more important to have a simple interface than a simple implementation 

Look for opportunities to reduce the suffering of your users, for example, configuration parameters, instead of transferring the responsibility to the user client, make some computations and provide default values based on best practices. 

## Exceptions 
Exceptions add complexity and coupling. Reduce them as much as possible 

## Modifying existing code

Stay strategic. If you're not improving the design probably you are making it worse

## Consistency
A powerful tool for reducing complexity and making behavior more obvious. 
Naming, coding style, interfaces, APIs

## Code should be obvious 
Code must be easy to read, not to write

## Software trends
- Object-oriented programming
- Unit test
- TDD
- Agile

## Design for performance
Measure then make the improvements

# Conclusion
This is a book about software complexity. Dealing with complexity is the most important challenge of software design.

Things lead to complex software. Dependencies and obscurity

The reward for being a good designer is that you get to spend a larger fraction of your time in design, which is fun. Poor designers spend most of their time chasing bugs in complicated and brittle code. 