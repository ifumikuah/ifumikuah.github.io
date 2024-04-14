---
title: "Understanding shorthand flex in CSS"
date: 2024-04-14
description: "Brief explanation about flex-grow, flex-shrink, and flex-basis"
tags: [tech, webdev]
---

{{< katex >}}

Flexbox is a 2-dimensional CSS layout that is often used for responsive screen needs. Each item in flex has its own `flex` property, with values: `0 1 auto`.

`flex` is shorthand of:

```plain
flex: [flex-grow] [flex-shrink] [flex-basis]
```

`flex: 0 1 auto` just same as:

```css
.class {
  flex-grow: 0;
  flex-shrink: 1;
  flex-basis: auto;
}
```

## It's all about proportion

 
To understand `flex-grow` and `flex-shrink`, you just need to play around with proportions.

example:

There are 5 items, each with `flex-grow: 1`.

![img01](./img/img01.png "Five items")

This means each item is \\(\dfrac{1}{5}\\) the size of the container.

**What if one gets `flex-grow: 3`**?

![img02](./img/img02.png "C gets `flex-grow: 3`")

This means that items other than C still get 1, while C items get 3.

Creates \\(1+1+3+1+1 = 7\\), means that A, B, D, E have the size of \\(\dfrac{1}{7}\\), while C has \\(\dfrac{3}{7}\\) **of the container size**.

## Grow and shrink

Remember that the size of an item is also determined by the size of its container, so `flex-grow` and `flex-shrink` regulate how that happens.

- `flex-grow` will regulate item size **if the container size is larger than the size of all its items combined**.

- `flex-shrink` will regulate the size of an item **if the container size is smaller than the size of all its items combined**.

### Value 0

The `flex-grow` default is `0`, which means that the item size never changes even if the container is increased or decreased. This also applies to `flex-shrink`.

## Flex-basis

The `flex-basis` is the initial size of the item. If `flex-grow` or `flex-shrink` is assigned a value of `0`, then the item size will use the value of `flex-basis`. 