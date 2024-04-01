---
title: "Input dan Form Submission di JS II"
date: 2024-04-01
description: "Macam-macam input HTML dan submission form di JS, Part II"
tags: [teknologi, webdev]
---

Artikel ini adalah lanjutan dari [sebelumnya](../01-04-2024-input-dan-form-submission-js-i/). Sebelumnya kita membuat sebuah formulir yang sudah siap pakai untuk diproses dengan Javascript.

Maka sekarang saatnya untuk mengerjakan kode Javascriptnya.

Yang kita inginkan adalah menyimpan objek berisikan data yang barusan dimasukkan (*datum*) kedalam sebuah array, sekiranya seperti:

```plain
[{...datum}, {...datum}, {...datum}]
```

Pertama, Element `<form>` itu sendiri harus didapatkan, kita sendiri sudah memberi `id` pada element tersebut sebelumnya. Kemudian kita membutuhkan sebuah array kosong untuk menampun objeknya.

```js
const form = document.getElementById("form");
const data = [];
```

Kemudian kita perlu menambahkan `EventListener` berjenis `submit` terhadap formulir tersebut. Ini membuat formulir tersebut akan *trigger* sebuah `event` tepat saat tombol submit yang kita buat sebelumnya ditekan.

Kita akan menjalankan fungsi `submitForm` saat formulir di submit oleh pengisi.

```js
form.addEventListener("submit", submitForm);
```

Oke, sekarang kita fokus ke fungsi tersebut. Secara *default* event ini akan reload ulang halaman ketika di *trigger*, ini artinya semua form yang diisi akan dibaca namun halaman akan ter-*reset* seketika.

Kita tidak mau hal tersebut terjadi, maka gunakan `preventDefault()` untuk menghilangkan sifat default event ini.

```js
function submitForm(event) {
  event.preventDefault();
}
```

`FormData` memainkan peran penting dalam pemrosesan input ini, konstruktor `FormData()` menawarkan cara terbaik untuk mengubah formulir yang diinput menjadi sepasang key-value yang nantinya bisa disimpan dalam sebuah objek.

```js
function submitForm(event) {
  const formData = new FormData(form);

  event.preventDefault();
}
```

`formData` saat ini menyimpan pasangan key-value yang dapat di-*iterate* dengan sebuah perulangan (for-loop).

```js
function submitForm(event) {
  const formData = new FormData(form);

  for (const [key, val] of formData) {
    // Do smth with 'key' and 'val'
  }

  event.preventDefault();
}
```

Kita akan menyimpan pasangan key-value tersebut kedalam sebuah object. Lalu kita butuh object tersebut untuk dimasukkan kedalam array.

```js
function submitForm(event) {
  const formData = new FormData(form);
  let datum = {}

  for (const [key, val] of formData) {
    datum[key] = val
  }
  data.push(datum) //Masukkan kedalam array

  event.preventDefault();
}
```

Kode JS untuk membuat formulir menjadi sebuah objek sudah selesai.

Namun, jika kamu bingung bagaimana untuk benar-benar memastikannya, misal: log ke console atau cetak data dengan manipulasi DOM.

Kamu bisa menggunakan kode dibawah untuk log ke console:

```html
<main>
  <form id="form">
    <!-- forms -->
  </form>
  <!-- Buat tombol diluar form untuk log data yang tersimpan -->
  <button style="margin: 1rem 0;" id="refresh-data">Refresh Data</button>
</main>
```
```js
const refresh = document.getElementById("refresh-data");

refresh.addEventListener("click", () => {
  console.log(data);
});
```

Source code dari tutorial dapat diakses melalui [JSFiddle](https://www.jsfiddle.net):

{{< button href="https://jsfiddle.net/detrux/Lzo35ujy/2/" target="_blank" >}}
Basic
{{< /button >}}
{{< button href="https://jsfiddle.net/detrux/dcz92rta/3/" target="_blank" >}}
More Advanced Version
{{< /button >}}
