# When to test
First, let's break down methods into four different categories.
A post on stack overflow sums the different types of methods very accurately. http://stackoverflow.com/questions/16043819/junit-testing-void-methods
>If your method has no side effects, and doesn't return anything, then it's not doing anything.

>If your method does some computation and returns the result of that computation, you can obviously enough assert that the result returned is correct.

>If your code doesn't return anything but does have side effects, you can call the code and then assert that the correct side effects have happened. What the side effects are will determine how you do the checks.

This covers 3 of the four diffenet types of methods. I would add that a third type
is a method that both returns results of a computation and has side effects.
We would do well to stay away from this fourth type of method. We also hopefully 
never write a method that does nothing, which leaves us with the middle two.

## Methods that return calculation results
These methods are the easiest to test. Capture all the possible and valid inputs to the function individually as separate tests.
For each test a deterministic outcome can be expected. Write the unit tests to cover all these cases. Remember to keep it DRY!

## Methods that have side effects
The side effects of these methods should *never* be side effects on the parameters passed to them. What will we affect then?
There are two possibilites.
* Class member variables (fields)
* Class dependencies

### How to test class member variable changes
This is easy if the member is exposed as a getter. Just get the value and make sure it is as you expect.

It becomes more difficult when there is no getter exposing the state of the member. If one encounters this state
one easy solution is to change the accessibility of the member to expose it to the unit test. In many circumstances
this is acceptable, but it is not the ideal solution. A better solution is to find a publicly accessible method that does expose
information about the state that you expect to change and test this instead.

If there is a member variable that is not exposed via a setter or any other public method, reasses why you are trying to test this
state. If you really do need to test the state maybe it is time for a refactor! (Are you following the Single Responsibility Priciple?)
