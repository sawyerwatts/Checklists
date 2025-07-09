# Design

Design can be applied to systems, individual applications, features within an application, classes,
and functions. This info is more relevant at higher levels, but not irrelevant to lower levels.

## Philosophy and Terminology

Code provides value by representing and implementing knowledge, ideas, and responsibilities.
However, code itself is a cost because it itself is knowledge (and tests and liabilities and etc):
it must be read and learned and understood.

Code understandability has several components: Cognative Load, Focus, Clarity, Modularity, and
Compatibility. Critcally, these each can be evaluated *Locally*/*isolated* and
*globally*/*Cummulatively*, as well as *Essential* vs *Incidental*. In regards to decoupling, there
is *mechanical*/*syntactic* decoupling and *conceptual*/*semantic* decouling.

Of note, this does not use the term simplicity because simplicity is in the eye of the beholder,
And it is better to use one of these other more specific terms.

**Focus** is how focused a piece of code is (is it a single thread or is it many braids?).
Focus is not **Clarity**, because focused code can be very obtuse (like code that does bit
operations - it's very focused on doing just that operation, but it's likely not clear what that
operation means).

**Cognative Load** is the amount of domain, language, codebase, etc knowledge necessary to
understand a piece of code (and that code itself).

**Modularity** is cognative load dependency analysis: how well does a piece of code encapsulate and
abstract away the target cognative load from the rest of the system, and how well it is isolated /
decoupled from the target cognative load of the rest of the system.

- [ ] Is it obvious what cognative load the module is and is not targeting?
- [ ] Is the interface small (thus likely simpler for clients and a good encapsulation)?
    - [ ] What are the important details?
    - [ ] What are the unimportant details?
    - [ ] Are these details' significance appropriately reflected in the interface?
- [ ] How could this module be implemented without any given dependency? Does making the module more
general-purpose reduce its dependencies?
- [ ] What is the blast radius for, and/or difficulty in implementing, any particular change to the
encapsulated knowledge, implementation details, or dependencies?
- [ ] Temporal decomposition is when a system is decomposed based off the workflow being
implemented. These are not (necessarily) good modules!

**Conceptual Cohesion** is how well additions/changes use the existing cognative load versus
introducing more load, particularly at a cummulative level (remember, code is a cognative load).

### Good Reads/Watches

- *On the Criteria To Be Used in Decomposing Systems into Modules* by David Parnas is a solid read,
but it's also very old and kinda obsoleted by future works.
- *No Silver Bullet—Essence and Accident in Software Engineering* by Fred Brooks defines two forms of
complexity: essential and incidental.
- *A Philosophy of Software Design* by John Ousterhout is a seminal in this section as it is the best
work I've encountered that discusses complexity and modularity in depth.
- *Simple Made Easy* by Rich Hickey is great too, he goes through the history of the words simple
and complex, pointing out how they have meant single threaded versus braided.

## Design Drafting and Evalation

If possible and relevant, produce and evaluate several designs per prompt, even if some designs are
actively terrible. Remember, the objective is to solve the problem at hand.

- [ ] What is your first blush design? When in doubt, just make a design.
- [ ] Consider direct, presumably low-implementation-cost design(s) - these are likely
addition-heavy designs.
- [ ] Consider high-conceptual-cohesion design(s) - these are likely change-heavy designs.
- [ ] Consider high-modularity design(s). Consider a generalized solution that can probably be
reused in later use cases. If possible, consider several generalized solutions, building up from a
direct solution and iterating to increase the generality, potentially until it's so general that
it's unusable.

## Design Evaluation and Feedback

Here are some criteria to evaluate the designs, ideally both in isolation and cummulatively (these
will likely recursively impact designs):

- [ ] Does it handle the happy path, sad path, and bad path?
    - [ ] Do partial failures exist, especially in batch jobs? How well are those handled?
- [ ] How's the focus?
- [ ] How's the clarity?
- [ ] How's the cognative load?
- [ ] How's the modularity? Is there semantic decoupling or only syntactic?
- [ ] How's the conceptual cohesion?
- [ ] How's the implementation cost (particularly for setting up the infrastructure)?
    - [ ] If relevant, how's the DevFinOps?
- [ ] Are there any events that would be beneficial to push?
- [ ] How much idempotency can there be? Where is the point of no return?
- [ ] How much can state be isolated? Less state means less that needs to be reset upon failure
- [ ] Is there an appropriate amount of checkpointing? Is there an appropriate amount of durability?
- [ ] Is a cache worth it?
- [ ] How does the design fare against [scalability and SLA requirements](./scalabilityAndSla.md)?
- [ ] How does the design fare against [security concerns](./security.md)?
- [ ] Ensure design decisions are documented.

