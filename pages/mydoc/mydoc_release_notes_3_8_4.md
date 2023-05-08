---
title: Версия 3.8.4
keywords: release notes, what's new, announcements, new features
last_updated: 07.05.23
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_8_4.html
toс: false
folder: mydoc
---


## Изменения в языке 3.8.4

### Ковариантность параметров обобщенных типов

При неявном приведении типов учитывается ковариантность параметров обобщений из стандартной библиотеки .NET. Это означает, что если параметр обобщения - ковариантный, то вместо него можно использовать любой производный тип. Например:

```pascal
begin
  var a: sequence of object;
  var b: sequence of integer;
  a := b;
  a := Arr(1,2,3);
  a := new List<DateTime>;
end.
```

### Именованные аргументы при вызове подпрограмм
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

## Linux версия

Появилась Linux-версия IDE PascalABC.NET (пока без отладчика). В качестве просмотровщика справки используется kchmviewer.

В Alt Linux 10.1 PascalABC.NET включен в репозиторий

Из графических модулей под Linux работает GraphABC

## Новые стандартные модули

### Модуль Мозаика для дошкольников

Пример программы:

```pascal
uses Мозаика;

begin
  Window.Title := 'цыплёнок';  

  НарисоватьРяд(П*9+кк2*2);
  НарисоватьРяд(П*9+ж*2+жк2);
  НарисоватьРяд(П*9+ж*2+ч+кк2);
  НарисоватьРяд(П*6+жк2+П*2+жк4+ж+жк3);
  НарисоватьРяд(П*5+жк2+ж+жк2+П*2+ж);
  НарисоватьРяд(П*5+ж*6);
  НарисоватьРяд(П*5+жк4+ж*5);
  НарисоватьРяд(П*6+жк4+ж*3+жк3);
  НарисоватьРяд(П*3+бк1+бк2+П*2+ж+П+ж);
  НарисоватьРяд(П*3+б*2+П*2+о+п+о);
  НарисоватьРяд(П*3+бк4+бк3+П*2+о+ок2+о+ок2);
  НарисоватьРяд(З*15);
end.
```
### Модуль TurtleWPF, реализующий черепашью графику с использованием GraphWPF

Пример программы:

```pascal
uses TurtleWPF;

begin
  Down;
  SetSpeed(11);
  SetColor(Colors.Red);
  for var i:=1 to 450 do
  begin
    SetColor(RGB(128+i,0,i));
    Forw(i);
    Turn(96);
  end;
end.
```

### Модуль LightPT бесшумной легковесной автоматической проверки заданий

Модуль LightPT обеспечивает бесшумную легковесную автоматическую проверку заданий, выполняемых учащимися.

Для его работы в папке должен находиться текстовый файл lightpt.dat, единственная строка в котором - имя урока.

Также в папке должен находиться файл Tasks.pas, осуществляющий легковесную проверку заданий. Его примерное содержимое:

```pascal
unit Tasks;

{$savepcu false}

uses LightPT;

procedure CheckTaskT(name: string);
begin
  case name of
   'Simple2': begin 
      FilterOnlyNumbers;
      CheckInput(Arr(cInt)*2);
      CheckOutputNew(Int(0)+Int(1));
   end;
  'MinMax': begin 
    CheckInputCount(2);
    FilterOnlyNumbers;
    CheckOutputNew(Min(Int(0),Int(1)),Max(Int(0),Int(1)))
   end;
   'ArrSum': begin 
    CheckInput(Arr(cInt)*10); // Проверка типов и количества введенных элементов
    FilterOnlyNumbers;        // Фильтрация только чисел – на случай вывода строк-подсказок 
    var a := IntArr(10);      // Считывание введенного массива целых
    var out := new ObjectList; // Подготовка списка для вывода
    out.AddRange(a);           // Добавление в него всех элементов массива a 
    out.Add(a.Where(x -> x mod 2 = 0).Sum); // Добавление в него суммы четных
    CheckOutputSeqNew(out);   // Проверка совпадения с выводом ученика      
    end;
  end;
end;

initialization
  CheckTask := CheckTaskT;
finalization
end.
```



В модуль LightPT интегрированы Робот, Чертежник, задачник PT



## Изменения в модулях

Цветной вывод в окне вывода (Windows)

School - Permutations, Cartesian, Combinations для строк

Graph3D LocalAxisX Y Z, MoveByLocal

Utils Benchmark

OnMouseWheel в GraphWPF.pas

x.Sqr возвращает int64

Sort(a,x->x) - ускорение

Digits с основанием в School

Короткие создающие функции DictStr, DictInt, DictStrInt, LstLin, LstStr, HSetInt, HSetStr, SSetInt, SSetStr
и Dict для последовательностей пар

Convert = System.Convert

function Each<Key,Res>(Self: sequence of Key; proj: Key -> Res): Dictionary<Key,Res>; extensionmethod;

a.Cartesian(n) -> a.CartesianPower(n)

Timers: TimerProc -> OnTimer

GraphWPF - Arrow

GraphWPF TextOut и DrawText - параметр text сделан object - можно выводить и кортежи и автоклассы

GraphWPF: TextOut(pos: Point

## Оптимизация

Ускорена функция Abs для целых

[cache] для одного параметра не оборачивает его в Tuple - за счет этого ускорена в 10 раз

В [cache] Tuple заменен на ValueTuple (ускорить кеширование функций с 2 параметрами и более примерно в 3 раза)

## Оболочка

Новый файл теперь создается в папке файла из текущей вкладки

Окно About - улучшено оформление и добавлена гипетрссылка на Telegram канал

nuget - инсталляция сборок с учетом папки netstandard2.0
и копирование нативных dll из папок runtimes\win-x64\native\








{% include links.html %}

