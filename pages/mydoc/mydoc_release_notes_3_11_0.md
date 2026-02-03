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

## Изменения в стандартоном модуле

### Внешние функции Between и InRange

Внешние функции Between и InRange реализованы для любого типа, удовлетворяющего IComparable<T>

```pascal
begin
  Between(6,2,6).Print;
  Between(6,2,6,inclusive := False).Println;
  var dt1 := DateTime.Create(2025,04,16);
  var dt3 := DateTime.Now;
  Between(DateTime.Now,dt1,dt3).Print; 

  // Поведение изменено!! Теперь диапазон [6,2] считается пустым
  InRange(5,2,6).Println;
end.
```

### Новый стандартный метод последовательностей AdjacentGroupBy
`AdjacentGroupBy` группирует соседние одинаковые по ключу элементы. Возвращает последовательность групп
```pascal
type
  Player = auto class
    Name: string; Score: integer;
  end;  

function GetPlayers := [
  new Player('Alice', 95),
  new Player('Bob', 95),
  new Player('Charlie', 110),
  new Player('Diana', 110),
  new Player('Karina', 120)
];

begin
  var leaderboard := GetPlayers;
  foreach var group in leaderboard.AdjacentGroupBy(p -> p.Score) do
  begin
    Println(group.Key);
    group.Println;
  end;
end.
```

## Устранение багов
Устранен ряд утечек памяти в Intellisense 

## AbcObjects для Linux
Модуль AbcObjects адаптирован для работы в Mono под Linux

## Разное

### Возможность трассировать программы с модулем PT4
Программы для модуля PT4 теперь можно трассировать

### Сборка с помощью dotnet
Проект теперь собирается при помощи современной утилиты dotnet

{% include links.html %}

