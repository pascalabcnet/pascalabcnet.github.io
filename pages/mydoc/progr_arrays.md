---
title: Программы и алгоритмы с использованием массивов
keywords: arrays, examples, programs, algorithms
last_updated: 31.12.19
summary: "Здесь приведены примеры программ и алгоритмов, используемых в курсе Основы программирования для студентов 1 курса ФИИТ мехмата ЮФУ"
sidebar: mydoc_sidebar
permalink: progr_arrays.html
folder: mydoc
---

## Методы и операции для работы с массивами

Объявление и выделение памяти 
```pascal
  var n := ReadInteger;
  var a: array of integer;
  a := new integer[n];
```
или 
```pascal
  var n := ReadInteger;
  var a := new integer[n];
```

Ввод-вывод
```pascal
  var a := ReadArrInteger(n);
  var r := ReadArrReal(n);
  Print(a);
  a.Println;
  a.Println(', ');
```

Заполнение
```pascal
  a := Arr(1,3,7);
  a := Arr(1..10);
  a := ArrFill(10,666);
  a := ArrGen(n,i->i*i[,from:=0])
  a := ArrGen(n,1,x->x+2)
  a := ArrGen(n,1,1,(x,y)->x+y)
```

Заполнение случайными числами
```pascal
  a := ArrRandomInteger(n[,from,to]);
  r := ArrRandomReal(n[,from,to]);
```

Срезы
```pascal
  var a := Arr(0,1,2,3,4,5,6,7,8,9);
  a[2:5]    // 2 3 4 
  a[:2]     // 0 1
  a[5:]     // 5 6 7 8 9 
  a[:]      // копия массива a
  a[::2]    // 0 2 4 6 8
  a[1::2]   // 1 3 5 7 9
  a[5:2:-1] // 5 4 3
  a[::-1]   // 9 8 7 6 5 4 3 2 1 0
```

Операции + и *
```pascal
  var a := Arr(1,2,3);
  var b := Arr(4,5,6);
  a + b // 1 2 3 4 5 6
  a * 2 // 1 2 3 1 2 3
  Arr(0)*3 + Arr(1)*3 // 0 0 0 1 1 1 
  (Arr(0) + Arr(1))*3 // 0 1 0 1 0 1
```

## Циклы по массиву

**For**
```pascal
for var i:=0 to a.Length-1 do
  a[i] += 1;
```

**Foreach**
```pascal
foreach var x in a do
  Print(x);
```

**Foreach по индексам**
```pascal
foreach var i in a.Indices do
  a[i] *= 2;
```

**Foreach по определённым индексам**
```pascal
foreach var i in a.Indices(x -> x in 10..20) do
  a[i] += 1;
```

```pascal
foreach var i in a.Indices((x,i) -> i.IsEven and (x > 0)) do
  a[i] += 1;
```

## Инвертирование

**Задача.** Инвертировать массив

**Решение 1.** Алгоритм
```pascal
procedure Reverse<T>(a: array of T);
begin
  var n := a.Length;
  for var i:=0 to n div 2 - 1 do
    Swap(a[i],a[n-i-1]);
end;
```

**Решение 2.** С помощью срезов
```pascal
a := a[::-1]
```

**Решение 3.** С помощью стандартной процедуры
```pascal
Reverse(a)
```

## Поиск

**Задача.** Есть ли в массиве a элемент x

**Решение.** С помощью операции in или метода Contains:
```pascal
x in a
a.Contains(x)
```

**Задача.** Найти индекс первого вхождения элемента x

**Решение 1.** С использованием break
```pascal
function IndexOf<T>(a: array of T; x: T): integer;
begin
  Result := -1;
  for var i := 0 to a.High do // a.High = a.Length - 1
    if a[i] = x then
    begin
      Result := i;
      break;
    end;
end;
```

**Решение 2.** Без использования break
```pascal
function IndexOfW<T>(a: array of T; x: T): integer;
begin
  var n := a.Length;
  var i := 0;
  while (i<n) and (a[i]<>x) do
    i += 1;
  Result := i=n ? -1 : i;
end;
```

**Решение 3.** Поиск с барьером

Добавим в конец массива барьер, равный x. В массиве должно быть место под этот элемент

```pascal
function IndexOfWithBarrier<T>
  (a: array of T; n: integer; x: T): integer;
begin
  Assert((0 < n) and (n < a.Length));
  a[n] := x; // барьер
  var i := 0;
  while a[i]<>x do
    i += 1;
  Result := i=n ? -1 : i;
end;
```

За счет использования барьера экономится одна операция сравнения

**Решение 4.** Стандартные методы
```pascal
a.IndexOf(x)
a.LastIndexOf(x)
```


## Поиск по условию

**Задача.** Поиск по условию 

**Решение 1.** Алгоритм
```pascal
function FindIndex<T>(a: array of T; cond: T->boolean): integer;
begin
  Result := -1;
  for var i := 0 to a.High do
    if cond(a[i]) then
    begin
      Result := i;
      break;
    end;
end;
```

