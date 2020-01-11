---
title: Программы и алгоритмы с использованием последовательностей
keywords: sequences, examples, programs, algorithms
last_updated: 12.01.20
summary: "Здесь приведены примеры программ и алгоритмов, используемых в курсе Основы программирования для студентов 1 курса ФИИТ мехмата ЮФУ"
sidebar: mydoc_sidebar
permalink: progr_sequences.html
folder: mydoc
---

## Последовательности - опредление

**Последовательность** – набор элементов, которые можно перебирать один за другим.

Последовательность:
* Вообще говоря не хранится в памяти целиком 
* Задает алгоритм получения значений одно за другим
* В каждый момент в памяти одно значение
* Цикл по последовательности – цикл foreach
* Массивы и списки – разновидности последовательностей

## Пользовательские генераторы последовательностей

```pascal
function Arithm(n: integer; a0,h: real): sequence of real;
begin
  var x := a0;
  loop n do
  begin
    yield x;
    x += h;
  end;
end;

begin
  Arithm(10,11,3).Println;
  foreach var x in Arithm(10,11,3) do
    Print(x);
end.
```

Оператор yield возвращает значение из генератора и запоминает место, на котором мы остановились в функции. При вычислении следующего значения код продолжает выполняться с запомненного места.

## Генераторы бесконечных последовательностей

```pascal
function inf: sequence of integer;
begin
  while True do
    yield 1;
end;

function Take(s: sequence of integer; n: integer): sequence of integer;
begin
  var i := 1;
  foreach var x in s do
  begin
    yield x;
    i += 1;
    if i > n then break;
  end;
end;

begin
  Take(inf,10).Print;
end.
```

## Простые задачи на последовательности

**Задача 1.** Симметрична ли последовательность
```pascal
a.SequenceEqual(a.Reverse);
```

**Задача 2.** Все ли элементы последовательности различны
```pascal
a.Count = a.Distinct.Count;
```

**Задача 3.** Совпадают ли первая и вторая половина последовательности длины n
```pascal
var n2 := n div 2;
a.Take(n2).SequenceEqual(a.Skip(n2));
```

**Задача 4.** Найти второй максимум последовательности
```pascal
a.OrderDescending.Distinct.Skip(1).First;
```

**Задача 5.** Функция проверки x на простоту
```pascal
function IsPrime(x: integer) := (2..x.Sqrt.Round).All(i -> x mod i <> 0);
```

**Задача 6.** Количество простых чисел, меньших n
```pascal
function IsPrime(x: integer) := (2..x.Sqrt.Round).All(i -> x mod i <> 0);

begin
  var n := 50000;
  (2..n).Where(IsPrime).Count.Println;
  MillisecondsDelta.Println;
end.
```

**Задача 7.** Табулирование значений функции
```pascal
PartitionPoints(1,2,10).PrintLines(x -> $'{x,4:f1} {Sin(x),7:f4}');
```



{% include links.html %}
