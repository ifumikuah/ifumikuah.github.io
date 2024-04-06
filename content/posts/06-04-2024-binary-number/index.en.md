---
title: "Base-2"
date: 2024-04-06
tags: [tech]
---

{{< katex >}}

Base-2 is a number system based on two numbers, 0 and 1. It's also called the **Binary** number system if you're studying computer science.

Binary is the basic fundamental if you start learning about computers. How is a computer identical to this binary number? Computers are man-made compute machines, and machines only recognize two conditions: **On** and **Off**.

These binary numbers represent those two states.

## Why is it called Base-2

Because it is based on 2 numbers, so is Octal which is also called Base-8, Hexadecimal is also called Base-16. even the Decimal number that you use everyday is also called Base-10.

Can we create our own number system? *The answer is yes, we can.

## Conversion to Binary (Base-2)

There's no need to explain Decimal because it's the system you use every day. Conversion to Binary involves the [modulo operation](../../mathematics/06-04-2024-modulus/), which is finding the remainder of the quotient to determine whether each unit gets \\(0\\) or \\(1\\).

For example, we will find the binary number of 1:

$$
  1_{10} = 1_2
$$

Since 1 is a binary number

$$
  3_{10} = n_2
$$

We only have 2 numbers, 0 and 1, so how do we represent 3 in binary?

The formula goes like this:

$$
  3\div 2 = 1
$$

If divided, the result is \\(1\\\), this is called **Quotient**. Save the quotient for later use.

We don't need the quotient to get the binary, we need the **Remainder**. For that we use modulo.

$$
  3 \space{mod}\space 2 = 1
$$

We get the first binary: \\(1\\). But you have to repeat this until  **the last quotient is zero**.

How do you do that? The previous quotient must be divided by \\(2\\) continuously until it runs out.

$$
  3 \space{mod}\space 2 = 1 \qquad{quot.} = 1\\\
  1 \space{mod}\space 2 = 1 \qquad{quot.} = 0
$$

The binary of \\(3_{10}\\) is \\(11_2\\)

Don't forget to record the remainder based on the smallest to largest quotient (from bottom to top).

## More Examples

$$
  15_{10} = n_2
$$

$$
  15 \space{mod}\space 2 = 1 \qquad{quot.} = 7\\\
  7 \space{mod}\space 2 = 1 \qquad{quot.} = 3\\\
  3 \space{mod}\space 2 = 1 \qquad{quot.} = 1\\\
  1 \space{mod}\space 2 = 1 \qquad{quot.} = 0\\\
$$

$$
  15_{10} = 1111_2
$$

Another one:

$$
  22_{10} = n_2
$$

$$
  22 \space{mod}\space 2 = 0 \qquad{quot.} = 11\\\
  11 \space{mod}\space 2 = 1 \qquad{quot.} = 5\\\
  5 \space{mod}\space 2 = 1 \qquad{quot.} = 2\\\
  2 \space{mod}\space 2 = 0 \qquad{quot.} = 1\\\
  1 \space{mod}\space 2 = 1 \qquad{quot.} = 0\\\
$$

$$
  22_{10} = 10110_2
$$

## Can we create ourselves a number system?

[Yes](../06-04-2024-base-5/).