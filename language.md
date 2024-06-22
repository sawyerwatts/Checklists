# Language

These checklists are mildly to severely opinionated, and this checklist is
particularly opinionated since dynamic languages are just for glue scripts.

## Language Features

- Static typing FTW
- Sum types are a plus (and exhuastive pattern matching as well)
- Nulls are explicitly built into the type system
- How well does it support returning domain errors and throwing developer
errors
- Ensure the language has directory-level encapsulation
    - Ideally, the language can import subdirs' types into a superdir and
    re-export select types, also allowing for encapsulation of the subdir
    itself
- Validation library

## Frameworks and QOC libraries

- Settings library
    - Read settings from file, env vars, CLI, etc
    - A designated spot for user secrets, at least when in dev
    - Easily validating settings
- Cancellation support, in some manner
    - Interop with the interrupt signal from CLI
- Structured logger
    - File sink
    - Ideally, an asyncronous logger
- Prebuilt answers to as many other checklist items as possible, particularly
for the app types targeting
- How's the ecosystem?

## Tooling

- Build tool
- LSP and IDE
    - Project-wide diagnostics
    - Decompiler especially
    - Formatter
- Package manager
- Templating engine

### Relational DBs

- How does the language support getting multiple `select` results from a single
  sproc/func/etc call?
- How does the language send table-value parameters or composite arrays to a
target relational DB sproc?

