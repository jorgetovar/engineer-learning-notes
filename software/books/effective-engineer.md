# Effective Engineer

This is a [great book](http://www.effectiveengineer.com) about the **Value** produced / the **Time** invested. One of the most important takeaways is: Effort is not proportional to impact. This is open-eyer for me, I have been in projects with an expectation of 70 hours per week of work, with a lot of technical debt, and in retrospect poor outcomes. Finally, this is unsustainable, nobody on the team remains in this company. 

# Part 1: Adopt the Right Mindsets

## Chapter 1 - Focus on High-Leverage Activities

- **Leverage = Impact produced / Time invested**

Increase your leverage in three ways:

1. Reducing time for activities
2. Increase the outcome of activities
3. Shift to high-leverage activities

We have to be critical about the tasks and the time that we allocate in our day-to-day job. **Remember effort != impact**. In a performance review, you give more credit to the people that create the important stuff, release it frequently, and improve organization productivity, not to workaholic that doesn't get anything done. 

## Chapter 2 - Optimizing for learning

**Compounding leads to an exponential growth curve**

- The earlier that you learn, the more compound knowledge you will have.
- Seek work environments that allow you to invest in learning: 
  - Training
  - Feedback
  - Autonomy
  - Smarter people
- Dedicate time on the Job to improve your skills:
  - Read core abstractions of your organization and improve some of them
  - Jump into code you don't know
  - Seek feedback
  - Participate in design discussions
- **Always be learning:**
  - New languages and frameworks
  - Read books
  - Attend talks, conferences, and meetups
  - Networking
  - Pursue what you love
  - Write to clarify your thoughts [Blog]

One of the most important takeaways here is to **own your story**, your company is not responsible for your career. You have to make time to develop your skills and be a Software Craftsman.

## Chapter 3 - Regular prioritization and execution

- Track to-dos in a single and easily accessible list: I like to use Trello, and plan week work, remember the **human brain is better at processing than saving information**. 
- Ignore tasks that don't produce value, **I LOVE to skip useless meetings, nobody will complain about you if you use this time for HACKING and produce something useful**.
- **Focus on important and not urgent**
  - Important + Urgent: Pressing issues, deadlines, some bugs
  - **Important + Not Urgent**: The magic happens here, it's important, but you have time to solve the problem by thinking in the long term like personal development, tooling, and guidelines. 
  - Urgent + Not important: *Most meetings*, emails
  - Not important + Not urgent: Busywork, time wasters

You always have more tasks to do than time to execute them, you have to be aware of this and prioritize constantly to get better outcomes.
- Protect your maker's scheduler : [Paul Graham](https://twitter.com/paulg?ref_src=twsrc%5Egoogle%7Ctwcamp%5Eserp%7Ctwgr%5Eauthor) explains this better than me [Makers schedule](http://www.paulgraham.com/makersschedule.html). In summary, don't break the *Flow* of developers **[2-3] hours time boxes**
- Use if-then plans: I have never heard about this before, but is: "If I don't have that meeting I will refactor the payment component", very useful.
- Prioritize, Prioritize, Prioritize.
- **Limit the Work in Progress**: one of the most important pieces of advice for me. I like to deploy in small batches, CI/CD, pair programming, and reduce context switches at all costs. 

# Part 2: Key strategies, Execute!

## Chapter 4 - Invest in iteration speed

To deliver value to the users, you have to release your software. The unique constant in software is *change*. So, it's extremely important to increase your feedback loop, engage the users and avoid wasting your time.

- Move fast to learn fast: learn quickly, and remember that **learning compounds**.

- Invest in tools that increase productivity and reduce errors:  Tools and [Design](https://martinfowler.com/bliki/DesignStaminaHypothesis.html) always payoff because they make it easier to evolve the software into the future and reduce burnout.

- Debugging and Validation Loops: I prefer the always trustworthy **TDD**, but one nice practice that I rescue for this section, is making shortcuts to test, for example in Web Development when you have to click 10 times to load the page with the bug.

- **Master Your Programming Environment**: 

  - Get proficient with the IDE or text editor: Try to make everything with shortcuts
  - Learn one high-level programming language **-For example Python, you can use boto3 for AWS Cloud, and a lot of nice libraries for Data Science, and boring manual tasks-**
  - Get Familiar with Unix shell commands
  - Make it easy to run your Tests
  - Use the REPL: My favorite language is **Clojure**, and one of the reasons I love it, is because you can test your ideas in the [REPL](https://clojure.org/guides/repl/introduction)

- **Remove Non-Engineering Bottlenecks**

  - Seek team autonomy -Remove bureaucracy-
  - Improve overall communication. I always push for public channels and transparency in information.
- Improve the review Process: **Pair programming** is a good way to do it, in the early stages feedback is more valuable and cheaper

I always promote Clojure over almost all languages, it embraces Functional Programming, Simplicity, and Concurrency, all elements necessary to make good software, but in this case, Python is on the rise in adoption, and the libraries to interact with Cloud providers are just amazing, learn both!.

## Chapter 5 - Measuring what we want to improve

If you choose wisely the right metrics, it's possible to improve overall organizational performance and set a clear vision for the Team.
For example in the [Hello, Startup](https://dev.to/jorgetovar621/hello-startup-book-review-5e03) book they show us five basic metrics for almost any startup:
- **Acquisition**: how users find your product
- **Activation**: how many users engage with your product
- **Retention**: how many users use the product again
- **Referral**: existing users of your product helping you acquire new users
- **Revenue**: how much money you’re making and what channels it’s coming through
- **The magic number**: some metric that, when the user crosses it, they have an “a-ha” moment and finally “get” the product.

The Right Metrics can drive the right behavior, culture, and priorities. I like the [ELK Stack](https://www.elastic.co/what-is/elk-stack) to instrument the system, it's important to keep in mind that decisions with data are most likely to be better, however, is important to be aware of the data integrity.

Carefully choose your key metrics, one of the most **subjective metrics in the agile world is velocity** -Not useful for performance-, If you want to track the performance of the Team use this [4 DevOps Metrics](https://www.thoughtworks.com/radar/techniques/four-key-metrics)- **Lead Time, Deployment frequency, Mean Time to Restore and change fail percentage**. Another useful metric is hours worked per week. I believe that features that impact the OKRs and increase the growth of the startup are most important.  

## Chapter 6 - Validate Ideas early and often

- **Find the low-effort activities to validate your hypothesis**
- Use A/B testing: A very useful technique, if you have good metrics and can activate based on certain conditions, you will build the right product.
- Seek out Feedback: show the product to the users
- Collect data and assess the value of your work

One of the most valuable lessons in Lean is to **minimize waste** if you are creating a product and don't show the results to the stakeholders. Probably we are working on the wrong thing. Use metrics and data to test your hypothesis, remember **learning is compound**, and the faster that you get the sense that you're on the right path the best.
 
## Chapter 7 - Project estimation skills

- Descompose into granular taks
- **Think of estimates as probability distributions**: Maybe the most important takeaway from this chapter is, In Uncle Bob's talk, he shows us how to give scenarios to the product and business people - worst, most probably, best scenario-.
- Beware the mythical man-month: **More resources and people are not proportional to the speed**
- Timeboxing to constrain tasks that can grow in scope
- Budget for the unknown: keep some time for unplanned work -Buffer-
- Define Milestones
- Software is the long-term task: additional hours increase burnout and reduce quality, don't sprint in a marathon

We as Software Developers, tend to fail in our estimations, maybe is an excess of confidence, lack of understanding or a lot of push for the business people **-Some developers are too bad at dealing with people-**, but professionals, have the responsibility to say NO in some cases, provide options and give better data to our stakeholders.

This nice talk gives some good points about this [Uncle Bob Talk](https://www.youtube.com/watch?v=0UvbdVbckfk)

# Part 3: Approaches for building long-term value

## Chapter 8 - Balance quality with pragmatism

- Implement a good code review process: Pair programming, Github, Guidelines, Mentoring
- Increase accountability for code changes
- Create good abstractions: Pursuit simplicity [Rich Hickey Talk](https://www.youtube.com/watch?v=f84n5oFoZBc)
- Scale code quality with automated testing: use TDD, ATDD, **it frees you from the fear of changing and improving the design**
- Manage your technical debt: Remember we have to pay all the debts, don't let it increases to a point to become unplayable

If you haven't read it. stop right now and buy this amazing book [The Pragmatic Programmer](https://pragprog.com/titles/tpp20/the-pragmatic-programmer-20th-anniversary-edition/), **almost all the decisions in Design and Software are about trade-offs**, it important to have speed but please don't sacrifice quality, we are professionals and craftsmen, be proud of your code.

## Chapter 9 - Minimize the operational burden

- Build systems to fail fast: Don't hide errors, allow them to reach the users, the feedback is more quickly, and possibly find problematic issues sooner
- Operation simplicity: I like **Infrastructure as Code**, Terraform, and CDK of AWS are just amazing. Implement CI/CD pipelines, with automation, is simpler to develop and create products
- Automate mechanical tasks: **don't sacrifice long-term benefits for short-term activities**, don't underestimate the frequency of the task
- Make process idempotents: Clojure is a wonderful language, in the learning path I learn to appreciate pure functions, and minimize side effects and mutability, all these tools enable you to create idempotent transactions
- Ability to Respond and Recover Quickly: Infra as code and observability, have a DRP plan, and maybe if you are mature enough *Chaos engineering*

Wonderful topic, in the DevOps world we are embracing accountability on the teams, we have to maintain and operate the products we create, for this reason, is important to make operations as code, and make frequent and reversible changes. [AWS operations pillar](Make frequent, small, reversible changes) show us the importance of thinking in running workloads effectively,

## Chapter 10 - Invest in our team's growth

**Learning is compound!**
- Hiring and onboarding is a team Job
- Impart teams culture and values
- Expose the fundamentals of your startup
- Mentor

**Build a great Engineering Culture**, promote ownership of the code, participate in post-mortems and write about it. Startups are about people, so, investing in growth is a high-leverage activity.

# In conclusion

This book has some fundamental ideas about effectiveness, key takeaways are **Effort!=Impact**, our **most important resource is TIME** so use it wisely, **embrace simplicity** and automation, make prioritization a habit, **focus on important and not urgent**, and finally invest in learning, because learning is compound, the sooner you do it, better the returns.

 
