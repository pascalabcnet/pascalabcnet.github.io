---
title: Производительность алгоритмов с последовательностями
keywords: examples, programs, algorithms
last_updated: 12.07.20
summary: "Статья даёт сравнительный анализ производительности простейших методов последовательностей в сравнении с массивами"
sidebar: mydoc_sidebar
permalink: article_performance_of_sequences.html
folder: mydoc
---

<script src="//i.upmath.me/latex.js"></script> 

Методы последовательностей позволяют записать решения многих стандартных задач в одну строку и этим очень удобны. 

А что можно сказать об их эффективности?

Рассмотрим следующую простую задачу.

**Задача**. Дано n=1000000 случайных целых чисел от 0 до MaxInt-1. Отфильтровать чётные, поделить нацело на 2, отсортировать и вычислить количество элементов > MaxInt div 4

Решить с использованием лямбда-выражений и последовательностей, а также классическим способом с использованием рукописных алгоритмов для массивов.

Что быстрее? Насколько?

Понятно, что методы последовательностей медленнее рукописных алгоритмов. Но вот насколько?

## Программа 1. 

```pascal
##
var n := 10000000;
var a := ArrRandom(n,0,integer.MaxValue-1);
var b := Copy(a);
Milliseconds;
a.Where(x -> x mod 2 = 0).Select(x -> x div 2).Order.Count(x->x>integer.MaxValue div 4).Println;
Println(MillisecondsDelta);

a := Copy(b); // в рукописном алгоритме массив меняется. Запоминаем его копию чтобы учесть это в суммарном времени 
var j := 0;
for var i:=0 to a.Length-1 do
  if a[i] mod 2 = 0 then
  begin  
    a[j] := a[i];
    j += 1;
  end;
SetLength(a,j);

for var i:=0 to a.Length-1 do
  a[i] := a[i] div 2;

Sort(a);

var count := 0;
for var i:=0 to a.Length-1 do
  if a[i] > integer.MaxValue div 4 then
    count += 1;

Println(count);
Println(MillisecondsDelta);
```

**Результат:**
```pascal
2495939
1526
2495939
407
```

**Вывод.** Программа с методами последовательностей работает в 3.7 раза медленнее программы с рукописными алгоритмами.

Для чистоты 


{% include links.html %}
