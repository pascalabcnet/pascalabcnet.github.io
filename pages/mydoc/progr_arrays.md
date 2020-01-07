---
title: Программы и алгоритмы с использованием массивов
keywords: arrays, examples, programs, algorithms
last_updated: 31.12.19
summary: "Здесь приведены примеры программ и алгоритмов, используемых в курсе Основы программирования для студентов 1 курса ФИИТ мехмата ЮФУ"
sidebar: mydoc_sidebar
permalink: progr_arrays.html
folder: mydoc
---

### Количество по условию
**Задача.** Сколько нечетных среди n введенных

**Решение 1.** Ввод данных в цикле
```pascal
var c := 0;
loop n do
begin
  var x := ReadInteger;
  if x mod 2 <> 0 then
    c += 1;
end;
```

1. Ввод-вывод
```pascal
  var a := ReadArrInteger(n);
  var r := ReadArrReal(n);
  Print(a);
  a.Println;
  a.Println(', ');
```

2. Заполнение
```pascal
  a := Arr(1,3,7);
  a := ArrFill(10,666);
  a := ArrGen(n,i->i*i[,from:=0])
  a := ArrGen(n,1,x->x+2)
  a := ArrGen(n,1,1,(x,y)->x+y)
```

3. Заполнение случайными числами
```pascal
  a := ArrRandomInteger(n[,from,to]);
  r := ArrRandomReal(n[,from,to]);
```

4. Срезы
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

5. Операции + и *
```pascal
  var a := Arr(1,2,3);
  var b := Arr(4,5,6);
  a + b // 1 2 3 4 5 6
  a * 2 // 1 2 3 1 2 3
  Arr(0)*3 + Arr(1)*3 // 0 0 0 1 1 1 
  (Arr(0) + Arr(1))*3 // 0 1 0 1 0 1
```

6. Инвертирование массива (есть стандартная Reverse(a)
или a[::-1])
```pascal
procedure Reverse<T>(a: array of T);
begin
  var n := a.Length;
  for var i:=0 to n div 2 - 1 do
    Swap(a[i],a[n-i-1]);
end;
```

7. Поиск (есть стандартная x in a)
```pascal
function FindIndex<T>(a: array of T; x: T): integer;
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

7а. Поиск по условию (есть стандартная a.FindIndex(cond))
```pascal
function FindIndex<T>
  (a: array of T; cond: T->boolean): integer;
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

7b. Поиск без использования break
```pascal
function FindIndexW<T>(a: array of T; x: T): integer;
begin
  var n := a.Length;
  var i := 0;
  while (i<n) and (a[i]<>x) do
    i += 1;
  Result := i=n ? -1 : i;
end;
```

7c. Поиск с барьером
```pascal
function FindIndexWithBarrier<T>
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

8. Количество (есть стандартная a.Count(условие))
```pascal
function Count<T>(a: array of T; cond: T->boolean): integer;
begin
  Result := 0;
  foreach var x in a do
    if cond(x) then
      Result += 1;
end;
```

9. Минимальный элемент и его индекс (a.Min, a.IndexMin)
```pascal
function MinElem(a: array of real): (real,integer); 
begin
  var (min, minind) := (a[0], 0);  
  for var i:=1 to a.Length-1 do
    if a[i]<min then
      (min, minind) := (a[i], i);
  Result := (min, minind)
end;
```

10. Сдвиг влево
```pascal
procedure ShiftLeft<T>(a: array of T);
begin
  for var i:=0 to a.Length-2 do
    a[i] := a[i+1];
  a[a.Length-1] := default(T);
end;
```

11. Сдвиг вправо
```pascal
procedure ShiftRight<T>(a: array of T);
begin
  for var i:=a.Length-1 downto 1 do
    a[i] := a[i-1];
  a[0] := default(T);
end;
```

12. Циклический сдвиг вправо
```pascal
procedure CycleShiftRight<T>(a: array of T);
begin
  var v := a.Last;
  for var i:=a.Length-1 downto 1 do
    a[i] := a[i-1];
  a[0] := v;  
end;
```

13. Слияние двух упорядоченных в один упорядоченный
// a,b упорядочены по возрастанию
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

14. Поиск в упорядоченном массиве (есть стандартная a.BinarySearch(x))
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



{% include links.html %}
