# Recursion Review

## What is Recursion?

Recursion is simply a function that calls itself. There is no magic occuring, contrary to popular belief, but it's often very difficult to understand what is going on 'under the hood'. When a function calls itself recursively try to imagine this GIF:

![](https://lh6.googleusercontent.com/-BOYdZI6tT7Y/UJwzRKYdQNI/AAAAAAAC5js/Ltg-gd6SCQQ/photo.jpg)

We see the elevator doors openening to reveal other elevator doors until finally one opens that the gentleman can enter. Once he is inside the doors begin to close one by one until it looks normal again. This is one of my favorite ways to imagine recursion. I will be continually referencing this GIF as we go through the rest of this article to try and explain how recursion is working.

## Two Key Components of a Recursive Function

### Recursive Case

This is the case that we want to continually run until we hit our base case/escape case. Imagine all the elevator doors opening in the GIF. When one door opens a slightly smaller one keeps appearing. Think of this process as getting closer to our base case. We begin far away from the base case but as our recursion runs we get slightly closer each time until finally we hit the base case.

### Base Case/Escape Case

This is the point at which we want our recursion to terminate. I like to think of the base case as the if statement in an if/else block. If a specific condition (the breaking point) is met we want to terminate our function, we want to break/escape out. This occurs when that last door in the GIF opens and the guy can crawl in. If we don't include a base case our function will run recursively forever and we will get a ```Maximum call stack size exceeded``` error.

There are times when a recursive function can have two base cases so try not to think of recursive functions as having just one base case (more on this below). The base case is just the point at which we want to get out of the recursive loop.

## Examples of Recursion

Let's check out some code.

```js

function countdownToOne(num) {
  //Base Case
  if (num === 1) {
    return console.log(num)
  } else {
  //Recursive Case
    console.log(num)
    return countdownToOne(num - 1)
  }
}

```

Cool, so this is a simple countdown function that takes a number and will recursively countdown & print all numbers from num to 1. So if we give this function 5 we will get 5, 4, 3, 2, 1. Our base case in this scenario is 1. If num is ```1``` we want to simply ```return console.log(num)``` and escape out of our function. Our recursive case occurs when num is not 1. We first want to console.log the number being passed into the function and then we want to call our countdown function again on one less than the number being passed in. This will cause our function to run again and again until we hit the base case.

![Diagram of Recursion](https://imgur.com/a/sCJhS)

## Common Pitfalls

1. Not returning the recursive function call
  a. If the recursive call is not ```return```ed it won't execute
