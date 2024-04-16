# Karatsuba's Algorithm

## Introduction
Let's think about "how fast" we can multiply two numbers together. In elementary school, you learned how to multiply two numbers (by putting one number above another, and multiplying one number at a time).

### Example
$$
\begin{array}{r}
    \; 12 \\
    \times \; 34 \\
    \hline
    \; 48 \\
    360 \\
    \hline
    408
\end{array}
$$

In the above example, we can notice that we need *four* single digit multiplications (we will call them SDMs) in order to solve. We also needed a little bit of addition to bring everything together, but that part doesn't really matter in this context. For our purposes, all we need to conceptually think about is the number of SDMs that are performed when multiplying two numbers.
\
\
We can also observe that for two $n$ digit numbers, we need $n^2$ SDMs using traditional vertical multiplication (schoolbook multiplication, as Justin calls it).

## Let's Try to Figure Out Some Stuff Now
### Example
Suppose we have two double digit numbers, $A$ and $B$. $A$ consists of the digits $a_1$ and $a_0$. By the same regard, $B$ with $b_1$ and $b_0$.
\
\
Suppose $A$ = $a_1a_0$ and $B$ = $b_1b_0$.
\
If $A$ = 42, then $a_1$ is 4 and $a_0$ is 2.
\
If $B$ = 87, then $b_1$ is 8 and $b_0$ is 7.
\
\
We can actually represent 87 as $10(8) + 1(7)$ since 8 is in the ten's place and 7 is in the one's place. More generally, we can say:

$$A = 10(a_1) + 1(a_0)$$
$$B = 10(b_1) + 1(b_0)$$

So let's put this all together:

$$AB = \left [ 10(a_1) + 1(a_0) \right ]\left [ 10(b_1) + 1(b_0) \right ]$$
$$AB = 100(a_1b_1) + 10(a_1b_0 + a_0b_1) + 1(a_0b_0)$$

Great! But we can optimize this further. Let's consider this partially unrelated equality below:

$$(a_1 + a_0)(b_1 + b_0) = a_1b_1 + a_1b_0 + a_0b_1 + a_0b_0$$

So now let's look at the equality from earlier:

$$AB = 100(a_1b_1) + 10(a_1b_0 + a_0b_1) + 1(a_0b_0)$$

More specifically the $(a_1b_0 + a_0b_1)$ part of it. We can actually rewrite this part as such:

$$(a_1 + a_0)(b_1 + b_0) - a_0b_0 - a_1b_1$$

Now:

$$AB = 100(a_1b_1) + 10[(a_1 + a_0)(b_1 + b_0) - a_0b_0 - a_1b_1] + 1(a_0b_0)$$

Observe that we have $a_1b_1$, $(a_1 + a_0)(b_1 + b_0)$, $a_0b_0$, another $a_1b_1$, and another $a_0b_0$. Without the duplicates, we have $a_1b_1$, $(a_1 + a_0)(b_1 + b_0)$, and $a_0b_0$.
\
\
Notice how that's *three* SDMs,  rather than the four we had earlier for two double digit multiplication! Before we go any further, let's define something.

### Mini Definition
A *decimal shift*, for our purposes, can be described using the example from above. In this equality:

$$AB = 100(a_1b_1) + 10[(a_1 + a_0)(b_1 + b_0) - a_0b_0 - a_1b_1] + 1(a_0b_0)$$

We see that we're multiplying by 100. Well, when you're multiplying something by 100, you just add two zeros at the end of the number. In the same way above, we can evaluate the inside of the parentheses and then simply add two zeros to the end. This would constitute *two decimal shifts*.
\
\
Now, for the 10 term, we know that multiplying something by 10 just adds a zero at the end. In the above example, we evaluate the inside of the parentheses, then we add a zero. This constitutes *one decimal shift*.
\
\
In total, we have *three decimal shifts*.

## Constructing the Algorithm
### Preliminary Notes
- Adding two $k$-digit numbers is $\Theta(k)$
- A singular decimal shift is $\Theta(1)$

Now, from the example in the previous section, we can count the number of additions and subtractions in the equality. Let's take a look at what I mean:

$$AB = 100(a_1b_1) + 10[(a_1 + a_0)(b_1 + b_0) - a_0b_0 - a_1b_1] + 1(a_0b_0)$$

In this example, there are *six* additions/subtractions, *three* SDMs, and we are manipulating numbers of at most $2n$ digits in general, and $n + \frac{n}{2}$ decimal shifts, so $\Theta(n)$.
\
\
We can put this all together as such:

$$3T(n/2) + \Theta(n)$$

Using the Master Theorem (whose steps I will skip), we get:

$$T(n) = \Theta(n^{\lg 3}) = \Theta(n^{1.58...})$$

We can safely conclude that Karatsuba's Algorithm takes less time than regular "schoolbook" multiplication -- since that's $\Theta(n^{2})$.