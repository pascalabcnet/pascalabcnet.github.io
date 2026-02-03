---
title: Версия 3.11.0
keywords: release notes, what's new, announcements, new features
last_updated: 31.08.25
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_11_0.html
toс: false
folder: mydoc
---


## Изменения в языке 3.11.0 (1.10.25)

### Оператор exit(param)
Теперь в функциях можно использовать exit(param) для моментального возврата значения
```pascal
function FastPower(a: real; n: integer): real;
begin
  if n = 0 then exit(1); // a^0 = 1
  if n = 1 then exit(a); // a^1 = a
  if n < 0 then exit(1 / FastPower(a, -n)); // отрицательная степень
  
  var half := FastPower(a, n div 2);
  if n mod 2 = 0 then exit(half * half); // чётная степень
  exit(half * half * a); // нечётная
end;
```

### Распаковка KeyValuePair
Значение типа KeyValuePair можно распаковать в переменные
```pascal
var t := 2 to 3.5;
  var (a,b) := t;
  Println(a,b);
  foreach var (a1,b1) in Dict(1 to 7, 22 to 555) do
    Println(a1,b1);
```

### Инициализация элемента массива пустой коллекцией
Элемент массива, который сам является массивом, теперь можно инициализировать пустой коллекцией
```pascal
begin
  var a: array of array of integer := [[],[],[1]];
  Println(a,TypeName(a));
  var b := [[],[1],[]];
  Println(b,TypeName(b));
  var aa: array of array of array of integer := [[[],[2]],[]];
  Print(aa,TypeName(aa));
end.
```




## Изменения в модулях

### Новый Модуль TurtleABC 
Модуль `TurtleABC` написан на Windows.Forms и поэтому может использоваться в Linux

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

### Ускорение встроенных множеств
Встроенные множества `set of T` ускорены примерно в 10 раз. Изменена их внутренняя реализация

## Разное

### Возможность трассировать программы с модулем PT4
Программы для модуля PT4 теперь можно трассировать

### Сборка с помощью dotnet
Проект теперь собирается при помощи современной утилиты dotnet

{% include links.html %}

