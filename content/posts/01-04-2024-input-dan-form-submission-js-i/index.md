---
title: "Input dan Form Submission di JS I"
date: 2024-04-01
description: "Macam-macam input HTML dan submission form di JS"
tags: [teknologi, webdev]
---

Artikel ini berisikan tentang bagaimana cara membuat dan apa saja input di HTML dan bagaimana cara memprosesnya di JS supaya data tersebut dapat diproses lebih jauh lagi.

Input adalah element paling kompleks dan essensial dalam Web Development. Cara untuk membuat elemen `<input>` dasarnya adalah sebagai berikut.

```html
<input type="text" placeholder="type a key" />
```

## Membuat sebuah input

Jika kamu membuat element ini apa adanya seperti dibawah:

```html
<input/>
```

Maka akan sama dengan:

```html
<input type="text"/>
```

Selain `type="text"`, `<input>` juga tidak sebatas formulir dengan kotak seperti diatas. Ada banyak lagi, diantaranya:

|Type|Description|
|-|-|
|`type="text"` <sup>DEFAULT</sup>|Input text|
|`type="number"`|Input khusus angka|
|`type="color"`|Input warna|
|`type="date"`|Input tanggal|
|`type="range"`|Input slider yang menerima nilai numerik `min` sampai `max`|
|`type="checkbox"`|Input centang, biasanya digunakan untuk input *boolean*|
|`type="radio"`|Input pilihan berganda, hanya bisa memilih satu pilihan|

### Label

Agar orang yang mengisi benar-benar tau input yang mereka isi dimaksudkan untuk apa, kamu harus memberi sebuah element `<label>` sesuai dengan inputnya.

```html
<label for="fullname">Nama Lengkap</label>
<input type="text" id="fullname" />
```

Pada elemen label sendiri ada attribut `for` sebagai penyambung antara label dan inputnya. Untuk valuenya, kamu harus memberi value yang sama dengan `id` milik `<input>` agar mereka terhubung.

### Tombol submit

Jika kamu selesai dengan inputnya, kamu bisa submit dan cetak hasil inputan seperti dibawah:

```html
<body>
  <main>
    <input id="input-name"/>
    <button id="submit">Submit</button>

    <p id="out"></p>
  </main>
  <script>
    const submit = document.getElementById("submit")
    const inputName = document.getElementById("input-name")
    const out = document.getElementById("out")

    submit.addEventListener("click", () => {
    	out.innerText = inputName.value
    })
  </script>
</body>
```

### Form element

Best practices untuk formulir adalah dengan membungkus semua input yang ada dengan element `<form>`.

```html
<form>
  <label for="fullname">Nama Lengkap</label>
  <input type="text" id="fullname" />
  
  <label for="age">Umur</label>
  <input type="number" id="age" />
</form>
```

## Membuat sebuah formulir yang beik dan benar

Diatas memberikan basic yang baik untuk membuat sebuah formulir, namun membuatnya menjadi formulir yang fungsional akan dijelaskan disini.

Formulir yang fungsional adalah formulir yang dapat berinteraksi dengan Javascript sebelum diproses oleh back-end. Ini artinya formulir tersebut harus *compliant* dengan aturan main Javascript. 

Salah satunya adalah dengan memberikan attribut `id` agar bisa diidentifikasi oleh DOM:

```html
<form id="form-i">
  <label for="fullname">Nama Lengkap</label>
  <input type="text" id="fullname" />
  
  <label for="age">Umur</label>
  <input type="number" id="age" />
</form>
```

Sesudah itu, kita ingin agar formulir tersebut pastinya dibaca oleh Javascript sebagaimana lembaran formulir yang seudah diisi. Yaitu kita ingin Javascript nantinya memproses data tersebut menjadi sebuah object.

Namun, Javascript tidak bisa menangkap *key* dari masing-masing *value* yang sudah kita masukkan. Kita sudah memiliki *value* dari input yang kita ketik, namun *key* harus didapatkan dengan memberi attribut `name` pada masing-masing `<input>`.

```html
<form id="form-i">
  <label for="fullname">Nama Lengkap</label>
  <input type="text" id="fullname" name="fullname" />
  
  <label for="age">Umur</label>
  <input type="number" id="age" name="age" />
</form>
```

**Satu lagi!**, diperlukan *Submit Button* untuk tombol mengirimkan formulir yang sudah diisi. Sekalian kita akan merapikan (secara tampilan) formulirnya:

```html
<form id="form-i">
  <div class="forms-row">
    <label for="fullname">Nama Lengkap</label>
    <input type="text" id="fullname" name="fullname" required />

    <label for="age">Umur</label>
    <input type="number" id="age" name="age" required />
  <div>
  <button type="submit">Submit</button>
</form>
```

{{< alert  "code" >}}
  Kamu bisa menambahkan attribut `required` untuk memastikan input tersebut wajib diisi.
{{< /alert >}}

## Membuat ini fungsional di JS

Akan dijelaskan dalam artikel [selanjutnya](../01-04-2024-input-dan-form-submission-js-ii/).