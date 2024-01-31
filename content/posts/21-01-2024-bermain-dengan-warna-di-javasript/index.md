---
title: "Webdev Project: Bermain dengan warna di javascript"
date: 2024-01-22
tags: [teknologi, webdev]
---
  
  {{< katex >}}

Bermain dengan warna di Javascript dengan membuat webapp color palette sederhana. Webapp ini nantinya akan generate palet warna berdasarkan gambar input.

Tujuan utama projek ini adalah, kamu bisa mengatahui mekanisme konversi RGB ke HSL.

## Kenapa kamu harus menggunakan HSL?

RGB (**R**ed, **G**reen, **B**lue) adalah model warna additive yang digunakan untuk merepresentasikan warna dari perangkat elektronik ke persepsi manusia. Singkatya, RGB ini adalah salah satu cara manusia agar komputer memahami warna yang manusia pahami selama ini. Kelemahan RGB adalah kamu harus mengkalkulasi juga porsi 3 kanal (*channel*) di masing-masing kanal warna RGB.

Kehadiran model HSL datang untuk mempermudah kamu mengatur warna di perangkat digital. **H**ue sebagai warna murni itu sendiri yang menjadi patokan, **S**aturation mengatur tingkat kekusaman (*grayish*) dari Hue, dan **L**igthness mengatur intensitas cerah dari Hue.

<!--- Semakin besar *gap* value antar kanal, maka semakin dominan juga warna dengan kanal yang paling besar, misal: rgb(0,255,0) menghasilkan hijau murni dan rgb(200,50,60) menghasilkan merah yang lebih dominan. Namun jika *gap* value nya sangat dekat atau bahkan sama akan menghasilkan warna mendekati hitam, abu-abu, atau putih --->

## Garis besar webapp

