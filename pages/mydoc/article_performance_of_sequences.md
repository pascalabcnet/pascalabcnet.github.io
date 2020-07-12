---
title: Методы последовательностей vs рукописные алгоритмы для массивов
keywords: examples, programs, algorithms
last_updated: 12.07.20
summary: "Статья даёт сравнительный анализ производительности простейших методов последовательностей в сравнении с рукописными алгоритмами для массивов"
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

## Программа 1. Интегральный алгоритм

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

## Программа 2. Без сортировки.

Закомментируем сортировку в обоих случаях. 

```pascal
##
var n := 10000000;
var a := ArrRandom(n,0,integer.MaxValue-1);
var b := Copy(a);
Milliseconds;
a.Where(x -> x mod 2 = 0).Select(x -> x div 2).Count(x->x>integer.MaxValue div 4).Println;
Println(MillisecondsDelta);

a := Copy(b);  
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

var count := 0;
for var i:=0 to a.Length-1 do
  if a[i] > integer.MaxValue div 4 then
    count += 1;

Println(count);
Println(MillisecondsDelta);
```


**Результат:**
```pascal
163
85
```

**Вывод.**  Всего в 2 раза медленнее. То есть, сортировка для последовательностей по ключу ощутимо неэффективнее обычной сортировки (асимптотики у них совпадают - n * log(n))

## Программа 3. Только вычисление Count

```pascal
var n := 100000000;
var a := ArrRandom(n,0,integer.MaxValue-1);
var b := Copy(a);
Milliseconds;
a.Count(x->x>integer.MaxValue div 4).Println;
Println(MillisecondsDelta);

a := Copy(b);
var count := 0;
for var i:=0 to a.Length-1 do
  if a[i] > integer.MaxValue div 4 then
    count += 1;
```

**Результат:**
```pascal
713
375
```

**Вывод.** Опять-таки в 2 раза медленнее.

## Программа 4. Только Where и Select

```pascal
var n := 100000000;
var a := ArrRandom(n,0,integer.MaxValue-1);
var b := Copy(a);
Milliseconds;
a.Where(x -> x mod 2 = 0).Select(x -> x div 2).Count.Println;
Println(MillisecondsDelta);

a := Copy(b);
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
Println(MillisecondsDelta);
```

**Результат:**
```pascal
814
678
```

**Вывод.** На 20% медленнее. Всего-то

## Программа 5. Where, Select и Count - более сложные лямбда-выражения

Сделаем предположение, что падение производительности в методах последовательностей происходит из-за лямбда-функций с очень простым телом. Накладные расходы на вызов такой функции сравнимы с накладными расходами на вычисление этих выражений. 

Усложним выражения:

```pascal
##
var n := 10000000;
var a := ArrRandom(n,0,integer.MaxValue-1);
var b := Copy(a);
var q := |1,2,3,4,5|;
Milliseconds;
a.Where(x -> (x mod 10) in q).Select(x -> x div 2 + x div 3 + x div 4 + x div 5 + x div 6).Count(x->x>integer.MaxValue div 4).Println;
Println(MillisecondsDelta);

a := b;
var c := Copy(a);
var j := 0;
for var i:=0 to a.Length-1 do
  if a[i] mod 10 in q then
  begin  
    a[j] := a[i];
    j += 1;
  end;
SetLength(a,j);

for var i:=0 to a.Length-1 do
  a[i] := a[i] div 2 + a[i] div 3 + a[i] div 4 + a[i] div 5 + a[i] div 6;

var count := 0;
for var i:=0 to a.Length-1 do
  if a[i] > integer.MaxValue div 4 then
    count += 1;

Println(MillisecondsDelta);
```

**Результат:**
```pascal
427
406
```

**Вывод.** На 5% медленнее. Совсем ни о чём. 

**Итоговый вывод.** Последовательности столь же эффективны по времени, что и массивы. Неэффективность в методах последовательностей возникает из-за передачи в качестве параметров лямбда-функций. Однако чем более сложное будет тело лямбда-функций, тем меньше будет разница по времени при использовании методов последовательностей и рукописных адгоритмов для массивов

А по записи? Что более понятно? Где меньше вероятность сделать ошибку? Что легче модифицировать? 

Вам судить:

**Последовательности**:

```pascal
a.Where(x -> x mod 2 = 0).Select(x -> x div 2).Count.Println;
```

**Рукописные алгоритмы с массивами**:

```pascal
a := Copy(b);
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
```







{% include links.html %}
