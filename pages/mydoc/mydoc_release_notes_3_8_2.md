---
title: Версия 3.8.2
keywords: release notes, what's new, announcements, new features
last_updated: 21.10.21
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_8_2.html
toс: false
folder: mydoc
---

## Изменения в языке

### Операция not in
Если функция помечена атрибутом [Cache], то ее результат при первом вызове кешируется и при втором и последующих вызовах используется. Таким образом, второй и последующие вызовы такой функции выполняются мгновенно 

Хорошо иллюстрирует идею кеширования рекурсивная функция для вычисления чисел Фибоначчи. Вызов fib(42) выполнятся за 2 мс, а вызов fib1(42) - за 10 секунд (!)  

```pascal
##
[Cache]
function fib(n: integer): integer := 
  if n in 1..2 then 1 
  else fib(n-1) + fib(n-2);

function fib1(n: integer): integer := 
  if n in 1..2 then 1 
  else fib1(n-1) + fib1(n-2);

Println(fib(42),MillisecondsDelta/1000);
Println(fib1(42),MillisecondsDelta/1000);
```
Вывод:
```pascal
267914296 0.011 
267914296 9.362 
```

## Уточнения в языке

### Print теперь разделяет элементы вывода пробелами
 
```pascal
##
function Add(a,b: integer) := a + b;

Print(Add(2,3));
```

### Директива {$zerobasedstrings} и срезы
Директива {$zerobasedstrings} теперь воздействует на срезы
```pascal
{$zerobasedstrings}
begin
  var s := '0123456789';
  Print(s[0:5],s[5:8],s[8:]); // 01234 567 89
end.
```
## Стандартная библиотека

### Cтандартный поток ErrOutput
В стандартном модуле появился стандартный поток ErrOutput для вывода шибок:

```pascal
##
Print(ErrOutput,'Error1')
```

По умолчанию он связан с консолью, но легко перенаправляется в произвольный файл

### PartioalSum для числовых последовательностей

В процедуре Sort для массивов и списков появилась перегруженная версия с дополнительным параметром - проекцией на ключ:

```pascal
##
var pp := |('Сидоров',20),('Петров',22), ('Попов',20),('Иванов',22)|;
Sort(pp, p->p[1]); // Сортировка по возрасту
pp.Print;
```



## Стандартные модули

### Стандартный модуль XLSX
Стандартный модуль XLSX предназначен для

Пример. 
```pascal
uses PlotWPF;

begin
  var g := new GridWPF(2,2,10);

  var c := new LineGraphWPF(0,Pi,v -> v*Sin(v*10));
  c.PlotRect := Rect(0,0,10,10);
  c.Graph[0].ChangeData(0,Pi,x->x*x);
  c.Graph[0].Color := Colors.Green;
  c.Graph[0].Thickness := 3;
  c.AddLineGraph(0,Pi,v -> Sqrt(v));
  
  var c2 := new LineGraphWPF(0,Pi,v -> Sin(v*10)-Cos(v*7));
  
  var c4 := new MarkerGraphWPF(|1.0,2,3,4,5|,|5.0,15,7,12,2|);
  c4.AddLineGraph(|1.0,2,3,4,5|,|5.0+1,15+1,7+1,12+1,2+1|);
  c4.AddMarkerGraph(|1.0,2,3,4,5|,|5.0+1,15+1,7+1,12+1,2+1|,Colors.Bisque,MarkerType.Diamond,8);
  c4.Graph[0].Thickness := 0.7;
  c4.Graph[1].MarkerType := MarkerType.Box;
  c4.Graph[2].Thickness := 0.7;

  var gg := new GridWPF(2,2,3);
  new LineGraphWPF(0,2,x->Cos(10*x));
  new LineGraphWPF(0,2,x->Sqrt(x));
  new LineGraphWPF(0,2,x->Sin(10*x));
  new LineGraphWPF(0,2,x->exp(x));
end.
```





{% include links.html %}

