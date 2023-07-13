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
In Python, `unittest` is a built-in library, and it supports the following concepts:
- Test case: a single unit of testing
- Test suite: a group of test cases that are executed together
- Test runner: a component that will handle the execution and the result of all the test cases

#### Assert Methods
`unittest` provides various assert methods to conduct testing:

    class MyTests(TestCase):
      def test_assertions(self):
        self.assertTrue(1 == 1)
        self.assertFalse(1 == 2)
        self.assertGreater(2, 1)
        self.assertLess(1, 2)
        self.assertIn(1, [1, 2])
        self.assertIsInstance(1, int)

#### Assert for Exceptions
Exceptions can be tested using `with` statement:

    class MyTests(TestCase):
      def test_exceptions(self):
        with self.assertRaises(ZeroDivisionError):
          1 / 0
        with self.assertRaises(TypeError):
          1 + '2'

#### Example
Example script to test:

<img src=https://github.com/TravisH0301/learning/assets/46085656/fd504d1a-a467-41a3-a7c4-722f71b93035 width=400>

Example unit test script:

<img src=https://github.com/TravisH0301/learning/assets/46085656/e8ed0894-5f80-4c87-b66c-c4b4eb69b8ce width=700>

Running example test script:

<img src=https://github.com/TravisH0301/learning/assets/46085656/17ff4fec-ef00-4ec8-863d-c1693f021851 width=700>




