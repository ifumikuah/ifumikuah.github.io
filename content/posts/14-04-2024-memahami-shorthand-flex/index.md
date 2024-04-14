---
title: "Memahami shorthand flex di CSS"
date: 2024-04-14
description: "Penjelasan ringkas tentang flex-grow, flex-shrink, dan flex-basis"
tags: [teknologi, webdev]
---

{{< katex >}}

Flexbox adalah layout 2 dimensi CSS yang sering digunakan untuk kebutuhan responsive screen. Setiap item di flex memiliki properti `flex` nya sendiri, bervalue: `0 1 auto`

`flex` shorthand dari:

```plain
flex: [flex-grow] [flex-shrink] [flex-basis]
```

`flex: 0 1 auto` sama artinya dengan:

```css
.class {
  flex-grow: 0;
  flex-shrink: 1;
  flex-basis: auto;
}
```

## It's all about proportion

Untuk memahami `flex-grow` dan `flex-shrink`, kamu hanya perlu bermain-main proporsi.

*misal*:

Ada 5 buah item, masing-masing memiliki `flex-grow: 1`.

![img01](./img/img01.png "Lima buah item")

Ini artinya masing-masing item memiliki ukuran \\(\dfrac{1}{5} \\) dari ukuran container.

**Bagaimana jika salah satunya mendapatkan `flex-grow: 3`**?

![img02](./img/img02.png "C mendapatkan `flex-grow: 3`")

Ini artinya item selain C masih mendapatkan 1, sedangkan item C mendapatkan 3.

Membuat \\(1+1+3+1+1 = 7\\), artinya A, B, D, E memiliki ukuran \\(\dfrac{1}{7}\\), sedangkan C memiliki \\(\dfrac{3}{7}\\) **dari ukuran kontainer**.

## Grow and shrink

Ingat kalau ukuran item juga ditentukan oleh ukuran dari containernya, maka `flex-grow` dan `flex-shrink` meregulasi bagaimana itu terjadi.

- `flex-grow` akan meregulasi ukuran item **jika ukuran kontainer lebih besar daripada ukuran semua itemnya dikombinasikan**.

- `flex-shrink` akan meregulasi ukuran item **jika ukuran kontainer lebih kecil daripada ukuran semua itemnya dikombinasikan**.

### Value 0

Initial `flex-grow` adalah `0`, ini artinya ukuran item tidak pernah berubah sekalipun container dibesarkan atau dikecilkan. Ini juga berlaku untuk `flex-shrink`.

## Flex basis

`flex-basis` adalah size initial dari item. Jika `flex-grow` atau `flex-shrink` diberi value `0`, maka ukuran item akan memakai value yang dimiliki `flex-basis`.