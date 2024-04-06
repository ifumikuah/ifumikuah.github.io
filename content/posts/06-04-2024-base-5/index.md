---
title: "Membuat sistem bilangan custom: Base-5"
date: 2024-04-06
tags: [teknologi]
---

{{< katex >}}

Setelah mempelajari [Sistem Biner](../06-04-2024-binary-number/), kamu mungkin penasaran bagaimana caranya mebuat sistem bilangan sendiri?.

Disini kita akan membuat sebuah sistem bilangan 5 angka, Base-5. Kita sebut sistem bilangan ini **Pental Number**.

## Rules

Peraturannya mirip seperti biner, alih-alih dua angka (0 dan 1), kita menggunakan 0 sampai 4 sebagai angkanya.

```plain
0, 1, 2, 3, 4 = Base-5
```

## Konversi

Kita juga akan menggunakan modulo, tapi \\(5\\) sebagai *divisor*.

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

Contoh lagi:

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

## Konversi Desimal ke Pental

Untuk konversi ke desimal, kita harus perhatikan ada berapa digit dari \\(n_5\\).

Contoh \\(140_5\\), ada 3 digit:

Maka yang dilakukan adalah menambahkan \\({dn}*5^d\\) di setiap digitnya. Dimulai dari angka paling kanan adalah ditambah \\(5^0\\)

$$
  (1\ast5^2) + (4\ast5^1) + (0\ast5^0) \\\
  25 + 20 + 0 = 45
$$

Contoh lain:

$$
  1143_5 = n_{10}
$$

$$
  (1\ast5^3) + (1\ast5^2) + (4\ast5^1) + (3\ast5^0)\\\
  125 + 25 + 20 + 3 = 173
$$