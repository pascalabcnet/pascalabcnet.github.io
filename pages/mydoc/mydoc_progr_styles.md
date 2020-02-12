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

### Стиль PascalABC.NET с алгоритмом

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

### Стиль C#

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
  Write(sum)
end.
```



{% include links.html %}
