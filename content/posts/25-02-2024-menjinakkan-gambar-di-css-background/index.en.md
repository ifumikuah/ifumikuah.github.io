---
title: "Playing with images in CSS: Background Edition"
date: 2024-02-25
description: "Playing around with element related with images in CSS"
tags: [teknologi, webdev]
---

There are two types of images that can be used when creating images in webdev, the `<img>` element method as before and the `background-image` method from CSS.

The `background` in CSS, cannot be read by the DOM and also has no SEO influence, this makes `background` more suitable as an image for background or other decorative purposes.

Here we will use the similar image and code as before:

```html
<style>
  body {
    height: 100vh;
    display: flex;
    align-items: center;
  }

  .box {
    background-image: url('assets/mbti-5.png');
    margin: 0 auto;
    width: 500px;
    height: 300px;
    background-color: rgb(211, 232, 255);
  }
</style>
<body>
  <div class="box"></div>
</body>
```

![Cover](img/img01.png "Cropped image")

## Background repeat

If the image is smaller than the container, it will be repositioned like tiles while maintaining the aspect ratio.

```css
.box {
  background-repeat: repeat;
}
```

![Repeat](img/img02.png "Repeates just as tiles")

Set `background-repeat` to `no-repeat` to remove the tiled repetitions.

## Background Size

`background-size` is just like `object-fit` basically. But `background-size` only has 2 values `contain` and `cover` which are both identical to `object-fit` in functionality.

![bg-size1](img/img03.png "`background-size: contain`")

![bg-size2](img/img04.png "`background-size: cover`")

## Background Position

The `background-position` is just the same as the `object-position`, and the value is the same too.

![bg-size2](img/img05.png "`background-position: 20%`")

Unlike `object-position`, the default value of `background-position` is `0% 0%`.