---
title: Версия 3.8.4
keywords: release notes, what's new, announcements, new features
last_updated: 07.05.23
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_8_4.html
toс: false
folder: mydoc
---

Новый файл теперь создается в папке файла из текущей вкладки

Ускорена функция Abs для целых

[cache] для одного параметра не оборачивает его в Tuple - за счет этого ускорена в 10 раз
В [cache] Tuple заменен на ValueTuple (ускорить кеширование функций с 2 параметрами и более примерно в 3 раза)

Плагин TeacherControlPlugin учительского контроля
Модули LightPT бесшумной легковесной проверки выполнения задач. Интегрированы Робот, Чертежник, задачник PT

Цветной вывод в окне вывода (Windows)

Появилась Linux-версия IDE PascalABC.NET (пока без отладчика)
В Alt Linux - rpm-пакет PascalABC.NET
GraphABC работает под Linux
Linux F1 - вызывается kchmviewer

School - Permutations, Cartesian, Combinations для строк
Graph3D LocalAxisX Y Z, MoveByLocal

Utils Benchmark


OnMouseWheel в GraphWPF.pas
x.Sqr возвращает int64

окно About - улучшено оформление и добавлена гипетрссылка на Telegram канал

Ковариантность (не вся)

Модуль Мозаика для дошкольников МозаикаABC.pas
модуль TurtleWPF.pas

Digits с основанием в School

Короткие создающие функции DictStr, DictInt, DictStrInt, LstLin, LstStr, HSetInt, HSetStr, SSetInt, SSetStr
и Dict для последовательностей пар

Convert = System.Convert
function Each<Key,Res>(Self: sequence of Key; proj: Key -> Res): Dictionary<Key,Res>; extensionmethod;

a.Cartesian(n) -> a.CartesianPower(n)
Timers: TimerProc -> OnTimer
GraphWPF - Arrow
GraphWPF TextOut и DrawText - параметр text сделан object - можно выводить и кортежи и автоклассы
TextOut(pos: Point

nuget - инсталляция сборок с учетом папки netstandard2.0
и копирование нативных dll из папок runtimes\win-x64\native\
-decimal

Sort(a,x->x) - ускорение

## Изменения в языке 3.8.4

### Именованные атрибуты при вызове подпрограмм
При вызове подпрограмм теперь можно использовать именованные атрибуты. Они могут менять значения параметров по умолчанию.

```pascal
procedure p(s: string; x: integer := 1; y: integer := 2; z: integer := 3);
begin
  Print(s,x,y,z);
end;

begin
  p('Hello', z := 33, y := 22);
end.
```
Вывод:
```pascal
Hello 1 22 33 
```



## Уточнения в языке

### Возможность описания подпрограмм в коротких программах 
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

### Распаковка ValueTuple
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

### Вывод в консоль - по умолчанию UTF-8

По умолчанию кодировка вывода в консоль заменена на UTF-8

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

