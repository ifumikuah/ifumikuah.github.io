---
title: "Base-2"
date: 2024-04-06
tags: [teknologi]
---

{{< katex >}}

Base-2 adalah sistem angka berbasis dua angka, hanya 0 dan 1. Sistem ini disebut juga sistem bilangan **Biner** jika kamu mempelajari ilmu komputer.

Biner adalah basic fundamental jika kamu mulai mempelajari tentang komputer. Bagaimana komputer identik dengan bilangan biner ini?, Komputer adalah mesin kalkulasi buatan manusia, dan yang namanya mesin hanya mengenal dua kondisi: **Hidup** dan **Mati**.

Bilangan biner inilah yang merepresentasikan dua keadaan tersebut.

## Kenapa disebut Base-2

Karena didasarkan oleh 2 angka, begitu juga bilangan Oktal yang disebut juga Base-8, Hexadecimal disebut juga Base-16. bahkan bilangan Desimal yang kamu pakai sehari-hari disebut juga dengan Base-10.

Apakah kita bisa menciptakan sistem bilangan sendiri? *Jawabannya tentu bisa*.

## Konversi ke Biner (Base-2)

Tidak perlu lagi menjelaskan Desimal karena sistem tersebut kamu pakai setiap hari. Konversi ke Biner melibatkan [operasi modulo](../../mathematics/06-04-2024-modulus/), yaitu mencari sisa bagi untuk menentukan apakah setiap satuan mendapatkan \\(0\\) atau \\(1\\).

Sebagai contoh, kita akan mencari angka biner dari 1:

$$
  1_{10} = 1_2
$$

Karena 1 termasuk dalam bilangan biner

$$
  3_{10} = n_2
$$

Kita hanya memiliki 2 angka, yaitu 0 dan 1, lantas bagaimana cara nya merepresentasikan 3 di biner?

Rumusnya adalah seperti ini:

$$
  3 \div 2 = 1
$$

Jika dibagi, hasilnya adalah \\(1\\), ini namanya **Quotient**. Simpan quotient tersebut untuk dipakai pada tahap selanjutnya.

Kita tidak membutuh kan quotient untuk mendapatkan binernya, yang dibutuhkan adalah **Remainder**, sisa baginya. Untuk itu kita menggunakan modulo

$$
  3 \space{mod}\space 2 = 1
$$

Kita mendpatkan biner pertama: \\(1\\). Namun kamu harus mengulangi hal tersebut sampai \\(3\\) tersebut habis dibagi, artinya: **Quotient terakhir harus nol**.

Bagaimana caranya?, quotient sebelumnya tadi harus dibagi \\(2\\) terus menerus sampai habis.

$$
  3 \space{mod}\space 2 = 1 \qquad{quot.} = 1\\\
  1 \space{mod}\space 2 = 1 \qquad{quot.} = 0
$$

Biner dari \\(3_{10}\\) adalah \\(11_2\\)

Jangan lupa untuk mencatat remainder berdasarkan quotient yang terkecil ke terbesar (dari bawah ke atas).

## Contoh Lainnya

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

## Apakah kita bisa membuat sistem bilangan sendiri?

[Ya](../06-04-2024-base-5/).