---
title: Версия 3.8.1
keywords: release notes, what's new, announcements, new features
last_updated: 21.08.21
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_8_1.html
toс: false
folder: mydoc
---

## Изменения в языке

### Атрибут [Cache] для кеширования результатов функции
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

### Возможность описания подпрограмм в коротких программах 
```pascal
##
function Add(a,b: integer) := a + b;

Print(Add(2,3));
```

### Директива {$string_nullbased+} и срезы
Директива {$string_nullbased+} теперь воздействует на срезы
```pascal
{$string_nullbased+}
begin
  var s := '0123456789';
  Print(s[0:5],s[5:8],s[8:]); // 01234 567 89
end.
```
### Единичное присваивание в лямбде в скобках

Ранее лямбда-процедуры с телом из одного присваивания необходимо было записывать с окаймляющим begin-end. Теперь достаточно взять присваивание в скобки:

```pascal
begin
  var f: procedure(x: integer);
  var sum := 0;
  f := x -> begin sum += x end; // ранее
  f := x -> (sum += x); // сейчас
end.  
```

### Распаковака ValueTuple
ValueTuple - типы теперь также можно распаковывать в переменные:
```pascal
begin
  var (n,s) := System.ValueTuple.Create(2,'ab');
  Print(n,s);
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

### Cтандартная Sort для массивов и списков с проекцией на ключ

В процедуре Sort для массивов и списков появилась перегруженная версия с дополнительным параметром - проекцией на ключ:

```pascal
##
var pp := |('Сидоров',20),('Петров',22), ('Попов',20),('Иванов',22)|;
Sort(pp, p->p[1]); // Сортировка по возрасту
pp.Print;
```

### Метод последовательности EachCount
Метод последовательности EachCount теперь можно вызывать, минуя GroupBy:
```pascal
##
var s := 'каракатица так и катится';
s.EachCount.Println
```
Результат:
```pascal
(к,4) (а,6) (р,1) (т,4) (и,3) (ц,1) ( ,3) (с,1) (я,1)
```

### Процедура Assert теперь не генерирует код в Release
```pascal
##
Assert(2*2=5)
```
Если мы снимем флаг Debug в настройках и запустим программу по Shift-F9, то ошибка выводиться не будет.
Можно убедиться, что в исполняемом файле код с Assert отсутствует

## Стандартные модули

### Стандартный модуль PlotWPF
Стандартный модуль PlotWPF предназначен для рисования графиков функций и множеств точек в системе координат

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

