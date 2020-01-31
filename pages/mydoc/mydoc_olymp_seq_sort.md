---
title: Олимпиадные задачи. Сортировка и последовательности
keywords: olympiads
last_updated: 12.01.2020
sidebar: mydoc_sidebar
permalink: olymp_seq_sort.html
toc: false
folder: mydoc
---

<script src="//i.upmath.me/latex.js"></script> 

### Задачи, использующие использующие последовательности, возможно, упорядоченные.

[**ЗАДАЧА №5**](https://acmp.ru/index.asp?main=task&id_task=5) 		
	
**Статистика** (Время: 1 сек. Память: 16 Мб Сложность: 15%)

Вася не любит английский язык, но каждый раз старается получить хотя бы четверку за четверть, чтобы оставаться ударником. В текущей четверти Вася заметил следующую закономерность: по нечетным дням месяца он получал тройки, а по четным – четверки. Так же он помнит, в какие дни он получал эти оценки. Поэтому он выписал на бумажке все эти дни для того, чтобы оценить, сколько у него троек и сколько четверок. Помогите Васе это сделать, расположив четные и нечетные числа в разных строчках. Вася может рассчитывать на оценку 4, если четверок не меньше, чем троек.
Входные данные

В первой строке входного файла INPUT.TXT записано единственное число N – количество элементов целочисленного массива (1 ≤ N ≤ 100). Вторая строка содержит N чисел, представляющих заданный массив. Каждый элемент массива – натуральное число от 1 до 31. Все элементы массива разделены пробелом.
Выходные данные

В первую строку выходного файла OUTPUT.TXT нужно вывести числа, которые соответствуют дням месяцев, в которые Вася получил тройки, а во второй строке соответственно расположить числа месяца, в которые Вася получил четверки. В третьей строке нужно вывести «YES», если Вася может рассчитывать на четверку и «NO» в противном случае. В каждой строчке числа следует выводить в том же порядке, в котором они идут во входных данных. При выводе, числа отделяются пробелом. 

```pascal
begin
  var n := ReadlnInteger;
  var a := ReadArrInteger(n);
  var s1 := a.Where(t -> t.IsOdd).Println;
  var s2 := a.Where(t -> t.IsEven).Println;
  if s1.Count > s2.Count then
    Write('NO')
  else
    Write('YES')
end.
```

Вначале путем фильтрации элементов массива а формируется последовательность нечетных чисел , которая выводится при помощи функции Println и запоминается в s1. Затем таким же путем формируется последовательность четных чисел s2. Далее выводится YES или NO в зависимости от результата сравнения количеств элементов в этих последовательностях.

[**ЗАДАЧА №9**](https://acmp.ru/index.asp?main=task&id_task=9)
	
**Домашнее задание** (Время: 1 сек. Память: 16 Мб Сложность: 27%)

Петя успевает по математике лучше всех в классе, поэтому учитель задал ему сложное домашнее задание, в котором нужно в заданном наборе целых чисел найти сумму всех положительных элементов, затем найти где в заданной последовательности находятся максимальный и минимальный элемент и вычислить произведение чисел, расположенных в этой последовательности между ними. Так же известно, что минимальный и максимальный элемент встречаются в заданном множестве чисел только один раз и не являются соседними. Поскольку задач такого рода учитель дал Пете около ста, то Петя как сильный программист смог написать программу, которая по заданному набору чисел самостоятельно находит решение. А Вам слабо?

**Входные данные**
В первой строке входного файла INPUT.TXT записано единственное число N – количество элементов массива. Вторая строка содержит N целых чисел, представляющих заданный массив. Все элементы массива разделены пробелом. Каждое из чисел во входном файле, в том числе и N, не превышает 10<sup>2</sup> по абсолютной величине.

**Выходные данные**
В единственную строку выходного файла OUTPUT.TXT нужно вывести два числа, разделенных пробелом: сумму положительных элементов и произведение чисел, расположенных между минимальным и максимальным элементами. Значения суммы и произведения не превышают по модулю 3*10<sup>4</sup>.

```pascal
begin
  var n := ReadlnInteger;
  var a := ReadArrInteger(n);
  var (i1, i2) := (a.IndexMax, a.IndexMin);
  if i2 < i1 then
    Swap(i1, i2);
  Print(a.Where(t -> t > 0).Sum, Trunc(a?[i1 + 1:i2].Product))
end.
```

Вначале находятся индексы максимального и минимального элементов в массиве - i1 и i2. Не имеет значения, принадлежит i1 (i2) максимальному или минмальному элементу, ведь требуется только интервал [i1, i2]. Поэтому следует обеспечить выполнение условия i1 < i2. Выводится сумма элементов массив, прошедших фильтрацию по условию t > 0, и также произведение элементов, попавших в интервал [i1, i2]. Отбор элементов обеспечивается безопасным срезом. Безопасный срез не контролирует выход индекса границу массива и выполняется немного быстрее. Можно было воспользоваться и обычным срезом a[i1 + 1 : i2]. Расширение Product получает произведение элементов, попавших в срез. Поскольку Product возвращает значение типа real, пришлось использовать функцию Trunc для приведения к целому.
