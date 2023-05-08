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

При неявном приведении типов учитывается ковариантность параметров обобщений из стандартной библиотеки .NET. Это означает, что если параметр обобщения - ковариантный, то вместо него можно использовать любой производный тип

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

# Уточнения в языке

-decimal

## Linux версия

Появилась Linux-версия IDE PascalABC.NET (пока без отладчика)

В Alt Linux - rpm-пакет PascalABC.NET

GraphABC работает под Linux

Linux F1 - вызывается kchmviewer



## Новые стандартные модули

Модуль Мозаика для дошкольников МозаикаABC.pas

модуль TurtleWPF.pas

### LightPT

Плагин TeacherControlPlugin учительского контроля

Модуль LightPT бесшумной легковесной проверки выполнения задач. Интегрированы Робот, Чертежник, задачник PT



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

