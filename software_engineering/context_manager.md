# Context Manager
Managing resources  is an important aspect while coding on Python. Without properly 
releasing the external resources such as files and connections, one might 
run into a memory leak issue. 

And even there is a closing or disconnecting call at the end of the code, they may not
be called when an error or an exception occurs.

In Python, one can use `try-finally` and `with` statements to prevent memory leaks. 
And `with` statement can provide a neat and reusable way of handling resources 
using context managers. 

## Examples
### `try-finally`
    file = open('info.txt', 'w')
    try: 
        file.write('Hello')
    finally:
        file.close()

### `with`
    with open('info.txt', 'w') as file:
        file.write('Hello')

## Building Context Manager 
`with` statement uses context managers to perform an opening and exiting actions.
Many libraries and built-in functions support context management, such as open(). 
And one can create customised context managers too. 

There are two ways to build context managers using `class` and `function`.

### Class based Context Manager
To implement a context manager in a class, one needs to add `__enter__` and `__exit__` 
methods. 

`__enter__` method handles the opening process, and `__exit__` method controls the closing
process. 

    class DatabaseConnection(object):
        def __enter__(self):
            self.conn = db.connect('database.db')
            return self.conn
        def __exit__(self, type, value, traceback):
            self.conn.close()
    
    with DatabaseConnection() as db_conn:
        cur = db_conn.cursor()
        cur.execute('SELECT column FROM table')
        con.commit()

In this code example, `__enter__` returns the connection to the target variable, `db_conn`.
And when the `with` statement executes successfully, `__exit__` will be called and the exception
parameters, `type`, `value` and `traceback` will become `None`.
However, when there is an exception, `__exit__` will still be called, and exception parameters will
be passed to the method. And this will be followed by an exception message in the output. 
Note that when `True` is returned in `__exit__` method, the exception will be suppressed.

### Function based Context Manager
Context managers can also be built with a generator function and a `contextlib.contextmanager` decorator.

    from contextlib import contextmanager

    @contextmanager
    def hello_name(name):
        try:
            print('What is your name?')
            yield name
            print('Hello', name)
        except:
            print('Error')
        finally:
            print('Goodbye', name)
        
    with hello_name('David') as my_name:
        print(my_name)

In this example, the decorator `contextmanager` is called with the generator function `hello_name` as
an argument. This wrapped generator is passed to the target variable `my_name`. 

Looking at the generator, the code before `yield` will be executed when the `with` statement is executed.
And the code after `yield` will be executed when the `with` statement has successfully executed. 
When an exception is raised, the code within `except` statement will be triggered. And lastly, the code
within `finally` statement will run regardless of whether the `with` statement ran successfully or not. 

