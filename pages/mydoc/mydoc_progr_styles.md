---
title: Стили написания программ на PascalABC.NET
keywords: styles
last_updated: 07.01.2020
sidebar: mydoc_sidebar
permalink: mydoc_progr_styles.html
toc: false
folder: mydoc
---

PascalABC.NET как мультипарадигменный язык позволяет писать программы, используя различные стили и различные парадигмы.
Это может быть использовано грамотным преподавателем для построения хорошей траектории обучения прогаммированию - от азов до сложных современных элементов.

Рассмотрим несколько стилей написания программ на PascalABC.NET

### Алгоритмический стиль PascalABC.NET 

```pascal
begin
  var (a,b) := ReadInteger2; // автовывод типов
  var sum := 0;              // автовывод типов
  for var i:=a to b do       // счётчик цикла описывается в заголовке цикла
    sum += i*i;
  Print($'Сумма = {sum}')    
end.
```

### Функциональный стиль PascalABC.NET

```pascal
begin
  var (a,b) := ReadInteger2;
  (a..b).Sum(x -> x*x).Print
end.
```

### Процедурный стиль

```pascal
function SumSquares(a,b: integer): integer;
begin
  Result := 0;
  for var i := a to b do     
    Result += i * i
end;

begin
  var (a,b) := ReadInteger2;   
  Print($'Сумма = {SumSquares(a,b)}')    
end.
```

### Сочетание функционального и процедурного стиля

```pascal
function SumSquares(a,b: integer) := (a..b).Sum(x -> x*x);

begin
  var (a,b) := ReadInteger2;
  Print($'Сумма = {SumSquares(a,b)}')    
end.
```

### Объектный стиль

```pascal
type Algorithms = class
  static function SumSquares(a,b: integer) := (a..b).Sum(x -> x*x);  
  static function SumCubes(a,b: integer) := (a..b).Sum(x -> x*x*x);
end;

begin
  var (a,b) := ReadInteger2;
  Println($'Сумма квадратов = {Algorithms.SumSquares(a,b)}');
  Println($'Сумма кубов = {Algorithms.SumCubes(a,b)}')    
end.
```

### Стиль C# (для тех, кто даже на PascalABC.NET предпочитает писать как на C#)

```pascal
// Запуск - по Shift-F9
uses System;
begin
  var arr := Console.ReadLine.Split(new char[](' '),StringSplitOptions.RemoveEmptyEntries);
  var (a,b) := (integer.Parse(arr[0]),integer.Parse(arr[1]));
  var sum := 0;
  for var i:=a to b do 
    sum += i*i;
  Console.WriteLine($'Сумма = {sum}')
end.
```

### Стиль старого паскаля (не рекомендуется)

```pascal
var a,b,i,sum: integer; // всё в кучу, описание далеко от использования
begin
  Read(a,b);
  sum := 0;
  for i:=a to b do
    sum := sum + i*i;
  Write('Сумма = ',sum)
end.
```



{% include links.html %}
