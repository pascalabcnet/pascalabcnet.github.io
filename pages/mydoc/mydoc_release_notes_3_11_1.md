---
title: Версия 3.11.1
keywords: release notes, what's new, announcements, new features
last_updated: 03.02.26
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_11_1.html
toс: false
folder: mydoc
---

## Изменения в версии 3.11.1 (03.02.26)

### Основное - реализован новый модуль DataFrameABC для промышленной работы с датасетами
Возможности DataFrameABC:
* Постолбцовое хранение данных (column-store) для высокой производительности
* Типизированные столбцы: int, float, string, bool с поддержкой NA
* Cursor API для эффективного построчного обхода без лишних аллокаций
* Фильтрация строк с пользовательским предикатом (`Filter`)
* Изменение схемы таблицы: `Select`, `Drop`, `Rename`
* Добавление вычисляемых столбцов: `WithColumn*`
* Группировка данных и агрегаты (`GroupBy`)
* Соединение таблиц: Inner / Left / Full Join (один и несколько ключей)
* Описательная статистика: `Count`, `Mean`, `Std`, `Min`, `Max`, `Describe`
* Загрузка данных из CSV и многострочного CSV-текста
* Удобный вывод больших таблиц с предпросмотром (`Print` / `PrintPreview`)

Семантика операций:
* `Select` / `Drop` / `Rename` → view (без копирования данных)
* `Filter` / `Join` / `Sort` / `WithColumn` → materialize (создаются новые массивы)
* `Count` считает только валидные (non-NA) значения

Пример:
```pascal
uses DataFrameABC;

begin
  var df := DataFrame.FromCsvText('''
  name,age,score
  Alice,20,85
  Bob,22,90
  Charlie,21,78
  Bob,22,90
  Clara,,78
  Kat,21,NA
  ''');
  
  df.Filter(row -> row.Int('age') > 20).Println;
  
  df.GroupBy('age').Mean('score').Println;

  var stat := df.Describe('score');
  Println($'score: count={stat.Count}, min={stat.Min}, max={stat.Max}, mean={stat.Mean}, std={stat.Std.Round(3)}');
end.
```

## Изменения в языке PascalABC.NET 3.11.1

### Контейнеры в интерполированных строках

Теперь в интерполированных строках в качестве значений можно использовать любые контейнеры - они будут интерполироваться до соответствующего значения
```pascal
begin
  Println($'{Arr(1,2,3)}');
  Println($'{Lst([1,2],[3,4])}');
  Println($'{SetOf(1,2,3)}');
end.
```
Будет выведено
```
[1,2,3]
[[1,2],[3,4]]
{1,2,3}
```

### Паскалевский спецификатор формата данных в Println
В Print - Println, так же как и в Write, можно теперь использовать паскалевские спецификаторы формата данных
```pascal
  Print(Pi:0:4)
```

## Изменения в стандартных функциях

### ArrRandomInteger и MatrRandomInteger с диапазоном
Функции ArrRandomInteger и MatrRandomInteger могут теперь принимать диапазон, что воспринимается естественнее чем 2 параметра:
```pascal
begin
  var a := ArrRandomInteger(10,5..10);
  a.Println;
  var m := MatrRandomInteger(3,4,2..9);
  m.Println(3);
end.
```

### Параметр Digits в методе ToString для массива вещественных
При преобразовании масива вещественных в строку можно указать, сколько цифр после десятичной точки учитывается. Это аналогично методу ToString для индивидуального значения типа real:
```pascal
begin
  var a := ArrRandomReal(5);
  var s := a.ToString(1);
  Println(s);
  var r: real := 3.14;
  s := r.ToString(1);
  Println(s);
end.
```

Вывод:
```
1.3 1.5 0.4 9.8 1.3
3.1
```

### Параметр allowOverlap в методе CountOf
В методе CountOf для последовательностей появился необязательный параметр allowOverlap, позволяющий учесть наложения 
```pascal
begin
  var s := 'arara';
  s.CountOf('ara').Println;
  s.CountOf('ara',allowOverlap := True).Println;
end.
```

Вывод:
```
1
2
```

## Новые стандартные функции и методы

### Кубический корень - работает и с отрицательными числами
```pascal
Print(CbRt(-27));
```

### Метод Copy для массивов и матриц
Для клонирования массивов и матриц можно теперь использовать стандартный метод `Copy`
```pascal
begin
  var a := [1,3,5];
  var a1 := a.Copy;
  Println(a1);
  var m := Matr([2,4,6],[3,5,8]);
  var m1 := m.Copy;
  Println(m1);
end.
```

### Стандартные функции GCD, GCM
Функции нахождения наибольшего общего делителя и наименьшего общего кратного теперь - в стандартном модуле
```pascal
##
var (a,b) := (56,98);
 
var GCD := GCD(a, b);
var LCM := LCM(a, b);
```

### Стандартная функция Hypot
Стандартная функция Hypot позволяет находить гипотенузу по двум катетам. ее реализация - это не наивное Sqrt(a*a + b*b) и позволяет избежать переполнения при больших a или b
```pascal
##
Hypot(3,4).Println;
Hypot(6,5).Println;
```



## Изменения в модулях

### Новый Модуль Coords
Модуль `Coords` позволяет отображать координатную сетку, масштабировать её колёсиком мыши и перемещать её центр левой мышью.

Он содержит ряд простых примитивов, таких как `DrawPoint`, `DrawPoints`, `DrawLine`, `DrawRectangle`, `DrawText`, `DrawTextUnscaled`.

```pascal
uses Coords;

function RandomPoint: Point 
  := Pnt(Random(-13,13),Random(-10,10));

begin
  DrawPoints(ArrGen(10,i -> RandomPoint),PointRadius := 4);
  DrawPoints(ArrGen(10,i -> RandomPoint),PointRadius := 6);
  DrawPoint(2,3,Colors.Red);
  DrawCircle(1,1,1,Colors.LightBlue);
  DrawRectangle(3,2,2,1);
  DrawText(3,2,'Hello');
  DrawTextUnscaled(0,0,'Текст не масштабируется', Size := 20, Color := Colors.Red);
  DrawText(-4,7,'Текст масштабируется', FontName := 'Courier New', Size := 34);
end.
```

### Новые стандартные методы последовательностей
Реализованы новые стандартные методы расширения последовательностей `IndexMinBy`, `IndexMaxBy`, `DistinctBy`.

### Новые методы множеств
Для новой реализации встроенных множеств реализованы методы `Add`, `AddRange`, `Remove` и `Contains`.

## Оптимизация производительности

### Ускорен CountOf
Оптимизирован и ускорен метод CountOf для строк

## Разное

### Возможность трассировать программы с модулем PT4
Программы для модуля PT4 теперь можно трассировать

### Сборка с помощью dotnet
Проект теперь собирается при помощи современной утилиты dotnet

{% include links.html %}

