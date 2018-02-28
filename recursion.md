# Recursion Review

## What is Recursion?

Recursion is simply a function that calls itself. There is no magic occuring, contrary to popular belief, but it's often very difficult to understand what is going on 'under the hood'. When a function calls itself recursively try to imagine this GIF:

![](https://lh6.googleusercontent.com/-BOYdZI6tT7Y/UJwzRKYdQNI/AAAAAAAC5js/Ltg-gd6SCQQ/photo.jpg)

We see the elevator doors openening to reveal other elevator doors until finally one opens that the gentleman can enter. Once he is inside the doors begin to close one by one until it looks normal again. This is one of my favorite ways to imagine recursion. I will be continually referencing this GIF as we go through the rest of this article to try and explain how recursion is working.

## Two Key Components of a Recursive Function

### Recursive Case

This is the case that we want to continually run until we hit our base case/escape case. Imagine all the elevator doors opening in the GIF. When one door opens a slightly smaller one keeps appearing. Think of this process as getting closer to our base case. We begin far away from the base case but as our recursion runs we get slightly closer each time until finally we hit the base case.

### Base Case/Escape Case

This is the point at which we want our recursion to terminate. I like to think of the base case as the if statement in an if/else block. If a specific condition (the breaking point) is met we want to terminate our function, we want to break/escape out. This occurs when that last door in the GIF opens and the guy can crawl in. If we don't include a base case our function will run recursively forever, an infinite amount of doors will continue opening, and we will get an error.

There are times when a recursive function can have two base cases so try not to think of recursive functions as having just one base case. The base case is just the point at which we want to get out of the recursive loop.

## Example of Recursion

Let's check out some code.

```js

function countdownToOne(num) {
  //Base Case
  if (num === 1) {
    console.log(num)
  } else {
  //Recursive Case
    console.log(num)
    return countdownToOne(num - 1)
  }
}

```

Cool stuff!

This is a simple countdown function that takes a number and will recursively countdown & print all numbers from ```num``` to 1. So if we give this function ```5``` we will get ```5 4 3 2 1``` as our output. Our base case in this scenario is ```1```. If ```num``` is ```1``` we want to simply ```console.log(num)``` and escape out of the function. Our recursive case occurs everytime ```num``` **is not** ```1```. Inside our recursive case we first want to ```console.log``` the number being passed into the function. Then we want to recursively call our countdown function again on ```num - 1```. This will cause our function to run again and again and again slowly decrementing toward our base case. We know we will hit the base case eventually because after a while ```num - 1``` will eventually be evaluate to ```1```.

Here is a nice way to imagine recursion happening:

![Function Diagram](https://i.imgur.com/iUXlI48.png)

We see that once we hit the recursive case we open a new recursive call. In this new call we begin at the top of the function again with ```num - 1``` as the new argument. If we had no base case this process would repeat over and over and over again in an infinite loop (which will be discussed below in a little more detail).

## Call Stack

One of the most confusing aspects of recursion I ran into is trying to understand what is going on behind the scenes while the recursive function is running. This invovles the call stack.

![Recursion Diagram](https://i.imgur.com/cRjhcCv.png)

As we can see in the above diagram we start with ```countdown(5)``` so we skip the ```if``` statement base case and head right to the recursive call. We then ```console.log(5)``` and ```return countdown(5 - 1)```. A very interesting thing occurs at this point. Our original function call ```countdown(5)``` pauses because it is awaiting the return of ```countdown(5 - 1)```. It cannot finish evaluating until it receives this value. What happens is a new function call, ```countdown(4)```, is created and placed on top of ```countdown(5)```. This process occurs each time we recursively call our function until we hit the base case of ```countdown(1)```.

Once our base case is recursively called and placed on top of the callstack we stop adding to the stack. The functions on the stack can now finish evaluating and then be popped off. Since a stack is LIFO (last in, first out) first we begin by returning the value of ```countdown(1)``` and then ```countdown(2)``` and then ```countdown(3)``` and so on until we clear our callstack.

This leads to another interesting point. If we are clearing the callstack in this manner (starting with 1 and ending with 5) shouldn't we get 1,2,3,4,5 as our output?

We actually don't get this output and this has to do with the pausing of our functions we talked about earlier. Remeber when we said a function does not finish evaluating because a new function call is placed on top of it in the call stack? This process also impacts the output result. When ```countdown(1)``` is finished processing it is popped off of the stack but the value isn't immediately logged because it is needed to evaluate the past function calls. This seems weird using the above example but let's imagine the following scenario.

Below is a function that will recursively calculate the power of a number and an exponent it is passed.

```js
function calcPower(baseNumber, exponent) {
  if (exponent == 0)
    return 1;
  else
    return baseNumber * calcPower(baseNumber, exponent - 1);
}
```

Assume we call the ```calcPower(5, 3)```, the call stack would look like this:

```js
5 * (5 * (5 * 1))
```

Another way to think of it:

```js
calcPower(5,3)      calculatePower(5,2);    calculatePower(5,1);
      5         *         (5             *        (5 * 1))
```

The left most side of the above line of code is the bottom of the call stack and the right most side is the top. So, the first call to be processed and popped off of the stack would be ```5 * 1``` but this value is needed to calculate the value of the previous paused recursive call and so on and so forth. Eventually we will get:

```js
5 * (5 * (5 * 1))
5 * (5 * 5)
5 * 25
125
```

## Common Pitfalls of Recursion

1. Not returning the recursive function call
  - If the recursive call is not ```return```ed it won't execute
2. Over thinking what is going on
  - I know this sounds crazy but the more you try to imagine what is going on at each step in the recursive call the more you will drive yourself crazy. Just try to come up with a base case and then the next simplest answer for what would happen in the recursive case. Given the countdown function use 1 as the base case and 2 as the recursive case. If it works for these two cases it most likely will continue to work for more advanced cases.
3. Not practicing
  - Recursion is tough! Keep practicing recursive problems. A lot (maybe all) problems that use an iterative approach can be solved with recursion so go on [CodeWars](www.codewars.com) and get practicing!
4. ```Maximum call stack size exceeded```
  - This is commonly seen when there is an error with our base case. We get this error because we keep stacking unfinished function calls on top of each other creating a never ending and ever growing call stack with no end in sight. We need the base case to prevent this. (See the callstack diagram above)
