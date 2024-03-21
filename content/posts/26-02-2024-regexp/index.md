---
title: "RegExp"
date: 2024-02-26
description: "Menggunakan regex untuk manipulasi string"
tags: [teknologi, webdev]
---

Apa itu RegExp? Regular Expression atau RegExp adalah cara untuk memanipulasi string, sintaks RegExp ini dipakai di berbagai bahasa mulai dari Python, JavaScript, Java, PHP, dan lainnya.

## Basic

```js
const regexConstructor = new RegExp(/b[aiueo]/, "gi")
const regex = /b[aiueo]/gi
```

Panduan dan Cheatsheet:

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Regular_expressions

- https://cheatography.com/davechild/cheat-sheets/regular-expressions/

- https://www.rexegg.com/regex-quickstart.html

Latihan: 

- https://www.codewars.com/collections/training-js-series-for-javascript-beginner-myjinxin2015

- https://regex101.com/

## Beberapa RegExp Penting

Dibawah adalah beberapa contoh match RegExp yang mungkin dibutuhkan untuk manipulasi string

### Match huruf pertama di setiap kata

```regexp
/(^|\s)[a-z]/gi
```

- Capturing group 1 (`(...)`): match awal dari string (`^`) atau (`|`) match whitespace (`\s`).
- Match dari huruf a sampai z (`[a-z]`).