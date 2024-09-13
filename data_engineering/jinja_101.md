Jinja 101
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
    {{ words|sort|join(' ') }}

    Returns: "Hello World"

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