1. Membuat sebuah form untuk input gambar.
2. Menampilkan gambar input.
3. Generate palet warna dan shadenya berdasarkan gambar input dengan bantuan [Color Thief](https://lokeshdhakar.com/projects/color-thief/).

## Membuat webapp

### HTML dan CSS

Ini adalah ini adalah HTML nya:

```html
<body>
  <main>
    <h1>Get Color Palette from Image</h1>
    <div class="output-container">
      <img id="js-output"></img>
    </div>
  
    <p>
      Upload Image: 
      <input type="file" multiple="false" accept="image/*" id="js-input">
    </p>
  </main>
</body>
```
Tag `input` dibuat agar hanya menerima satu file berjenis gambar (png, jpg, png, etc...).

Untuk CSS, alangkah baiknya kamu reset default browser stylesheet dulu dengan CSS Reset:

```css
*, *::before, *::after {
  box-sizing: border-box;
}

* {
  margin: 0;
  padding: 0;
  font: inherit;
}

img, picture, svg, video {
  display: block;
  max-width: 100%;
}
```

Halaman akan dibatasi luasnya max. 800px dan akan menggunakan font [Poppins](https://fonts.google.com/specimen/Poppins):

```css
main {
  margin: 0 auto;
  width: 800px;
  font-family: Poppins, sans-serif;
}
```

Untuk bagian atas lebih berfokus kepada komponen input:

{{< figure src="img/img01.png" >}}

```css
h1 {
  text-align: center;
  font-size: 2.6rem;
}

h1, p {
  margin: 3rem auto;
}

.output-container {
  border: 2px solid darkslateblue;
}
```

Untuk bagian bawah atau bagian komponen output, mungkin kamu bisa secara mencoba-coba dulu styling secara manual agar nanti pas scripting DOM di Javascript tidak modar 😁.

Kamu bisa membuat 2 buah *dummy* swatches:

```css
<div class="swatch-container">
  <div class="swatch">
    /* ... */
  </div>
  <div class="swatch">
    /* ... */
  </div>
</div>
```

Didalam setiap `swatch`, akan ada satu kotak besar untuk `color-main` yang adalah warna output dari Color Thief, dan 10 warna yang akan diisi dengan shade-shade dari `color-main`.

```css
<div class="swatch">
  <p>#color-code-01</p>
  <div class="color-container">
    <div class="color-main"></div>
    <div class="shade-container">
      <div class="shade"></div>
      <div class="shade"></div>
      <div class="shade"></div>
      <div class="shade"></div>
      <div class="shade"></div>
      <div class="shade"></div>
      <div class="shade"></div>
      <div class="shade"></div>
      <div class="shade"></div>
      <div class="shade"></div>
    </div>
  </div>
</div>
```

Hasilnya akan seperti ini:

{{< figure src="img/img02.png" >}}

### Javascript: Mengamankan masalah terbesar

Pertama, kamu akan membuat dulu simulasi `RGBtoHSL()` dengan mengkonversi warna indigo `rgb(51, 0, 153)` ke `hsl(	260, 100%, 60%)`

```js
const colorRGB = [51, 0, 153];

function RGBtoHSL(r, g, b) {
  
  return [h, s, l]
}

console.log(RGBtoHSL(...colorRGB))
```

Tentu saja kamu harus pantau fungsi yang kamu buat dengan `console.log()`

#### Hue

Untuk konversi ke HSL, semua kanal RGB harus di *normalisasi*, caranya adalah dengan membagi setiap kanal dengan max. nilai RGB, yaitu 255.

$$
  R^{\prime} = \frac {R} {255}\qquad
  G^{\prime} = \frac {G} {255}\qquad
  B^{\prime} = \frac {B} {255}\qquad
$$

Lalu menentukan nilai \\(\max\\) dan \\(\min\\) dari \\((R^{\prime}, G^{\prime}, B^{\prime})\\), dan jangan lupa juga untuk mencari selisihnya.

\\(Cmax = \max(R^{\prime}, G^{\prime}, B^{\prime})\\)

\\(Cmin = \min(R^{\prime}, G^{\prime}, B^{\prime})\\)

\\(\Delta = Cmax - Cmin\\)  

```js
function RGBtoHSL(r, g, b) {
  r /= 255, g /= 255, b /= 255
  const cmax = Math.max(r, g, b)
  const cmin = Math.min(r, g, b)
  const cdelta = cmax - cmin

  return [h, s, l];
}
```

Rumus Hue adalah yang paling ribet diatara ketiganya, rumus tergantung dari \\(Cmax\\) dan jika sama semua maka Hue nya 0

$$ 
  H = \begin{cases}
     60\degree\times (\frac {G^{\prime} - B^{\prime}} {\Delta} \bmod 6)\quad\text{if } Cmax = R^{\prime} \\\
     \newline
     60\degree\times (\frac {B^{\prime} - R^{\prime}} {\Delta} + 2)\quad\text{if } Cmax = G^{\prime} \\\
     \newline
     60\degree\times (\frac {R^{\prime} - G^{\prime}} {\Delta} + 4)\quad\text{if } Cmax = B^{\prime} \\\
     \newline
     0\degree \quad\text{if } \Delta = 0
  \end{cases}
$$

```js
function RGBtoHSL(r, g, b) {
  // ...
  let h, s, l; //Jangan lupa deklarasi HSL nya

  switch (cmax) {
    case cmin:
      h = 0
      break;
    case r:
      h = ((g - b) / cdelta) % 6
      break;
    case g:
      h = ((b - r) / cdelta) + 2
      break
    case b:
      h = ((r - g) / cdelta) + 4
      break;
    default:
      break;
  }

  h = Math.round(h * 60)
  // ...
}
```

#### Lightness

Formula untuk lightness adalah rata-rata dari \\(Cmax\\) dan \\(Cmin\\).

$$
  L = \frac {Cmax + Cmin} {2}
$$

```js
function RGBtoHSL(r, g, b) {
  // ...
  l = (cmax + cmin) / 2
  // ...
}
```

#### Saturation

Formula Saturation agak mirip dengan Hue, Saturation mengambil Lightness sebagai patokan.

$$
  S = \begin{cases}
    \frac {\Delta} {Cmax+Cmin}\quad \text {if } L \le 0.5 \\\
    \newline
    \frac {\Delta} {2-Cmax-Cmin}\quad \text {if } L \gt 0.5 \\\
    \newline
     0 \quad\text{if } \Delta = 0
  \end{cases}
$$

```js
function RGBtoHSL(r, g, b) {
  // ...
  if (cmax === cmin) {
    s = 0
  } else {
    s = l > 0.5 ? cdelta / (2 - cmax - cmin) : (cmax - cmin) / (cmax + cmin);
  }
  // ...
}
```

#### Honorable mention untuk Lightness

Satu satunya formula yang tidak ada syarat khusus untuk \\(\Delta = 0\\) adalah Lightness, ini dikarenakan \\(\Delta = 0\\) berarti semua kanal RGB adalah sama. Kanal yang sama akan selalu menghasilkan range warna putih-hitam atau ke abu-abuan.

Kamu juga bisa mempersingkat kode dengan membuat logika seperti ini:

```js
function RGBtoHSL(r, g, b) {
  // Variable declarations

  l = (cmax + cmin) / 2;

  if (cmax === cmin) {
    [h, s] = [0, 0]
  } else {
    // Hue conversion
    // Saturation conversion
  }

  return [h, s, l];
}
```

### Javascript: Input gambar

Agar gambar yang diinput bisa ditampilkan, gunakan `FileReader` bawaan Javascript.

```js
const swatchContainer = document.getElementById("js-swatch-container")
const input = document.querySelector("#js-input");
const output = document.querySelector("#js-output")
let imageURL = "";

input.addEventListener("change", function() {
  const reader = new FileReader();
  reader.addEventListener("load", function() {
    imageURL = reader.result;
    output.src = imageURL
    output.alt = "Uploaded Image"
  })
  reader.readAsDataURL(this.files[0])
})
```

### Javascript: Menggunakan Color Thief

Cara paling praktis menggunakan [Color Thief](https://lokeshdhakar.com/projects/color-thief/#getting-started) adalah melalui CDN.

*Paste*-kan ini di bagian paling atas HTML:

```html
<head>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/color-thief/2.3.0/color-thief.umd.js"></script>
</head>
```

Oke, sekarang lanjut ke Javascript.

Berdasarkan [panduan](https://lokeshdhakar.com/projects/color-thief/#getting-started), kamu harus membuat *instance* baru untuk menggunakan Color Thief:

```js
  const colorThief = new ColorThief();
```

Cara menggunakannya cukup mudah, tinggal panggil `colorThief` dengan elemen output tadi sebagai argumen. Tapi sebelum itu pastikan gambarnya sudah ready atau sudah ter-*load* sepenuhnya.

```js
const colorThief = new ColorThief()

output.addEventListener("load", function() {
  const getPalettes = colorThief.getPalette(output, 5);
  console.log(getPalettes); // Untuk kebutuhan monitoring, opsional
})
```

Diatas, kamu akan mengambil 5 warna dari gambar input.

### Javascript: Mempersiapkan DOM untuk palette warna

Bagian ini tidak terlalu sulit juga tidak terlalu mudah, karena yang dilakukan hanya membuat perulangan untuk `swatch` dan 9 `shade` di setiap `swatch`.

```js
function generateSwatch(color, swatchIds) {
  swatchIds = `sw-${swatchIds}`
  const divElement = document.createElement("div")
}
```

Kamu butuh `swatchIds` sebagai alat *unique class* dan `divElement` untuk membuat element `swatch` baru.

Lalu, kamu butuh sebuah variabel untuk menyimpan 9 buah shades dari warna input `color`. Dibungkus dalam fungsi akan mempermudah kerjaanmu:

```js
function generateSwatch(color, swatchIds) {
  // ...
  function generateShade() {
      let shade = "";
      for (let i = 1; i < 10; i++) {
        shade += `<div class="shade shade-${i} ${swatchIds}"></div>`;
      }
      return shade
  }
  // ...
}
```

Kenapa menggunakan `string`?, karena disini kamu akan menggunakan `innerHTML` untuk meletakkan element yang dibuat. Walaupun ini cara paling mudah, namun sangat rawan keamanannya jika kamu menggunakannya untuk produksi skala besar.

{{< alert >}}
**Warning!** Jangan sering menggunakan `innerHTML` untuk produksi skala besar!
{{< /alert >}}

`div` yang baru dibuat tadi, tambah class baru, yaitu `swatch` dan `swatchIds` untuk membuat swatch baru. Lalu "tambalkan" fungsi `generateShade()` kamu yang diatas tadi kedalam `innerHTML` swatch nya:

```js
function generateSwatch(color, swatchIds) {
  // ...

  divElement.className = `swatch ${swatchIds}`
  swatchContainer.appendChild(divElement)
  swatchContainer.lastElementChild.innerHTML = `
  <p>rgb(${color.join()})</p>
  <div class="color-container ${swatchIds}" >
    <div class="color-main"></div>
    <div class="shade-container ${swatchIds}">
      ${generateShade()}
    </div>
  </div>
  `

  // ...
}
```

Sejauh ini, kamu berhasil membuat element HTML nya. Sekarang tinggal styling `background-color` setiap kotak agar menjadi shade yang diharapkan.

```js
function generateSwatch(color, swatchIds) {
  // ...

  const mainColor = document.querySelector(`.${swatchIds} .color-main`)
  mainColor.style.backgroundColor = `rgb(${color})`
  
  let hsl = RGBtoHSL(...color)
  for (let j = 1; j < 10; j++) {
    const shadeClass = document.querySelector(`.shade-${j}.${swatchIds}`)
    shadeClass.style.backgroundColor = `hsl(${hsl[0]} ${hsl[1]}% ${j * 10}%)`;
  }

  // ...
}
```

Perulangan 9 kali diatas akan membuat 9 shade dari warna input `color`, Ligthness yang pertama akan \\(10\\% \\), lalu \\(20 \\% \\) dan seterusnya.

### Javascript: Tahap akhir

Kamu sudah mempersiapkan semuanya, sekarang yang harus dilakukan adalah kembali ke `eventListener` dari `output` dan gunakan `forEach()` untuk iterasi palette warna dari variabel diatasnya:

```js
output.addEventListener("load", function() {
  const getPalettes = colorThief.getPalette(output, 5);
  getPalettes.forEach((element, index) => {
    generateSwatch(element, index)
  });
})
```

{{< figure src="img/img03.png" >}}

### Javascript: Bug

Ketika kamu mengganti gambar untuk ke dua kalinya, 5 palette sebelumnya masih meninggalkan jejak. Ini dikarenakan `generateSwatch()` yang terus menambah element baru setiap gambar berganti.

Caranya adalah, kamu harus mereset `innerHTML` dari element di variabel `swatchContainer` setiap kali gambar diganti:

```js
input.addEventListener("change", function() {
  swatchContainer.innerHTML = "" // Tambahkan ini untuk mereset isi setiap gambar berubah
  const reader = new FileReader();
  reader.addEventListener("load", function() {
    imageURL = reader.result;
    output.src = imageURL
    output.alt = "Uploaded Image"
  })
  reader.readAsDataURL(this.files[0])
})
```

## Hasil akhir

Hasil akhir yang lengkap milik saya bisa dilihat melalui: 

- [Live](https://ifumikuah.github.io/webdev-miniprojects/self-miniprojects/color-palette-generator/)

- [Source Code](https://github.com/ifumikuah/webdev-miniprojects/tree/main/self-miniprojects/color-palette-generator)