**Решение 2.** С помощью стандартной функции
```pascal
a.FindIndex(cond)
```

## Количество по условию

**Задача.** Количество элементов, удовлетворяющих заданному условию

**Решение 1.** Алгоритм

```pascal
function Count<T>(a: array of T; cond: T->boolean): integer;
begin
  Result := 0;
  foreach var x in a do
    if cond(x) then
      Result += 1;
end;
```

**Решение 2.** С помощью стандартной функции
```pascal
a.Count(условие)
```

## Минимумы-максимумы

**Задача.** Найти минимальный элемент и его индекс 

**Решение 1.** Алгоритм
```pascal
function MinElemAndIndex(a: array of real): (real,integer); begin
  var (min, minind) := (real.MaxValue, -1);  
  for var i:=0 to a.Length-1 do
    if a[i]<min then
      (min, minind) := (a[i], i);  Result := (min, minind)
end;
```

**Решение 2.** С помощью стандартной функции
```pascal
a.Min
a.IndexMin
```

**Задача.** Найти минимальный элемент, удовлетворяющий условию, и его индекс 

**Решение.** Алгоритм
```pascal
function MinElemAndIndexCond(a: array of real: cond: real -> boolean): 
  (real,integer); 
begin
  var (min, minind) := (real.MaxValue, -1);  
  for var i:=0 to a.Length-1 do
    if (a[i]<min) and cond(a[i]) then
      (min, minind) := (a[i], i);
  Result := (min, minind)
end;
```


## Сдвиги

**Задача.** Сдвиг влево на 1

**Решение 1.** Алгоритм
```pascal
procedure ShiftLeft<T>(a: array of T);
begin
  for var i:=0 to a.Length-2 do
    a[i] := a[i+1];
  a[a.Length-1] := default(T);
end;
```

**Решение 2.** С помощью срезов
```pascal
a := a[1:] + Arr(0);
```

**Задача.** Сдвиг вправо

**Решение 1.** Алгоритм
```pascal
procedure ShiftRight<T>(a: array of T);
begin
  for var i:=a.Length-1 downto 1 do
    a[i] := a[i-1];
  a[0] := default(T);
end;
```

**Решение 2.** С помощью срезов
```pascal
a := Arr(0) + a[:a.Length-1];
```

**Задача.** Циклический сдвиг вправо

**Решение 1.** Алгоритм
```pascal
procedure CycleShiftRight<T>(a: array of T);
begin
  var v := a.Last;
  for var i:=a.Length-1 downto 1 do
    a[i] := a[i-1];
  a[0] := v;  
end;
```

**Решение 2.** С помощью срезов
```pascal
var m := a.Length-1;
a := a[m:] + a[:m];
```

**Задача.** Циклический сдвиг влево на k

**Решение 1.** С помощью срезов
```pascal
a := a[k:] + a[:k];
```

**Решение 2.** С помощью частичного Reverse
```pascal
Reverse(a,0,k);
Reverse(a,k,a.Length-k);
Reverse(a);
```

## Преобразование элементов

**Задача.** Требуется преобразовать элементы массива по правилу x -> f(x)

**Решение 1.** Алгоритм
```pascal
procedure Transform<T>(a: array of T; f: T -> T);
begin
  for var i:=0 to a.Length-1 do
    a[i] := f(a[i]);
end;
```

**Решение 2.** С помощью стандартной функции
```pascal
a.Transform(x -> x*x)
```

Для преобразования части элементов:

```pascal
a.Transform(x -> if x mod 2 = 0 then x*x else x)
```

## Слияние

**Задача.** Слияние двух упорядоченных массивов в один упорядоченный

В массивах должно быть место под один барьерный элмент

```pascal
function Merge(a,b: array of real; n,m: integer): 
  array of real;
begin
  Assert((0 < n) and (n < a.Length));
  Assert((0 < m) and (m < b.Length));
  a[n] := real.MaxValue; // барьер
  b[m] := real.MaxValue; // барьер
  SetLength(Result,m+n);
  var (ia,ib) := (0,0);
  for var ir:=0 to n+m-1 do
    if a[ia]<b[ib] then
    begin
      Result[ir] := a[ia]; 
      ia += 1;
    end
    else
    begin
      Result[ir] := b[ib]; 
      ib += 1;
    end;
end;
```

## Бинарный поиск

**Задача.** Поиск в упорядоченном массиве (есть стандартная a.BinarySearch(x))
```pascal
function BinarySearch(a: array of integer; x: integer): integer;
begin
  var k: integer;
  var (l,r) := (0, a.Length-1); 
  repeat
    k := (l+r) div 2;
    if x>a[k] then
      l := k+1
    else r := k-1;
  until (a[k]=x) or (l>r);
  Result := a[k]=x ? k : -1;
end;
```

Асимптотическая сложность - O(log(n))

## Алгоритмы сортировки

### Сортировка выбором

