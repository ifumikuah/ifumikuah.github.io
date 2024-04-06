---
title: "Making you own custom number system: Base-5"
date: 2024-04-06
tags: [tech]
---

{{< katex >}}

After learning about [Binary System](../06-04-2024-binary-number/), you might be wondering how to create your own number system.

Here we will create a 5-digit number system, Base-5. We will call this number system **Pental Number**.

## Rules

The rules are similar to binary, but instead of two numbers (0 and 1), we use 0 to 4 as the numbers.

```plain
0, 1, 2, 3, 4 = Base-5
```

## Conversion

We will also use modulo, but \\(5\\) as the *divisor*.

$$
  45_{10} = n_5
$$

$$
  45 \space{mod}\space 5 = 0 \qquad{quot.} = 9\\\
  9 \space{mod}\space 5 = 4 \qquad{quot.} = 1\\\
  1 \space{mod}\space 5 = 1 \qquad{quot.} = 0\\\
$$

$$
  45_{10} = 140_5
$$

Another example:

$$
  173_{10} = n_5
$$

$$
  173 \space{mod}\space 5 = 3 \qquad{quot.} = 34\\\
  34 \space{mod}\space 5 = 4 \qquad{quot.} = 6\\\
  6 \space{mod}\space 5 = 1 \qquad{quot.} = 1\\\
  1 \space{mod}\space 5 = 1 \qquad{quot.} = 0\\\
$$

$$
  173_{10} = 1143_5
$$

## Decimal to Pental Conversion

To convert to decimal, we must note how many digits there are in \\(n_5\\\).

For example \\(140_5\\), there are 3 digits:

Then what we do is add \\({dn}*5^d\\\) for each digit. Starting from the rightmost digit is to add \\(5^0\\)

$$
  (1\ast5^2) + (4\ast5^1) + (0\ast5^0) \\\\\
  25 + 20 + 0 = 45
$$

Another example:

$$
  1143_5 = n_{10}
$$

$$
  (1\ast5^3) + (1\ast5^2) + (4\ast5^1) + (3\ast5^0)\\\\
  125 + 25 + 20 + 3 = 173
$$