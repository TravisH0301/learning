# Jinja 101
Jinja is a python-based template engine that is widely used in web frameworks like Flask and Django.
It allows users to generate templates with dynamic contents like variables and loops.<br>
This is possible as Jinja reads and compiles the templates, holding both static content and dynamic placeholders,
by replacing placeholders and executing any control structures (e.g., loop) with real values and logic from the application.

dbt is a good use case of an application leveraging Jinja to enable dynamic, flexible and reusuable SQL.
Before dbt sends the SQL to the database, Jinja compiles the code by replacing the Jinja code with actual SQL.
And this documentation covers the basic syntax and features of Jinja that can be used in dbt.

*Note: Jinja templates can have any extension (e.g., .html, .jinja, .sql)

## Delimiters
Jinja has the following default delimiters:

- Statements: {% ... %}
- Expressions (prints output): {{ ... }}
- Comments: {# ... #}

    {# note: Commenting can comment out a part of the template
        {% for user in users %}
            ...
        {% endfor %}
    #}   

*Note: Commenting inside delimiters is not possible.

## Variables
Variables can be defined by using `set` statement, and can accessed by using expressions.
Jinja has its own predefined varialbes (e.g., loop), and applications may provide them too.
And variable's attributes/elements can be accessed using a dot or square brackets.

    {% set names = [{"class_a": ["Tom", "David"]}] %}
    {{ names[0].class_a[0] }}

    Returns: "Tom"

*Note: Both single and double quotations indicate a string in Jinja.<br>
*Note: When a variable or attribute does not exist, Jinja will return an undefined value, which can cause a silent failure.

## Filters
Filters can be used to apply transformations on variables.
Filters are separated from the variable by a pipe symbol (|) and may have optional arguments in parentheses.<br>
For Jinja's built-in filters, visit https://tedboy.github.io/jinja2/templ14.html

    {% set words = ["World", "Hello"] %}
    {{ words | sort | join(' ') }}

    Returns: "Hello World"

Filters can also be applied to a block of code

        {% filter upper %}
            this text becomes uppercase
        {% endfilter %}

        Returns: THIS TEXT BECOMES UPPERCASE

*Note: Multiple filters can be applied in series.

## Tests
Tests can be used to test a variable against a common expression. This is done using `if` statement with `is` followed by the test.
When the test condition is met, Jinja will return true (=True), else false (=False).<br>
For Jinja's built-in tests, visit https://tedboy.github.io/jinja2/templ15.html

    {% set nums = [1, 2, 3] %}
    {% if nums[0] is defined %}
        {{ "exists" }}
    {% endif %}

    Returns: "exists"

## Whitespace Control
Adding a dash symbol (-) at the start of end of the tag will remove all whitespaces before or after the tag respectively.

    hello {% if True %} world {% endif %}

    Returns: hello  world

    hello {%- if True -%} world {% endif %}

    Returns: helloworld

## Escaping
Using expression delimiter with quotations can output escaped strings.

        {{ '{{' }}

        Returns: '{{'

A block of code can also be escaped using `raw` statement.

        {% raw %}
            {% for item in items %}
                ...
            {% endfor %}
        {% endraw %}

        Returns: {% for item in items %} 
                     ...
                 {% endfor %}

## Control Structures
A control structure controls the flow of a program - conditionals (e.g., if), for-loops, as well as macros and blocks.
Control structures appear inside statement blocks.

### For
        {% for user in users %}
            {{ user.username }}
        {% endfor %}

Inside of a for-loop block, the following special variables can be accessed.

|variable|description|
|--|--|
loop.index|Current iteration of the loop (starts from 1).
loop.index0|Current iteration of the loop (starts from 0).
loop.first|True if first iteration.
loop.last|True if last iteration.
loop.length|The number of items in the sequence.
loop.previtem|Previously iterated item. Undefined during first iteration.
loop.nextitem|Next iterating item. Undefined during last iteration.

`else` can be used in case no iteration took place due to empty sequence or filtering removed all items.

        {% for user in users %}
            {{ user.username }}
        {% else %}
            no users found
        {% endfor %}

### If
`if` statement can be used to test if a variable is defined, not empty and not false.

        {% if users %}
            {% for user in users %}
                ...
            {% endfor %}
        {% endif %}

`elif` and `else` can be used like Python.

        {% if num > 0 %}
            positive number
        {% elif num < 0 %}
            negative number
        {% else %}
            zero
        {% endif %}

### Macros
Macros are comparable with functions in regular programming.

        {% macro introduce(name, age=20) %}
            Hi my name is {{ name }} and I am {{ age }} years old
        {% endmacro %}

        {{ introduce("David") }}

        Returns: Hi my name is David and I am 20 years old

*Note: macro arguments can be predefined with default values.

## Scoping Behaviour
Variables set inside a block are not accessible outside of it (like local and global variables in Python). <br>
`namespace` objects allow propagating changes across scopes (acts as a global variable in Python).

        {% set global_num = namespace(value=1) %}
        
        {% if true %}
            {% set global_num.value = 2 %}
        {% endif %}

        {{ global_num.value }}

        Returns: 2

## Block Assignments
Block assignments can capture the contents of a block into a variable. And they supports filters.

        {% set hello | upper %}
            hello world
        {% endset %}

        {{ hello }}

        Returns: HELLO WORLD

## Expressions
### Literals
Literals are the simplest form of expressions such as strings and numbers.

- String: Defined within single or double quotations.
- Integers
- Floating point numbers
- List: Defined within square brackets.
- Tuple: Defined within round brackers. Immutable.
- Dictionary: Defined within braces (curly brackets) with a key and value.
- Boolean: true (=True) or flase (=False).

### Math
- Addition: "+"
- Subtraction: "-"
- Division: "/"
- Floor: "//" Truncated division result (e.g., 20 // 7 = 2)
- Modulo: "%" Reminder of division result (e.g., 20 // 7 = 6)
- Multiplication: "*"
- Power: "**"

### Comparison
- Equal: "=="
- Not Equal: "!="
- Larger Than: ">"
- Larger or Equal to: ">="
- Smaller Than: "<"
- Smaller or Equal to: "<="

### Logic
- and: Returns true if left and right operands are both true
- or: Returns true if left of right operand is true
- not: Negates a statement (e.g., `is not`, `not in`)

### Other Operators
- in: Returns true if the left operand exists in the right operand sequence
- is: Performs `test`
- Pipe: "|" Applies a `filter`
- Tilde: "~" Convers all oeprans into strings and concatenates them (e.g., {{ "Hello " ~ name ~ "!"}} Returns: Hello John! (given name="John"))
- Callable: "()" Calls a callable (e.g., {% set words = ["hello", "world"] | join(' ') %} {{ words }} Returns: hello world)
- Access attribute of an object: "." or "[]"











