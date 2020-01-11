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


### Описание, заполнение в цикле и вывод 

**Код 1** 
```pascal
var a2: array [,] of integer;
a2 := new integer[3,4];
for var i:=0 to a2.GetLength(0)-1 do
for var j:=0 to a2.GetLength(1)-1 do
  a2[i,j] := i + j;
a2.Println;
Println(a2); 
```

***Результат***
```
0 1 2 3
1 2 3 4
2 3 4 5
[[0,1,2,3],[1,2,3,4],[2,3,4,5]]
```

**Код 2** 

```pascal
**Код 1** 
var a := new integer[3,4];
for var i:=0 to a.RowCount-1 do
for var j:=0 to a.ColCount-1 do
  a[i,j] := i + j;
a.Println(3); // 3 позиции под элемент
```

***Результат*** 
```
0  1  2  3
1  2  3  4
2  3  4  5
```

### Заполнение по правилу и случайными

** Код ** 

```pascal
var a := Matr(3,4,1,2,3,4,5,6,7,8,9,10,11,12);
a.Println(3);
var a1 := MatrGen(3,4,(i,j)->i+j+0.5);
a1.Println(5,1);
```

***Результат*** 
```
1 2 3 4
5 6 7 8
9 10 11 12

0.5 1.5 2.5 3.5
1.5 2.5 3.5 4.5
2.5 3.5 4.5 5.5
```

** Код ** 

```pascal
var a := MatrRandomInteger(3,4);
a.Println(4);
var a1 := MatrRandomReal(3,4,1,9);
a1.Println(6,2);
```

***Результат*** 
```
22 32 10 41
11 25 50 50
81 19 25 73
7.58 1.99 4.99 2.09
7.39 2.82 3.04 7.39
5.86 8.64 1.33 5.63
```

## Операции со строками и столбцами

** Вывод k-той строки **

```pascal
var k := 1; // Номер строки

// Алгоритм
for var j:=0 to a.ColCount-1 do
  Print(a[k,j]);

// Метод
a.Row(k).Println;
```

** Вывод k-того столбца ** 

```pascal
var k := 2; // Номер столбца

// Алгоритм
k := 2;
for var i:=0 to a.RowCount-1 do
  Print(a[i,k]);

// Метод
a.Col(k).Println;
```

**Простейшие операции над строками и столбцами строками и столбцами ** 

```pascal
var a := MatrRandomInteger(3,4);
a.Println;
a.Row(0).Sum.Println;
a.Row(1).Average.Println;
a.Row(2).Product.Println;
a.Col(0).Min.Println;
a.Col(1).Max.Println;
```

## Массовые операции со строками и столбцами

**Сумма в каждой строке - алгоритм ** 

```pascal
var a := MatrRandomInteger(3,4);
a.Println;

var Sums := new integer[a.RowCount];
for var i:=0 to a.RowCount-1 do
begin
  var sum := 0;
  for var j:=0 to a.ColCount-1 do
    sum += a[i,j];
  Sums[i] := sum
end;
Sums.Println;
```

**Сумма в каждой строке - использование методов ** 

```pascal
var Sums := ArrGen(a.RowCount,r -> a.Row(r).Sum);
Sums.Println;
```

**Минимальный в каждом столбце** 

```pascal
var Mins := ArrGen(a.ColCount,c -> a.Col(c).Min);
Mins.Println;
```

**Количество четных в каждом столбце ** 

```pascal
var EvensCount := ArrGen(a.ColCount,c -> a.Col(c).Count(x->x.IsEven));
EvensCount.Println;
```

**Минимальный из максимальных элементов строк** 

```pascal
ArrGen(a.RowCount,r -> a.Row(r).Max).Min.Println;
```

## Замена строк (столбцов)

**Задача**. Упорядочить вторую строку матрицы по возрастанию

**Код**
```pascal
var a := MatrRandomInteger(3,4);
a.Println;
var b := a.Row(2);
Sort(b);
a.SetRow(2,b);
a.Println;
```

## Поиск в матрице

**Задача**. Содержит ли матрица заданный элемент

**Код**
```pascal
function Contains<T>(a: array [,] of T; x: T): boolean;
begin
  Result := False;
  for var i:=0 to a.RowCount-1 do
  for var j:=0 to a.ColCount-1 do
    if a[i,j]=x then
    begin
      Result := True;
      exit;
    end;
end;

begin
  var a := MatrRandomInteger(3,4,1,10);
  a.Println;
  var found := Contains(a,5);
  Println(found);
end.
```

{% include links.html %}
