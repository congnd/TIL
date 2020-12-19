## A philosophy of software design

Key takeaways

### Design principles
- Complexity is incremental: you have to sweat the small stuff
- Working code isn't enough
- Make continual small investments to improve system design
- Modules should be deep
- Interfaces should be designed to make the most common usuage as simple as possible
- It's more important for a module to have a simple interface than a simple implementation
- General-purpose moduels are deeper
- Separate general-purpose and special-purpose code
- Defferent layers should have different abstractions
- Pull complexity downward
- Define errors out of existence
- Design it twice
- Comments should describe things that are not obvious from the code
- Software should be desinged for ease of read, not ease of writing
- The increatements of software development should be abdstractions, not features

### Red flags
- Shallow moduel: the interface for a class or method isn't much simpler than its implementation
- Information leakage: a design decision is reflected in multiple modules
- Temporal decomposition: the code structure is based on the order in which operations are executed, not on information hiding
- Overexposure: An API forces callers to be aware of rarely used features in order to use commonly used features
- Pass-through method: a method does almost nothing except pass its arguments to another method with a similar signature
- Repetition: a nontrivial piece of code is repeated over and over
- Special-general mixture: special-purpose code is not cleanly separeated from general purpose code
- Conjoined methods: two methods have so many dependencies that its hard to understand the implementation of one without understanding the implementation of the other
- Comment repeats code: all of the information in a comment is immediately obvious from the code next to the comment
- Implementation documentation contaminates interface: an interface comment describes implementation dtails not needed by users of the thing being documented
- Vague name: the name of a variable or method is so imprecise that it doesn't convey much useful information
- Hard to pick name: it is difficult to come up with a precise and intuitive name for an entity
- Hard to describe: in order to be complete, the documentation for a variable or method must be long
- Nonobvious code: the behavior or meaning of a piece of code cannot be understood easily


https://www.goodreads.com/en/book/show/39996759-a-philosophy-of-software-design
