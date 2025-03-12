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
Теперь по умолчанию константа [1,2,3] имеет тип массива. Именно так сейчас сделано в современном Delphi.
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
Пусть s и s1 принадлежат к типу `faststring`. 
Тип `faststring` расширен методами
`s.IndexOf(s1)` - индекс подстроки в строке или -1 если не найдена
`s.ToSequence()` - преобразование в последовательность символов. Сама faststring по историческим причинам последовательностью не является
Операции:
`s1 in s` - входит ли подстрока s1 в s
`s1 + s` - конкатенация быстрых строк 
`s * n` - конкатенация строки s с собой n раз
`s.Replace(s1, s2: string; n: integer)` - возвращает новую `faststring` где n первых вхождений `s1` заменены на `s2`.

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
```pascal
type
  Base = class
  end;
  Person = auto class(Base)
    name: string;
    age: integer;
  end;
begin
  var p := new Person('Иванов',18)
end.
```

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

