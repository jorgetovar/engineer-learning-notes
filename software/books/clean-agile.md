 
# Clean Agile
 
[Clean Agile Book](https://www.amazon.com/Clean-Agile-Basics-Robert-Martin/dp/0135781868)


Great things are made of small teams making small things
Agile provides data
The customer doesn't know what he wants
Agile is not a mini-waterfall: Rather the activities of architecture, design, and implementation are continuous through the iteration
Agile is not about going fast is about destroying hope through continuous feedback
Adding manpower to a late project can have a negative impact: Learning curve, communication complexity
the only way to go fast is to go well
 
## Reasonable expectations
 
- We can't ship shit
- Continuous technical readiness: the system must be technically deployable at the end of the iteration (no QA stabilization, bypass demos)
- Stable productivity: Go very fast on greenfield project then slow down (mess in the code is accumulated)
The existing code is the most powerful instructor
- pouring the old code to redesign because this is all the documentation
The original developers had all been promoted and gone off to management positions. The current members of the team stood up in unison and said, “You can’t ship this, it’s crap. It needs to be redesigned.” (The tiger team never finished the new system design)
- Inexpensive adaptability: word. The word “ware” means “product.” The word “soft” means easy to change. Therefore, the software is a product that is easy to change
I’ve got some news for you, sunshine. If a change to the requirements breaks your architecture, then your architecture sucks.
- *Making changes relative inexpensive is our job*
- Continuous improvement: humans make better with time
The design and architecture of a software system should get better with time, and the structure of the code
- Fearless Competence: fear forces you to behave incompetently
- QA Should Find Nothing
- Test automation: Manual tests are expensive
- We cover for each other
- Honest estimates
- You need to say no
- Continuous aggressive learning
- Mentoring: the best way to learn is to teach
 
## Customer Bill of Rights
- An overall plan and to know what can be accomplished when and at what cost
- The most possible value out of every iteration
- See progress in a running system, proven to work by passing repeatable tests that you specify
 
## Developer Bill of Rights
- know what is needed with clear declarations of priority
- produce high-quality work at all times: business people can push for lower quality
- ask for and receive help from peers, managers, and customers
- accept your responsibilities instead of having them assigned to you
- change estimates -> estimates are not commitments
 
## Practices
 
### Technical practices
 
## Test-driven development
 
The three rules of TDD
- do not write any production code if there is not a failing test
- do not write more of a test than is sufficient to fail
- do not write more production code than is sufficient to pass the currently failing test
 
When you follow the Three Rules, the tests you end up writing become the code examples for the whole system
Test coverage is a team metric
hard to test is equal to some kind of coupling
Good tests enable refactoring and better design and remove the fear of cleaning, if nobody cleans the code eventually the code will become such a horrible mess of unmaintainable spaghetti
 
## Pairing
The goal is to spread and exchange knowledge, not concentrate on it
Increase collaboration and training
## Simple design
- Pass all the test
- Reveal the intent
- Remove the duplication
- Decrease elements
 
## Refactoring
Improve the structure of the code without altering the behavior
Red, Green, Refactor
Refactor is part of our job
 
### Team practices
 
## Continuous integration
Continuous Integration meant that developers checked in their source code changes and merged them with the mainline every “couple of hours.”
The build never be broke
## Metaphor (ubiquitous language)
Domain-driven design: Ubiquitous Language
its a thread of consistency that interconnects the entire project during every phase of its lifecycle
## Sustainable peace
As I matured, I realized that my worst technical mistakes were made during those periods of frenetic late-night energy.
That was the moment that I learned that a software project is a marathon, not a sprint, nor a sequence of sprints. To win, you must pace yourself
 
Working overtime is not a way to show your dedication to your employer. What it shows is that you are a bad planner, that you agree to deadlines to which you shouldn’t agree, that you make promises you shouldn’t make and that you are a manipulable laborer and not a professional.
 
You must sleep well
## Collective ownership
No one owns the code in an Agile project. The code is owned by the team as a whole. Any member of the team can check out and improve any module in the project at any time. The team owns the code collectively.
 
Maintain your ability to work outside your specialty
 
All progress in a module would stop when the author was not at work. No one else dared to work on something owned by someone else.
 
 
### Business practices
 
## Acceptance test
Must be written between BA, Developers, and QA
QA must write the unhappy paths and provide early feedback using the acceptance test
 
## Whole team (Cross-functional)
Shorten the distance between users and programmers
## Planning game
Breaking down the pieces and estimating them, estimates are imprecise by definition
 
Such estimates are composed of three numbers: best-case 95%, nominal-case 50%, and worst-case 5%.
A user story is an abbreviated description of a feature of the system, told from the point of view of a user
 
The wording is simple, and the details are omitted because it is too early to count on those details. We want to delay the specification of those details as long as possible
 
Iteration Zero: placeholders stories
Midpoint check
The end of the project is when the stories are not worth
Stories: INVEST
* Independent: Order of business value
* Negotiable: Check the cost of development
* Valuable: H/M/L
* Estimable
* Small: can be done in one iteration
 
Acceptance test writing should go quickly
The demo should include showing that all the acceptance tests run—including all previous acceptance tests
Always have a good story
Return of investment
* High cost - high value: do later
* Low cost - high value: do now
* Low value - high cost: do never
* Low value - low cost: do much later
 
## Small releases
The practice of Small Releases suggests that a development team should release their software as often as possible
 
### Values
 
#### courage
#### feedback
#### simplicity
#### communication
 
### Software craftsmanship
Agile coaches as another layer of management: people telling them what to do instead of helping them to get better at what they do.
 
Companies are still not mature enough to understand that technical problems are in fact business problems.
 
- Not only working software but also well-crafted software
- Not only responding to change but also steadily adding value
- Not only individuals and interactions but also a community of professionals
- Not only customer collaboration but also productive partnerships
 
Principles without practices are empty shells, whereas practices without principles tend to be implemented by rote, without judgment. Principles guide practices. Practices instantiate principles. They go hand in hand.
 
Craftsmanship also promotes the Clean Code and SOLID principles. It promotes small commits, small releases, and Continuous Delivery. It promotes modularity in software design and any type of automation that removes manual and repetitive work. And it promotes practices that improve productivity, reduces risk, and help to produce valuable, robust, and flexible software.
 
## Focus on the value, not the practice
 
A profession gives meaning to our lives


