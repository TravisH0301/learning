# Recursion in Python
When function is called in Python, the interpreter creates a new local namespace for the variables defined within the function.
Even when two different functions or two identical functions are called concurrently, the variables with the same names won't collide, 
because they are in separate namespaces.

In Python, there is a limit to the recursion to limit memory consumption. And this limit can be viewed and changed using the following code.

    from sys import getrecursionlimit
    # view current recursion limit
    getrecursionlimit()
    >> 1000
    
    # set recursion limit to 2000
    setrecursionlimit(2000)
    getrecursionlimit()
    >> 2000
  
## Countdown example
    def countdown(n):
      print(n)
      if n > 0:  # Recursion will end when n becomes <= 0
        countdown(n - 1)

## Factorial example
- n! = 
  - 1 if n==0 or n==1
  - n x (n-1)! if n > 1

In Python:

    def factorial(n):
      # Check if factorial base is reached
      if n <= 1:  
        # Returns 1 and ends recursion
        return 1
      else:
        val = n x factorial(n-1)
        return val
        
Running factorial(4) will be:

1. Calls factorial() with 4
2. Calls factorial() with 3
3. Calls factorial() with 2
4. Calls factorial() with 1 -> last call as factorial base is reached
5. factorial(1) returns 1
6. factorial(2) returns 2 x factorial(1) = 2 x 1 = 2
7. factorial(3) returns 3 x factorial(2) = 3 x 2 = 6
8. factorial(4) returns 4 x factorial(3) = 4 x 6 = 24

Hence, factorial(4) returns 24. This is how all recursive calls <b>stack up</b>.<br>
Think of function calling itself recursively, and called functions returning results in backwards.<br>
Think it as going in while calling functions, and coming back with results.

## Count elements in nested list example
    names = [ 'a', [ 'b', 'c', [ 'd', 'e'] ] ]
    
 The number of the elements in the list can be counted using recursion.
 - Traverse the list and check if it's list or not
 - If an element, count it
 - If a list, then
   - Traverse the sublist and do the similar steps
   - Once all sublists are exhausted, go back up while adding up the counts

In Python:

    def count_all_names(list_):
      count = 0

      # Traverse the list and count elements
      """When sublist is met, recursively call the function to count 
      the elements within the sublist
      """
      for x in list_:  
        # Add 1 if element is not a sublist
        if type(x) != list:
          count += 1
        # Recursively count elements of the sublist if sublist
        elif type(x) == list:
          """Function is called to count the elements of the sublist.
          During recursion, when no more sublist is found, the function will
          return the count. And while coming back up from the recursion, the
          count will be kept adding up using "+=".
          """
          count += count_all_names(x)

      return count
      
 1. Call function() with main list
 2. Call function() with sublist
 3. Call function() with second sublist
 4. function(second sublist) returns 2
 5. fuction(sublist) returns 2 + function(second sublist) = 2 + 2 = 4
 6. function(main list) returns 1 + function(sublist) = 1 + 4 = 5

## Palindrome example
A word is a palindrome when its reverse is the identical. ex) Racecar = racecaR

Recursion can be used, but it will not be effective as using "return True if word == word\[::-1\]"<br>
This can be achieved by checking first and last letters, second first and second last letters, and so on
until last letter is reached.

The recursion function is built to recursively call its own function while reaching the end point. <br>
Then once end point is reached, the returned result is combined while winding backwards.

    def is_palindrome(word):
      # Define base case of recursion (end of recursion)
      if len(word) == 1:
        return True
      
      # Recursively check the first and last letters
      return (word[0] == word[-1]) & is_palindrome(word[1:-1])
      
1. Call func() for 'racecar'
2. Call func() for 'aceca'
3. Call func() for 'cec'
4. Call func() for 'e'
5. func('e') returns True
6. func('cec') returns True & func('e') = True & True = True
7. func('aceca') returns True & func('cec') = True & True = True
8. func('racecar') returns True & func('aceca') = True & True = True
           
## Quicksort example
Quicksort uses a divide-and-conquer algorithm, where a given list of numbers are divided using the pivot number recursively until the lists 
are not dividable anymore. Then the divided lists are combined to result in the sorted list.

The list will be divided recursively in the following manner:
- Sublist 1 with numbers less than pivot number
- Sublist 2 with numbers equal to pivot number
- Sublist 3 with numbers larger than pivot number

This is done until all sublists are not dividable. Then the sublists will be combined in "sublist 1 + sublist 2 + sublist 3" order 
while rewinding back from the recursion.

Note that in this example, pivot number is calculated by "median of (first number + last number + middle number)".

    from statistics import median
    
    def quicksort(numbers):
      # Define base case to end recursion
      if len(numbers) <= 1:
        return numbers
      
      # Calculate pivot number
      pivot = median(numbers[0] + numbers[-1] + numbers[len(numbers)//2])
      
      # Create sublists using pivot number
      sub_1, sub_2, sub_3 = (
        [n for n in numbers if n < pivot],
        [n for n in numbers if n == pivot],
        [n for n in numbers if n > pivot]
      )
      
      # Recursively call function for 1 & 3 sublists, and collect results backwards
      return (quicksort(sub_1) + sub_2 + quicksort(sub_3))
      



![image](https://user-images.githubusercontent.com/46085656/179007664-0dc25dbb-f53d-4c74-9dac-f028d919ceb5.png)

    





