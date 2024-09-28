# Python Style Guide
This note summarises recommended Python style guides from PEP 8, Google & Black.

Note that different style guides may have conflicts, and based on the personal perference, one may choose a style over the other or even mix them.
The main purpose of the style guide is to improve code ***readability*** and make it ***consistent***.

- [PEP 8](#pep-8)
  - [Indentation](#indentation)
    - [Hanging Indentation](#hanging-indentation)
  - [Maximum Line Length](#maximum-line-length)
  - [Break before Binary Operation](#break-before-binary-operation)
  - [Blank Lines](#blank-lines)
  - [Imports](#imports)
  - [String Quotes](#string-quotes)
  - [Whitespace in Expressions and Statements](#whitespace-in-expressions-and-statements)
  - [Trailing Commas](#trailing-commas)
  - [Comments](#comments)
  - [Naming Conventions](#naming-conventions)
  - [Type Annotation](#type-annotation)
  - [Other Recommendations](#other-recommendations)
- [Google](#google)
  - [Python Rules](#python-rules)
    - [Lint](#lint)
    - [Imports](#imports)


## PEP 8
### Indentation
4 spaces. However many IDE automatically converts a tab into 4 spaces. Hence, tab can be used as long as it results in 4 spaces.
Generally, tabs are not recommended as added length can be difference based on the application/platform.

#### Hanging Indentation
Hanging indentation refers to when the code continues onto the next line (e.g., continuation of a code on multiple lines), indentation is given
after the first line. For variables wrapped inside parenthesis, brackets and braces, they need to align vertically.

    # When first line contains an argument, next lines should align vertically to the first line argument
    foo = function_name(arg_1, arg_2,
                        arg_3, arg_4)

    # When first line contains no argument, indentation is given for the subsequent lines with vertical alignment
    foo = function_name(
        arg_1, arg_2,
        arg_3, arg_4
    )

    # Additional indent is recommended to distinguish arguments from the rest
    # **However, PEP 8 also accepts just a single indent after the first line
    def function(
        arg_1,
        arg_2,
        arg_3):
      print(arg_1)

Note that the close parenthesis can be located on the last line or on the next new line. If on the new line, it can either align vertically or one indent behind.

### Maximum Line Length
79 characters. Having line length limit is to prevent wrapping of the code, that can disrupt the readability.

Long lines can be wrapped in parenthesis. A backslash can also be used for line continuation, yet parenthesis is a preferred way.

### Break before Binary Operation
    # Example
    total = (
      var_a
      + var_b
      + var_c
    )

### Blank Lines
- Surround top-level function and class definitions with two blank lines
- Surround method definitions within a class with one blank line
- Use blank lines to indicate logical sections

### Imports
- Always on the top of the file after any comments and docstrings, and before module globals and constants
- Imports should be on separate lines unless importing from the same module (e.g., from datetime import datetime, timedelta)
- Imports should be grouped in the following order (separated by a blank line):
  - Standard library
  - Third party library
  - Local library
 - Avoid wildcard import (e.g., from <module> import *) as it makes it difficult for readers to understand where a specific class/function is from

### String Quotes
- Either single quotes or double quotes, as long as consistent
- Use double quotes for docstrings

### Whitespace in Expressions and Statements
- No whitespace after open parenthesis or before close parenthesis (also apply to bracket and brace)
- No whitespace between a trailing comma and a following close parenthesis (e.g., Correct example => foo = (0,)
- No whitespace before a comma, semicolon, colon or parenthesis
- For slice, equal amount of whitespace must be given on both sides (e.g., Correct example => list[1:3] or list[1 : 3]
- No trailing whitespaces
- One whitespace on both sides of binary operators (e.g., =, +=, <, !=, is, is not, and, or)
  - However, for a keyword argument of an **unannotated** function, No whitespace around the assignment operator (e.g., def test(var_1, var_2="abc"))
 
### Trailing Commas
- Mandatory for a tuple of one element (e.g., foo = (0,))
- Apart from above, it's optional. It can be useful to leave a trailing comma in at the end of the wrapped variables.<br>

      # Example when a trailing comma can be beneficial
      # When a new element is to be added in the future, only a single line is to be added.
      # If there was no trailing comma after "b", two lines would require amendments.
      list = [
        "a",
        "b",
      ]

### Comments
- Comments should be complete sentences
- The first word should be capitalised, unless it is an identifier that begins with a lower case
- Block comments = multiple lines of comments that start with "#" and a single space
- Inline comments are to be used sparingly as it can be distracting. If required, at least two spaces between the code and the inline comment.
- Documentation strings (=Docstrings) is required for all public modules, functions, classes and methods
  - One-line docstrings must close on the same line (e.g., """Hello World.""")
  - Multi-line docstrings can either start on the first line or second line. Closing should be done on a new line.
 
### Naming Conventions
- lowercase
- lower_case_with_underscores
- UPPERCASE
- UPPER_CASE_WITH_UNDERSCORES
- CapitalisedWords (a.k.a. CapWords ot CamelCase)
<br>

- _single_leading_underscore: weak internal use indicator
- __double_leading_underscores: internal use indicator - invokes name mangling (e.g., FooBar.__boo becomes FooBar._FooBar__boo)
- single_trailing_underscore_: used to avoid conflicts with Python keywords
- \_\_double_leading_and_trailing_underscores\_\_: magic objects or attributes in environment (e.g., \_\_init\_\_, \_\_import\_\_). Not to invest such names.
<br>

- Modules should have short & all lowercased (with underscores) names
- Classes should have CapWords/CamelCase names
- Type variables should have short names (e.g., T, Num)
- Exceptions should have CapWords/Camelcase names with "Error" suffix (e.g., ValueError)
- Function/Method and variables should have lowercase (with underscores)
  - Use "self" for the first argument of instance methods
  - Use "cls" for the first argument of class methods
- Constants should have UPPERCASE (with underscores) names

### Type Annotation
Type hints make Python code more explicit about the types of the variables/arguments.
Python is dynamically typed, where a varible type is not defined in the beginning.
By explicitly annotating the type, it can enhance maintainability and bug prevention (through type checker program).

Function can be annotated to give hints about the types of the arguments.

    # In function annotation, space is given after colon, around assignment operator (=) & arrow sign
    def greeting(name: str = "John") -> str:
      return "Hello" + name

Below shows examples of types that can be annotated.

- Standard types: int, str, float, list, None, etc.
- Types from `typing` module:
  - Any: any type
  - Union: any of the given types (e.g., Union[str, int] => can be string or integer)
  - List: list of elements with given types (e.g., List[str] => list of string elements)
  - Dict: dictionary of key & value with given types (e.g., Dict[str, int] => keys have str type & values have int type)
  - Optional: either None or a given type (e.g., Optional[str] => can be string or None)
  - `TypeVar`: In case when an output argument type is equal to an input argument type, `TypeVar` can be used.

        # Wrong example
        # Below function annotation doesn't mean that the output will have the same type as the input
        from typing import Union
    
        def test(val: Union[str, int]) -> Union[str, int]:
          ...

        # Correct example
        # Below function annotation indicates that the output will have the same type as the input,
        # where both of them can be either a string or integer.
        from typing import Typevar

        T = TypeVar("T", str, int)  # This TypeVar can be either string or integer
        def test(val: T) -> T:
          ...

### Other Recommendations
- Do not use compound statements (e.g., do_one(); do_two(); do_three)
- Comparison to singletons like "None" should be done with `is` or `is not`
- Use `is not` instead of `not is` for readability
- Use a def statement instead of a lambda expression directly to an identifier (e.g., Wrong example => f = lambda x: 2 * x)
- Derive exceptions from `Exception` rather than `BaseException`
- Use exception chaining appropriately. Explicit exception chaining (e.g., raise X from y) should be used to indicate chain of exceptions explicitly.
  - Implicit Exception Chaining: uses \_\_context\_\_ attribute of an exception to show exception chains

        def test():
          try:
            42/0
          except Exception as e:
            raise ValueError
        """Above code execution will return the following:
        Traceback ... :
        ...
        ZeroDivisionError: ...

        During handling of the above exception, another exception occurred:

        Traceback ... :
        ...
        ValueError

        **Ultimate result is ValueError, whose \_\_context\_\_ points to ZeroDivisionError,
        whose \_\_context\_\_ points to None.
        """

  - Explicit Exception Chaining: uses \_\_cause\_\_ attribute to show exception chains

        def test_2():
          try:
            42/0
          except Exception as e:
            raise ValueError from e

        """Above execution will return the following:

        Traceback ... :
        ...
        ZeroDivisionError: ...

        The above exception was the direct cause of the following exception:

        Traceback ... :
        ...
        ValueError

        **Both implicit and explicit exception chaining help linking different exceptions.
        However, the explicit exception chaining allows developers to intentionally specify
        the relationship between exceptions. And this can create more understandable code.
        """
- When catching exceptions, mention specific exceptions whenever possible instead of a bare `except:` caluse
- Limit `try-except` clause to the minimum amount of code, as it can ignore any unconsidered edge-case errors.
- Use `with` statement to clean up any resource local to a particular section of code
- Use explicit `return` statement in a function, even it returns None
- Use `"".startswith()` and `"".endswith()` instead of string slicing (e.g., if name.startswith("John"):)
- Use `isinstance` for type comparison (e.g., if isinstance(name, str):)
- For sequences, use the fact that emtpy sequences are false (e.g., `if name_list:` instead of `if len(name_list)`)
- Don't compare boolean values to True or False using `==` (e.g., `if is_valid:` instead of `if is_valid == True` or `if is_valid is True`)
- Don't use flow control statements (return, break, continue) within the finally suite of try...finally,
where the flow control statement would jump outside the finally suite.
This is because such statements will implictly cancel any active exception that is propagating through the finally suite.

## Google
### Python Rules
#### Lint
Linting provides a static analysis over the code to detect bugs and check compliance against PEP 8.<br>
Best practice is to use a style formatter (e.g., Black) followed by a linter like Flake 8 or Pylint (Flake 8 is lighter than Pylint).

#### Imports
Use `import` statements for packages* and modules only, not for individual types, classes or functions - except for common utility modules (e.g., from typing import List) or long module names to enhance readability.<br>
This is to explicitly define which namespace* the types, classes or functions are from to avoid confusion.

- Use `import x` for importing packages and modules
- Use `from x import y` where x is the package prefix (location or namespace) and y is the module name without prefix
- Use `from x import y as z` in to avoid name conflict with other modules or built-in names, when y is too long or too generic
- Use `import y as z` only when z is a standard abbreviation (e.g., import pandas as pd)

*Package: directory containing collection of sub-packages or modules (single .py file). This directory contains `__init__.py` to indicate it's a package.

*Namespace: container holding set of names (variables, functions, etc.) There are several namespopes in Python:
- Local: defined within a function
- Enclosing: defined within an enclosing function of nested functions<br>

      def outer_func():
          x = 5  # enclosing scope

          def inner_func():
              x = 2  # local scope
- Global: defined in the global level of modules or scripts, including imported packages/modules.
- Built-in: Python's built-in namespace (e.g., print())




