---
title: "Implementasi Bubble Sort di Linked List"
date: 2024-07-23
description: "Sortir linked list dengan bubble sort"
tags: [teknologi, c]
---

Bubble sort adalah algoritma sort basic yaitu dengan menggeser element yang paling terbesar ke ujung sampai tidak ada lagi yang harus digeser. Sebelum melakukannya pada linked list, pastikan dulu kamu memahami [cara kerjanya pada array klasik](../23-05-2024-bubble-sort-c/).

## Overview

Mekanismenya mirip seperti pada array klasik, hanya saja kita menggunakan perulangan [seperti ini](../22-07-2024-linked-list-1/#penjelasan-mencari-ekor) untuk *travese* node sampai `node->next == NULL`.

## Prototype

```c
void bubble_sort (Node** root);
```

Dimana:
- `Node** root` adalah alamat dari alamat `root`.

## Definition

Bubble sort memerlukan sejenis `bool` untuk mengawasi perulangan menjawab *"apakah masih ada yang perlu digeser lagi atau tidak?"*. Juga, semenjak kita menangani linked list yang merupakan bukan array, kita perlu cara tersendiri untuk menghitung total node yang ada.

```c
void bubble_sort (Node** root)
{
  Node* node;
  node = *root

  int i = 0;
  while(node->next != NULL)
  {
    i++;
    node = node->next;
  }

  bool swapped;
}
```

- `node` digunakan untuk travese antar node, disaat tertentu, adakalanya kita harus mereset `node` dengan `node = *root` untuk mengulang traverse.
- `i` adalah total node tanpa node terakhir, sama seperti array, kita tidak perlu node terakhir dihitung.
- `swapped` berupa boolean yang akan mengawasi berjalannya sortir.

Lalu setelah itu membuat perulangan seperti bubble sort pada umumnya. Untuk loop luar:

```c
void bubble_sort (Node** root)
{
  /*...*/

  for (int j = 0; j < i; j++)
  {
    swapped = false;
    node = *root;
    for (int j = 0; j < i; j++)
    {
      /*...*/
    }
    if (swapped == false) break;
  }
}
```

Di loop luar, kita perlu mereset `node` setiap perulangan.

Lalu untuk loop dalam, gunakan `swap_node` yang sudah kita buat [sebelumnya](../23-07-2024-linked-list-3/) untuk menukar node.

```c
void bubble_sort (Node** root)
{
  /*...*/

  for (int j = 0; j < i; j++)
  {
    swapped = false;
    node = *root;
    for(int k = 0; k < i-j; k++)
    {
      if (node->data > node->next->data)
      {
        swap_node(&root, node, node->next);
        swapped = true;
      }
      else
      {
        node = node->next;
      }
    }
    if (swapped == false) break;
  }
}
```

Penjelasan `if else` loop dalam, `node = node->next` akan traverse untuk mencari element paling besar pertama yang ditemuinya. Jika ketemu, maka dia akan menggeser ke kanan dan seterusnya.

## Full Code

```c
void bubble_sort (Node** root)
{
  Node* node;
  node = *root

  int i = 0;
  while(node->next != NULL)
  {
    i++;
    node = node->next;
  }

  bool swapped;
  for (int j = 0; j < i; j++)
  {
    swapped = false;
    node = *root;
    for(int k = 0; k < i-j; k++)
    {
      if (node->data > node->next->data)
      {
        swap_node(&root, node, node->next);
        swapped = true;
      }
      else
      {
        node = node->next;
      }
    }
    if (swapped == false) break;
  }
}
```