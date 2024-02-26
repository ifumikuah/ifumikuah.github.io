---
title: "Menjinakkan gambar di CSS"
date: 2024-02-25
description: "Menjinakkan elemen yang berkaitan dengan gambar di CSS"
tags: [teknologi, webdev]
---

Bagaimana cara menghadapi gambarmu di CSS yang sulit sekali rasanya untuk dikendalikan, mulai dari proporsi tidak sesuai, posisi gambar tidak sesuai, svg dan lainnya.

## Object Fit

Object Fit mengatur bagaimana proporsi gambar ditampilkan jika melebihi/mengurangi kontainernya. Nilai default adalah `fill` dan yang dibahas disini hanya 4 sisanya selain `fill`. 

Value `object-fit` ada 5: `fill`, `contain`, `cover`, `scale-down`, dan `none`.

![Example](img/mbti-5.png "Gambar yang akan dipakai")

### Cover

Mempertahankan aspek ratio gambar, tetapi juga mengikuti width dan height dari containernya. Jadi jika gambar lebih besar daripada container maka gambar akan di crop selagi mempertahankan aspek rationya.

```html
<style>
  .kotak {
    margin: 0 auto;
    width: 500px;
    height: 300px;
    background-color: rgb(211, 232, 255);
    display: flex;
  }

  .image {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
</style>
<body>
  <div class="kotak">
    <img class="image" src="assets/mbti-5.png" alt="IMG">
  </div>
</body>
```

![Cover](img/img01.png "Gambar dicrop")

### Contain

Mempertahankan aspek ratio gambar, tetapi sekaligus membuat keseluruhan gambar akan tetap masuk didalam kontainer. Jadi bagaimanapun caranya container dibesar/kecilkan, gambar akan selalu proporsional dan didalam container semua kontennya.

```html
<style>
  .kotak {
    margin: 0 auto;
    /* Silahkan adjust ukuran sesukamu */
    width: 500px;
    height: 300px;
    /* Silahkan adjust ukuran sesukamu */
    background-color: rgb(211, 232, 255);
    /* flex membuat posisi gambar ditengah (vertikal) */
    display: flex;
    /* flex membuat posisi gambar ditengah (vertikal) */
  }

  .image {
    width: 100%;
    height: 100%;
    object-fit: contain;
  }
</style>
<body>
  <div class="kotak">
    <img class="image" src="assets/mbti-5.png" alt="IMG">
  </div>
</body>
```
![Contain1](img/img02.png "Contain")

![Contain1](img/img03.png "Gambar akan tetap didalam container bagaimanapun caranya")

### None

Mempertahankan aspek rasio gambar, tapi tidak mengubah ukuran gambar sama sekali. Ini membuat gambar tetap dalam ukuran aslinya.

Gambar asli berukuran 900px x 350px, ini membuat gambar terpotong jika container lebih kecil daripada ukuran gambar dan sebaliknya.

![none1](img/img04.png "Gambar lebih besar daripada container")

![none1](img/img05.png "Gambar lebih kecil daripada container")

### Scale Down

`scale-down` adalah gabungan dari `none` dan `contain`. Gambar akan memiliki sifat `contain` jika container lebih kecil daripada gambar, dan akan memiliki sifat `none` jika container lebih besar daripada gambar.

Ini artinya gambar akan di *scale down* jika container lebih kecil, namun tidak ada perubahan ukuran ketika container lebih besar daripada gambar.

## Object Position

`object-position` mengatur bagian gambar yang ingin di tampilkan/diekspos ketika gambar itu lebih besar daripada frame (dalam posisi cropped).

```css
.class {
  object-position: position
}
```

Nilai default `object-postion` adalah: `center center` atau `50% 50%`

### Nilai Persentase dan Satuan PX

Nilai 0 (titik awal) berada di atas kiri dari container, atau lebih tepatnya `object-postion: 0px 0%` sama dengan `object-postion: left top`

![position1](img/img06.png "`object-postion: left top`")

### Nilai eksplisit

Nilai eksplisit yang tersedia adalah: `left`, `center`, `right`, `top`, dan `bottom`. Self explanatory, dimana posisi ini bebas urutan kombinasinya.


![position1](img/img07.png "`object-postion: bottom right`")

![position1](img/img08.png "`object-postion: right`, tidak perlu `center` karena nilai default")