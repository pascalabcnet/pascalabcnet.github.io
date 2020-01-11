---
title: Программы и алгоритмы для матриц
keywords: examples, programs, algorithms
last_updated: 31.12.19
summary: "Здесь приведены примеры программ и алгоритмов, используемых в курсе Основы программирования для студентов 1 курса ФИИТ мехмата ЮФУ"
sidebar: mydoc_sidebar
permalink: progr_matrices.html
folder: mydoc
---

<script src="//i.upmath.me/latex.js"></script>


##  Основные действия с двумерными массивами (матрицами)

** Описание, заполнение в цикле и вывод** 


**Описание, заполнение в цикле и вывод** 

```pascal
**Код 1** 
var a2: array [,] of integer;
a2 := new integer[3,4];
for var i:=0 to a2.GetLength(0)-1 do
for var j:=0 to a2.GetLength(1)-1 do
  a2[i,j] := i + j;
a2.Println;
Println(a2); 
```

***Вывод***
```
0 1 2 3
1 2 3 4
2 3 4 5
[[0,1,2,3],[1,2,3,4],[2,3,4,5]]
```

**Описание, заполнение в цикле и вывод 2** 

```pascal
**Код 1** 
var a := new integer[3,4];
for var i:=0 to a.RowCount-1 do
for var j:=0 to a.ColCount-1 do
  a[i,j] := i + j;
a.Println(3); // 3 позиции под элемент
```

***Вывод*** 
```
0  1  2  3
1  2  3  4
2  3  4  5
```

{% include links.html %}
