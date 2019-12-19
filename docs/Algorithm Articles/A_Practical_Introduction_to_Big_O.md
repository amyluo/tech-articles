#A Practical Introduction to Big O
######Express how efficient an algorithm is without the mathematical jargon

Whenever the phrase Big O is thrown around, you might correlate it with an algorithms test that you would likely encounter at a larger tech company.

While this is true, it should not be used as a reason to completely skip over such a vital aspect of software development — how code runs.

In layman’s terms, we’ll talk about what Big O actually is, why you should care about it, and how you can go about evaluating what the Big O is for certain pieces of code.

-----

##What is Big O?

Big O is a concept that has come to be accepted in the programming world where we can express how efficient an algorithm is without having to go through the means of deriving complex mathematical expressions.

Let’s face it, not all developers eat, sleep, and breathe math.

You might be facing a question along the lines of: “Why can’t I just see how long it takes my local machine to process the code?”
Examining how fast it took your code to run on your machine is not the best determinant for how fast your code would run overall as different users will be in different environments.

Therefore, we will need a metric to describe how fast something runs, regardless of what environment it’s actually running in. This is what Big O is used for.

When speaking about Big O, we can refer to either the runtime complexity or the space complexity (memory) of code. Oh, and as a disclaimer, I will use the word complexity and efficiency interchangeably.

-----

##Why Care About Big O?

In a day-to-day scenario, it may be a little unrealistic to be obsessive about Big O complexities for the code you’re working on. Your work also might not require you to know such information.
However, understanding Big O allows you to determine the advantages and disadvantages of different approaches to solve a problem and allows for a dialog to be created with other developers to discuss such tradeoffs.
As you go on and examine different algorithms, you will see a time vs. space tradeoff.

-----

##Evaluating Big O

Here are some common Big O terminologies that you should be familiar with. These Big O terms are in order from fastest to slowest:

1.O(1) — constant

2.O(log n) — logarithmic

3.O(n) — linear

4.O(n log n) — n log n

5.O(n²) — quadratic

6.O(n³) — cubic

7.O(2^n) — exponential

8.O(n!) — factorial

![Big-O Complexity Chart](https://miro.medium.com/max/1642/1*06uXThuI9EaN8LnJvNWZWw.png)

[Big O Time complexity graph URL https://www.bigocheatsheet.com/](https://www.bigocheatsheet.com/)

Big O runtimes are heavily generalized. We typically default to using the variable `n` as the input that our algorithm receives. Although we will never know exactly what the input’s size is, we must assume that it is extremely large.

We take the most expensive set of computations around a block of code and revolve the efficiency of the algorithm based around that.

At the end of this article, we will be able to make an expression that includes the runtime for the majority of computations within a certain block of code.

Here’s an example expression of what we might see:

`O(n) + O(n^2) + O(1)`

In this expression, we take the runtime with the heaviest cost and call that the Big O runtime of the algorithm. In this imaginary example, the runtime is `O(n²)` as that computation is what will take the most time in the given expression.

But wait… what is `n` in this example? When specifying a runtime, it is just as important to clarify what `n` refers to. In this example, we can say something along the lines of: “The runtime would be `O(n²)` where `n` is the length of our input array”.

In layman’s terms, we can say that that Big O is a way to measure how something performs given an `N` input.

Another rule of thumb when dealing with Big O: we drop constants in our expression. For example, `O(n) + O(n) + O(n)` will turn into `O(3n)`, but we never actually claim our runtime to be `O(3n)`. Instead, we simply call it to be `O(n)`.

Now that we have a basic understanding of different complexities, let’s examine a few of the common runtimes to see how they might look in code.

-----

##O(1) Complexity

This refers to a constant operation. In other words, regardless of how large our input N gets, this operation will still do a fixed amount of work. Consider the following example:
```php
public function add (int $n):int {
    return $n + $n;
}
```
No matter how large or small our input is, we are still doing a fixed amount of work and taking up a fixed amount of memory. Therefore, this `add()` function will have a run and space complexity of `O(1)`.

-----

##O(n) Complexity

This complexity refers to a linear operation. So that, whatever `n` is defined as, our run or space complexity is directly proportional to `n`. Let’s take a look at an example.
```php
public function addArray(array $n): array {
    // Declaring a new array.
    $result = [];

    // we are iterating over each element within our $n array.
    foreach ($n as $nValue) { // O(n) operation
        $result[] = $nValue * 2; // O(1) operation
    }

    return $result;
}
```
The time and space complexity of this algorithm are both `O(n)` where `n` is the size of our input array.

Why? For space, we create a new array in memory that is directly proportional to n’s length. If we weren’t creating this array, we would have an `O(1)` space complexity.

For time, we iterate over `n` elements. As this is not a fixed amount (scales with the size of `n`), we say that it has an `O(n)` complexity as this for-loop is our heaviest computation.

-----

##O(n²) Complexity

`O(n²)` algorithms are most commonly seen in two nested for loops where we iterate over the same length. Consider the following example:
```php
public function badAddAlgorithm(array $n) {
    foreach($n as $i => $nValue1) {
        foreach($n as $j => $nValue2) {
            $result = $nValue1 + $nValue2;
            print($result);
        }
    }
}
```
To make it crystal clear, let’s take a simple example and define `$n = [1,2,3,4,5]`.

For each iteration in i, we iterate through the entire array in our nested for-loop. Such that, when `i = 0` and `j = 0`, `j` will have to iterate up to `count($n)-1` before `i` is incremented by 1.

This can also be expressed as `O(n*n)` runtime.

A good rule to follow when dissecting runtimes: Whenever we have a for-loop that iterates `n` times, any additional work within this for-loop is multiplied by `n`.

In the previous example, we saw `O(n*1)` work being done. If the code is not nested within a for-loop or something similar, we keep the runtimes of the sections separated with a `+` symbol.

Consider the following code:
```php
public function testFunction(int $n) {
    $doubleTotal = $n + $n;
    $multiplyTotal = 1;
    $addTotal = 0;

    for($i = 0; $i < $n; $i++) {
        $multiplyTotal *= $i;
    }

    for($i = 0; $i < $n; $i++) {
        $addTotal += $i;
    }

    return $multiplyTotal - $addTotal + $doubleTotal;
}
```
We could express this runtime as:

`O(n) + O(n)` which just simplifies to be `O(n)` — take note that we are not multiplying `n` by n as they are not nested loops!

-----

##O(log n) Complexity

This might seem a little confusing at first glance as I’m intentionally trying to avoid math. Let’s consider the following code:
```php
public function logFunction(int $n) {
    $i = $n;
    while($i > 0) {
        $i = $i / 2;
    }
}
```
We initially define a variable `i` to be equal to `n` (a constant operation).

While `i` is greater than zero, at each iteration we cut `i` by half. This is what a logarithmic runtime consists of. As shown by the chart, this is the closest we can get to a constant runtime.

-----

##Conclusion

Big O notation is quite an elaborate topic but I hope you now have a better understanding than before. We were able to see situations where constant, linear, quadratic, and logarithmic runtimes exist within code.

If you have any questions about the notation, feel free to ask.


------------------------------------------------------------------------
Reference to [A Practical Introduction to Big O](https://medium.com/better-programming/a-practical-introduction-to-big-o-a9f9c416aaaf)