---
title: "Input and Form Submission II"
date: 2024-04-01
description: "HTML input elements and submission form, Part II"
tags: [tech, webdev]
---

This article is a continuation of the [previous one](../01-04-2024-input-dan-form-submission-js-i/). Previously we created a form that is ready to be processed with Javascript.

So now it's time to work on the Javascript code.

What we want is to store the object containing the data just entered (*datum*) into an array, something like:

```plain
[{...datum}, {...datum}, {...datum}]
```

First, the `<form>` element itself must be obtained, we have given the `id` to the element before. Then we need an empty array to store the objects.

```js
const form = document.getElementById("form");
const data = [];
```

Then we need to add an `EventListener` of type `submit` to the form. This will *trigger* an `event` right when the submit button we created earlier is pressed.

It will executes the `submitForm` function when the form is submitted by the person.

```js
form.addEventListener("submit", submitForm);
```

Okay, now we focus on the function. By *default* this event will reload the page when it is *triggered*, this means that all filled forms will be read but the page itself will resets instantly.

We don't want that to happen, so use `preventDefault()` to remove the default behavior of this event.

```js
function submitForm(event) {
  event.preventDefault();
}
```

`FormData` plays an important role in this input processing, the `FormData()` constructor offers the best way to convert the inputted form into a key-value pair that can later be stored in an object.

```js
function submitForm(event) {
  const formData = new FormData(form);

  event.preventDefault();
}
```

`formData` currently stores key-value pairs that can be iterated with a for-loop.

```js
function submitForm(event) {
  const formData = new FormData(form);

  for (const [key, val] of formData) {
    // Do smth with 'key' and 'val'
  }

  event.preventDefault();
}
```

We will store the key-value pair in an object. Then we need the object to be inserted into an array.

```js
function submitForm(event) {
  const formData = new FormData(form);
  let datum = {}

  for (const [key, val] of formData) {
    datum[key] = val
  }
  data.push(datum) //Push to an array

  event.preventDefault();
}
```

The JS code to convert the form into an object is done.

However, if you want to really make sure, for example: log to the console or print data with DOM manipulation.

You can use the code below to log to the console:

```html
<main>
  <form id="form">
    <!-- forms -->
  </form>
  <!-- Create a button outside the form to log saved data -->
  <button style="margin: 1rem 0;" id="refresh-data">Refresh Data</button>
</main>
```
```js
const refresh = document.getElementById("refresh-data");

refresh.addEventListener("click", () => {
  console.log(data);
});
```

The source code of the tutorial can be accessed through [JSFiddle](https://www.jsfiddle.net):

{{< button href="https://jsfiddle.net/detrux/Lzo35ujy/2/" target="_blank" >}}
Basic
{{< /button >}}
{{< button href="https://jsfiddle.net/detrux/dcz92rta/3/" target="_blank" >}}
More Advanced Version
{{< /button >}} 