```pascal
procedure SortByChoice(a: array of integer);
begin
  for var i := 0 to a.High-1 do
  begin
    var min := a[i];
    var imin := i;
    for var j := i + 1 to a.High do
      if a[j] < min then
      begin        
        min := a[j];
        imin := j;
      end;  
    Swap(a[imin],a[i]);
  end;
end;
```

С использованием срезов:

```pascal
procedure SortByChoice(a: array of integer);
begin
  for var i := 0 to a.High-1 do
    Swap(a[a[i:].IndexMin + i],a[i]);
end;
```

Асимптотическая сложность – Θ(𝑛<sup>2</sup>)

### Пузырьковая сортировка 

```pascal
procedure BubbleSort(a: array of integer);
begin
  for var i := 0 to a.High-1 do
    for var j := a.High downto i+1 do
      if a[j] < a[j-1] then
        Swap(a[j], a[j-1]);
end;
```

Асимптотическая сложность – Θ(𝑛<sup>2</sup>)


С флагом (эффективнее в ситуациях, когда массив частично отсортирован):

```pascal
procedure BubbleSort2(a: array of integer);
begin
  var i := a.High;
  var q: boolean;
  repeat
    q := true;
    for var j := 0 to i - 1 do
      if a[j+1] < a[j] then
      begin
        Swap(a[j+1], a[j]);
        q := false;
      end;
    i -= 1;
  until q;
end;
```

Асимптотическая сложность – Θ(𝑛<sup>2</sup>) в среднем и Θ(𝑛) в лучшем случае (когда массив отсортирован).


### Сортировка вставками

```pascal
procedure SortByInsert(a: array of integer);
begin
  for var i:=1 to a.High do
  begin
    var x := a[i];
    var j := i - 1;
    while (j >= 0) and (x < a[j]) do
    begin
      a[j+1] := a[j];
      j -= 1;
    end;
    a[j+1] := x;
  end;
end;
```

Асимптотическая сложность – Θ(𝑛<sup>2</sup>)

### Стандартная сортировка
```pascal
Sort(a);
```

Асимптотическая сложность – Θ(𝑛∙log⁡(𝑛))


## Методы и операции для работы cо списками List<T>

**Список** List<T> – это динамический массив с возможностью динамического изменения размеров по ходу работы программы.

Объявление списка:
```pascal
var l := new List<integer>;
```
  
Добавление элементов в конец списка:
```pascal
l.Add(5); // список расширяется, выделяя память под новые элементы
l.Add(3);
l.Add(4);
l += 8; // синоним l.Add(8)
```

Цикл по списку:
```pascal
for var i:=0 to l.Count-1 do
  Print(l[i]);
foreach var x in l do
  Print(x);
```

Заполнение списка:
```pascal
var l := Lst(5,2,3); 
var l1 := Lst(1..10); 
```

Операции со списками:
```pascal
var l := Lst(5,2,3); 
Print(2 in l); // True
Print(2 * l); // [5,2,3,5,2,3]
l.Print; // 5 2 3
Print(l + Lst(7,6,8)); // 5 2 3 7 6 8
```

Методы списков:
```pascal
l.Insert(ind,x); // вставка x по индексу ind
l.RemoveAt(ind); // удаление элемента по индексу ind
l.RemoveRange(ind,count) // удаление диапазона элементов
l.RemoveAll(x -> x.IsOdd); // возвращает число удалённых элементов
l.IndexOf(3); // индекс первого вхождения или -1
l.FindIndex(x -> x > 4); // индекс первого вхождения или -1
l.Clear; // очищает список
l.Reverse; // инвертирует список
l.Sort; // сортирует список
```

## Реализация функции Distinct

**Задача.** Реализовать функцию Distinct, по заданному массиву возвращающая список только различных элементов массива

**Решение.** Алгоритм

```pascal
function Distinct(a: array of T): List<T>;
begin
  Result := new List<T>;
  foreach var x in a do
    if not (x in Result) then
      Result += x;
end;
```

## Вставка и удаление элементов в массиве и списке

**Задача.** Дан массив (список) N вещественных. Требуется вставить элемент x на k-тое место (начиная с 0), k<=N.

**Решение.** В массиве - с помощью срезов:

```pascal
var N := ReadInteger;
var a := ReadArrInteger(N);
var l := Lst(a);
var k := ReadInteger;
a := a[:k] + Arr(x) + a?[k:]; 
```

**Решение.** В списке - с помощью стандартного метода:
```pascal
l.Insert(k,x);
```

**Задача.** Дан массив (список) N вещественных. Требуется удалить элемент с индексом k, k<N.

**Решение.** В массиве - с помощью срезов:

```pascal
var N := ReadInteger;
var a := ReadArrInteger(N);
var l := Lst(a);
var k := ReadInteger;
a := a[:k] + a?[k+1:]; 
```

**Решение.** В списке - с помощью стандартного метода:
```pascal
l.RemoveAt(k);
```




{% include links.html %}
