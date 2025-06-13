# Tackling Complexity through Simplicity

Code provides value by representing and implementing knowledge, ideas, and responsibilities.
However, code itself is a cost because it itself is knowledge (and tests and liabilities and etc):
it must be read and learned and understood.

Code understandability has several components: Cognative Load, Simplicity, Modularity, and
Compatibility. *Locally* and *Cummulatively*, each of these can be evaluated in many ways, such as
*Essential* vs *Incidental*.

**Cognative Load** is the amount of domain, language, codebase, etc knowledge necessary to
understand a piece of code (and that code itself).

**Simplicity** is how focused a piece of code is (is it a single thread or is it many braids?).

**Modularity** is cognative load dependency analysis: how well does a piece of code encapsulate and
abstract away the target cognative load from the rest of the system, and how well it is isolated /
decoupled from the target cognative load of the rest of the system.

- Is it obvious what cognative load the module is and is not targeting?
- Is the interface small (thus likely simpler for clients and a good encapsulation)?
    - What are the important details?
    - What are the unimportant details?
    - Are these details' significance appropriately reflected in the interface?
- How could this module be implemented without any given dependency? Does making the module more
general-purpose reduce its dependencies?
- What is the blast radius for, and/or difficulty in implementing, any particular change to the
encapsulated knowledge, implementation details, or dependencies?
- Temporal decomposition is when a system is decomposed based off the workflow being implemented.
These are not (necessarily) good modules!

**Compatibility** is how well additions/changes use the existing cognative load versus introducing
more load, particularly at a cummulative level (remember, code is a cognative load).

## Design

Here are some design prompts. If possible, produce and evaluate several designs per prompt, even if
some designs are actively terrible. Remember, the objective is to solve the problem at hand. Once
written, consider code understandability.

- Consider direct, presumably low-implementation-cost design(s). Consider an addition-heavy design,
  and a change-heavy design, and whatever else comes to mind.
- Consider high-compatibility design(s).
- Consider high-modularity design(s).
    - Consider a generalized solution that can probably be reused in later use cases. If possible,
    consider several generalized solutions, building up from the direct solution and iterating to
    increase the generality, potentially until it's so general that it's unusable.

## Good Reads/Watches

- *On the Criteria To Be Used in Decomposing Systems into Modules* by David Parnas is a solid read,
but it's also very old and kinda obsoleted by future works.
- *No Silver Bullet—Essence and Accident in Software Engineering* by Fred Brooks defines two forms of
complexity: essential and incidental.
- *A Philosophy of Software Design* by John Ousterhout is a seminal in this section as it is the best
work I've encountered that discusses complexity and modularity in depth.
- *Simple Made Easy* by Rich Hickey is great too, he goes through the history of the words simple
and complex, pointing out how they have meant single threaded versus braided.

