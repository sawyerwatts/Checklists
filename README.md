# Checklists

In early 2024, Sawyer started reading *Code That Fits in your Head* by Mark Seemann, which argues
that developers should have checklists to help ensure they do not forget important aspects of the
software development lifecycle. Critically, not every checklist item needs to be implemented because
It Depends, but the value is to ensure that all the items are considered and not forgotten.

This is an amalgamation of various sources Sawyer has seen online, in books, and from experience.

Sawyer is a backend-focused developer that primarily writes web APIs and console/batch applications,
and occassionally writes CLI tools. This is the perspective these checklists are written from.

## Getting Started

Several docs call this out as well, but this is not strictly a top-down, waterfall list, anything
can feed back into "previous" steps, especially design and implementation.

- [ ] Remember: ownership of IT, engagement in business needs, and a holistic eye will produce the
best software.
- [ ] If working on pre-existing code, ensure familiarity with the project(s) at hand, including the
data flow throughout the application and its various states.
- [ ] Ensure the requirements are known (to an appropriate degree for the task at hand). If in
doubt, [Here](./requirements.md) are some very basic questions.
- [ ] Ensure the system, application, and/or feature has been (broadly or finely)
[designed](./design.md).
- [ ] Based off the app and feature type, see the relevant subpage under [app/](./app/)
- [ ] If a new repo is necessary, ensure [repo hygiene](./repoHygiene.md)
- [ ] If a new application is necessary, ensure [application hygiene](./appHygiene.md).
- [ ] If a new feature is being implemented, ensure [feature hygiene](./featureHygiene.md).
- [ ] Take a pass at [operations](./operations.md) as relevant

