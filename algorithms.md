# Algorithms

Here's a collection of ideas to help parse and solve problems.

## Grokking Problems

Do (brute force) samples, examples, and tests by hand, aloud, until you're basically saying the
pseudocode.

- How would you solve this as a human?
- Draw a picture!
- What are the calculations, deltas, meta data, etc that you perform?
- Can you draw any parallels to more common problems?
- Try a state diagram
- Try pattern matching
- Try recursion, if the problem hasn't already jumped out as a recursion-friendly problem.

## Writing Algorithms

- Pseudocode! Especially if using a less expressive language like Go.
- Will sorting help me?
- Will classical data structure(s) help me break this problem up?
- If stuck on a step, consider the current type and the desired type
- Can I represent the state in a non-standard form for make the problem's data easier to sort,
store in a more classical structure, employ dynamic programming, etc?
    - A good data structure can perform a ton of heavy lifting
    - More explicitly persisting data (line the index of a char/substring in a matrix) can really
    help too
- Can dynamic programming or adjacent techniques help here?
    - Ex: using a list of name-value pairs to implement fizzbuzz
- Can I divide the problem space in any way?
- If regex is helpful, also recall that regex libs can also be used to parse out values
- Try going back to definitions and be mindful of preconditions and assumptions

## Alternate Lenses

When in doubt, here are some ideas to start throwing at the wall

### Common Algorithms

- Sorting
- Divide/Decrease and conquer
- Depth-/breadth-first search
- Recursion
- Dynamic Programming

### Data Structures

- List (array, linked, recursive)
- Stack
- Queue
- Graph/Matrix
- Tree
- Trei

