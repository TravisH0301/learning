# Time & Space complexity

## Time complexity
Time complexity is the time taken by each statement in the algorithm.<br>
Simple statement takes up 1 unit of time for time function. Ex) f(n) = 3 with 3 simple statements.

## Space complexity
Space complexity is the space taken by each variable in the algorithm.<br>
Simple variable takes up 1 word of space for space function. Ex) s(n) = 3 with 3 constant variables.

## Frequency count method
Frequency count method is an analysis method to identify time and space complexity.<br>
Ex) Time complexity in nested loops

    for(i=0; i<n; i++){              # n+1 units (as i iterates from 0 to n)
      for(j=0; j<n; j++){            # n (any statement within the first loop runs n times) * (n+1) (as j iterated from 0 to n)
        c[i,j] = 0;                  # n * n (first n from first loop and second n from second loop)
        for (k=0; k<n; k++){         # n * n * (n+1)
          c[i,j] = A[i,j] * B[k,j];  # n * n * n
        }
      }
    }
    The time complexity for this algorithm is the total sum of units.
    The time function becomes, f(n) = 2n^3 + 3n^2 + 2n + 1
    Hence, the time function is O(n^3) - order of n^3 
    
Ex) Space complexity in nested loops

    for(i=0; i<n; i++){              
      for(j=0; j<n; j++){            
        c[i,j] = 0;                  
        for (k=0; k<n; k++){         
          c[i,j] = A[i,j] * B[k,j];  
        }
      }
    }
    In this algorithm, there are the following variables:
    - A = n^2 words (assuming it's nxn, 2-dimensional variable)
    - B = n^2
    - i = 1 (since it's constant variable - doesn't matter if it changes during iteration, it's still constant value)
    - j = 1
    - k = 1
    - n = 1
    The space function becomes, s(n) = 3n^2 + 4
    Hence, the space function is O(n^2) - order of n^2

### Examples with time complexity
When determining the order of time complexity, only the largest degree will be concerned.

Example 1.

    for(i=0; i<n; i=i+2){  # The time unit for this loop won't be cared anymore, since both n+1 and n will give out the order of n
      statement  # n/2 (Since i increases by 2, this reduces the time unit by 2 - as twice less iteration)
    }
    Hence, the time function is O(n)
    
Example 2.

    for(i=n; i>0; i--){
      statement # n (from 0 to n will be the same as from n to 0)
    }
    Hence, the time function is O(n)

Example 3.

    for(i=0; i<n; i++){
      for(j=0; j<i; j++){
        statement
      }
    }
    In this case, we need to identify the pattern by simulating each iteration of the loop.
|i|j|statement execution|no. of statement execution for each parent (i) iteration|
|--|--|--|--|
|0|0|No|0|
|1|0|Yes|1|
|1|1|No|1|
|2|0|Yes|1|
|2|1|Yes|2|
|2|2|No|2|
|3|0|Yes|1|
|3|1|Yes|2|
|3|2|Yes|3|
|3|3|No|3|

For each parent iteration, the statement run number is equivalent to the number of i.<br>
If this iteration happens for n times, the total statement execution time will be:<br>
0 + 1 + 2 + 3 + ... + n = n(n+1)/2 = (n^2+n)/2<br>
Hence, the time complexity is O(n^2)
