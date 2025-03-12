---
title: Версия 3.10.3
keywords: release notes, what's new, announcements, new features
last_updated: 12.03.25
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_10_3.html
toс: false
folder: mydoc
---


## Изменения в языке 3.10.3 (12.03.25)

### Синтаксис [] по умолчанию для массивов
Теперь по умолчанию константа [1,2,3] имеет тип массива
```pascal
var a := [1,2,3]; // array of integer
var s: set of integer := [1,2,3]; // set of integer
var s: set of byte := [1,2,3]; // set of byte
```


### Инициализатор пустой коллекции []
Инициализатор [] пустой коллекции позволяет присваивать его любой коллекции, заменяя вызов конструктора по умолчанию.

Особенно удобно использовать [] для передачи в качестве параметра - тип формального параметра уже указан при описании, а при вызове в параметрах мы всего лишь указываем [].
```pascal
procedure ppp(s: sequence of integer; a: array of integer; lst: List<(real,real)>;
  d: Dictionary<string,integer>; hs: HashSet<integer>; ss: SortedSet<integer>);
begin end;

begin
  var old: set of integer := [];
  var s: sequence of integer := [];
  var a: array of integer := [];
  var lst: List<(real,real)> := [];
  var d: Dictionary<string,integer> := [];
  var st: Stack<string> := [];
  var q: Queue<string> := [];
  var hs: HashSet<integer> := [];
  var ss: SortedSet<integer> := [];
  
  ppp([],[],[],[],[],[]);
  
  var aa: array of array of integer := [[1],[],[2,3]];  
end.
```


### Новый тип faststring - синоним StringBuilder
IndexOf, ToSequence, 
in, +, *n
.Replace(Self: faststring; OldValue, NewValue: string; n: integer): faststring

```pascal
begin
  var mx := 0;
  for var n := 4 to 9999 do
  begin
    var s: faststring := '4' + '1' * n;

    while ('411' in s) or ('1111' in s) do
      s.Replace('411', '14', 1).Replace('1111', '1', 1);
    
    mx := max(mx, s.ToString.Sum(d -> d.todigit));
  end;
  print(mx, Milliseconds / 1000);
end.
```

### Автоклассы могут наследоваться от классов без полей

### Кортежи неявно могут преобразовываться покомпонентно
```pascal
begin
  var t: (real,real);
  t := (1,2);
  
  var t1: ((real,integer),real);
  t1 := ((3,4),5);
end.
```

## Изменения в модулях

### Новый Модуль Coords
```pascal

```

### Модуль Turtle - изменение реализации

### Модуль LightPT - ряд изменений

### Новые стандартные методы последовательностей
Реализованы новые стандартные методы расширения последовательностей `IndexMinBy`, `IndexMaxBy`, `DistinctBy`.

### Новые методы множеств
Для новой реализации встроенных множеств реализованы методы `Add`, `AddRange`, `Remove` и `Contains`.

## Оптимизация производительности

## Ускорение встроенных множеств
Встроенные множества `set of T` ускорены примерно в 10 раз. Изменена их внутренняя реализация

Поскольку синтаксис `[1,2,3]` по умолчанию отведен для массивов, то теперь для создания встроенных множеств используется `SetOf(1,2,3)`

## Другое

### Возможность трассировать программы с модулем PT4
Программы для модуля PT4 теперь можно трассировать

### Сборка с помощью dotnet
Проект теперь собирается при помощи современной утилиты dotnet

{% include links.html %}

