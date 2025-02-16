# Coding Challenge Interviews

Coding challenge interviews (like LeetCode and HackerRank) are such a unique combination of factors
that they warrant their own checklist (independent of an algorithms checklist) becauser Sawyer will
get frazzled and develop idiot brain.

1. Just before the interview, warm up with some challenge questions, ideally using the same platform
   as the interview.
1. The time constraint will frazzle you, take deep breathes and go nice 'n' slow to keep your
   composure and brain.
1. Fully read the question, preconditions, and examples fully.
1. Do (brute force) samples, examples, and tests by hand, aloud, until you're basically saying the
   pseudocode.
    - If you're with a human interviewer, confirm your comprehension and describe your algorithm.
    Ask if they want to see this or another, non-brute force algorithm, first.
1. If using a coding interview website, don't even bother with their editor and test suite, they
   always suck for developing and proving correctness.
1. Employ Test-Driven Development to write the test suite, starting with the given samples and
   tests, paying special mind to edge cases and assertions.
1. Write the pseudocode and then the algorithm (see [algorithms.md](./algorithms.md)).
1. Once the tests are green, copy the algorithm to the test site and run those tests.
    - If they complain about efficiency but otherwise are green, refactor.

