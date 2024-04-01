---
title: "Input and Form Submission I"
date: 2024-04-01
description: "HTML input elements and submission form"
tags: [tech, webdev]
---

This article contains how to create and what are the inputs in HTML and how to process them in JS so that the data can be processed further.

Input is the most complex and essential element in Web Development. The way to create an `<input>` element is basically as follows.

```html
<input type="text" placeholder="type a key" />
```

## Create an input

If you try to make an input without additional attributes:

```html
<input/>
```

That will be same as:

```html
<input type="text"/>
```

Besides `type="text"`, `<input>` is also not limited to forms with boxes as above. There are many more, including:

 
|Type|Description|
|-|-|
|`type="text"` <sup>DEFAULT</sup>|Input text|
|`type="number"`|Number-only input|
|`type="color"`|Color input||
|`type="date"`|Date input|
|`type="range"`|Slider input that accepts numeric values `min` to `max`|
|`type="checkbox"`|A checkbox input, usually used for *boolean* input|
|`type="radio"`| Multiple choice input, can only select one choice|
 
### Label

In order for the person who fills in to really know what their input is intended for, you must give a `<label>` element according to the input.

```html
<label for="fullname">Full Name</label>
<input type="text" id="fullname" />
```

In the label element itself, there is a `for` attribute linked with the label and its input. For the value, you must give the same value as `id` of `<input>` for them to be linked.

### Submit button

If you are done with the input, you can submit and print the input results as below:

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

Best practices for forms are to wrap all inputs with a `<form>` element.

```html
<form>
  <label for="fullname">Full Name</label>
  <input type="text" id="fullname" />
  
  <label for="age">Age</label>
  <input type="number" id="age" />
</form>
```

## Properly make a form

The above provides a good basis for creating a form, but making it a functional form will be explained here.

A functional form is one that can interact with Javascript before being processed by the back-end, which means it must be compliant with Javascript rules. 

One of them is to provide the `id` attribute so that it can be identified by the DOM:

```html
<form id="form-i">
  <label for="fullname">Full Name</label>
  <input type="text" id="fullname" />
  
  <label for="age">Age</label>
  <input type="number" id="age" />
</form>
```

After that, we want the form to be read by Javascript as if it were a completed form. That is, we want Javascript to process the data into an object.

However, Javascript cannot capture the *key* of each *value* that we have entered. We already have the *value* of the input we typed, but the *key* must be obtained by giving the `name` attribute to each `<input>`.

```html
<form id="form-i">
  <label for="fullname">Full Name</label>
  <input type="text" id="fullname" name="fullname" />
  
  <label for="age">Age</label>
  <input type="number" id="age" name="age" />
</form>
```

**One more thing!**, a *Submit Button* is needed for the button to submit the completed form. At the same time we will tidy up (as in appearance) the form:

```html
<form id="form-i">
  <div class="forms-row">
    <label for="fullname">Full Name</label>
    <input type="text" id="fullname" name="fullname" required />

    <label for="age">Age</label>
    <input type="number" id="age" name="age" required />
  <div>
  <button type="submit">Submit</button>
</form>
```

{{< alert  "code" >}}
  You can add the `required` attribute to ensure the input is required.
{{< /alert >}}

## Make it functional in JS

Will be explained in [next article](../01-04-2024-input-dan-form-submission-js-ii/).