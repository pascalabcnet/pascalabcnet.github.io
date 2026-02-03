---
title: Версия 3.11.0
keywords: release notes, what's new, announcements, new features
last_updated: 31.08.25
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_11_0.html
toс: false
folder: mydoc
---

# Изменения в версии 3.11.0 (1.10.25)

## Основное - реализован язык SPython - статическая реализация языка Python
В инфраструктуре PascalABC.NET реализован язык SPython - подмножество языка Python со статической типизацией

Файлы имеют расширение `.pys`, программы работают с той же скоростью, что и программы на PascalABC.NET и C# и в штатных сценариях (без вызова библиотечных функций) - примерно в 100 раз быстрее Python - программ.

Также реализована поддержка многоязыковости в инфраструктуре PascalABC.NET - от компилятора, таблицы символов и системы модулей до поддержки в оболочке (например, Intellisense). Пока отсутствует поддержка дебаггера.

```python
# Hello.pys
print('Привет! Это SPython!')
```

## Новые конструкции в языке

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

### Новый стандартный метод последовательностей IsOrdered и его производные
```pascal
type
  Player = auto class
    Name: string; Points: integer;
  end;  

function GetPlayers := [
  new Player('Alice', 120),
  new Player('Bob', 95),
  new Player('Charlie', 130),
  new Player('Diana', 110)
];

begin
  var leaderboard := GetPlayers;
  Println(leaderboard);
  if not leaderboard.IsOrderedByDescending(p -> p.Points) then
    Println('Рейтинг составлен неверно!');

  leaderboard := leaderboard.OrderByDescending(p -> p.Points).ToArray;
  Println(leaderboard);
  if leaderboard.IsOrderedByDescending(p -> p.Points) then
    Println('Теперь рейтинг составлен верно!');
end.
```


## Устранение багов
Устранен ряд утечек памяти в Intellisense 

## AbcObjects для Linux
Модуль AbcObjects адаптирован для работы в Mono под Linux

{% include links.html %}

