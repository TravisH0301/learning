# Test-Driven Development (TDD)
Test-driven development (TDD), a.k.a. red-green-refactor, is a software development approach in which tests are written before the code.
The process relies on the software requirements being converted to test cases. And development is tracked by repeatedly testing the software against all test cases. 

## TDD Workflow
1. Write a test: Unit test is written to test a small functionality of the codebase
2. Run the test: The test is expected to fail at this stage
3. Write the code: Simplest possible code is written that will pass the test
4. Run the test: After the code is written, test is ran to check if the functionality works
5. Refactor: Once the test is passed, the code is cleaned afterwards
6. Repeat from step 1

## Unit Test
Unit tests checks a single component of the software - a class, a function, or a method.
Being a low-level testing activity, it helps to detect bugs early and ensures code reliability.

Unit tests typically follows the Arrange, Act and Assert (AAA) pattern.
- Arrange phase: all the objects and variables needed for the test are set
- Act phase: the function/method/class under test is called
- Assert phase: outcome is verified

### Unit Test in Python
In Python `unittest` and `pytest` are two widely used testing frameworks.

#### unittest
unittest is a built-in library in Python and it supports the following concepts:
- Test case: a single unit of testing
- Test suite: a group of test cases that are executed together
- Test runner: a component that will handle the execution and the result of all the test cases


