---
title: Версия 3.7.2
keywords: release notes, what's new, announcements, new features
last_updated: 12.01.21
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_7_2.html
toс: false
folder: mydoc
---

## Новые конструкции в языке

### Расширенный foreach

foreach с распаковкой значений в несколько переменных. Значения должны быть кортежами или последовательностями

```pascal
begin
  var a := Arr((1,2),(3,4),(5,6));
  foreach var (x,y) in a do
    Print(x,y);
  Println;
  var b := Arr(|1,2,3|,|4,5|,|6,7,8,9|);
  foreach var (x,y) in b do
    Print(x,y);
end.
```


### Литералы для BigInteger

Литералы для BigInteger имеют окончание bi: `1bi`, `874658734265762345bi`

Пример 1

```pascal
begin
  var n := ReadInteger;
  var p := 1bi;
  for var i:=2 to n do
    p *= i;
  Print(p);  
end.
```

Пример 2

```pascal
##
Print(25bi ** 25 + 17bi ** 17)
```

### Использование uses в коротких программах

Пример 1

```pascal
##
uses Graph3D;
Sphere(Origin,1);
```

Пример 2

```pascal
###
uses School;
Pr(Bin(123));
```

## Стандартная библиотека

### Размещения и размещения с повторениями

В дополнение к методам a.Permutations и a.Combinations(m) для массивов реализованы

a.Cartesian(n) - Возвращает n-тую декартову степень множества элементов

a.Permutations(m) - Возвращает все частичные перестановки из n элементов по m

Кроме того, все указанные методы определены также над последовательностями

```pascal
##
var a := Arr(1,3,5,7);
a.Permutations.Println;
a.Cartesian(2).Println;
a.Permutations(2).Println;
a.Combinations(2).Println;
Println;
var s := Seq(1,3,5,7);
s.Permutations.Println;
s.Cartesian(2).Println;
s.Permutations(2).Println;
s.Combinations(2).Println;
```

### s.CountOf(x) для последовательностей

```pascal
###
var a := Arr(1,3,5,7,1,2,1,3,1,5);
a.CountOf(1).Print
```

### Sum, Average, Product для последовательностей BigInteger

```pascal
begin
  var s := SeqGen(100,i->BigInteger(i)**i);
  Print(s.Sum,s.Product);
end.
```

### Методы расширения строк s.IsInteger и s.IsReal:

```pascal
begin
  var s := '123.4 3 5 6.6 a v 67';
  var (si,sr) := (0,0.0);
  foreach var w in s.ToWords do
    if w.IsInteger then
      si += w.ToInteger
    else if w.IsReal then
      sr += w.Toreal;
  Print(si,sr);  
end.
```

### s.ToWords(delims) с разделителями в виде строки

Разделители в s.ToWords теперь можно задавать в виде строки

```pascal
begin
  var s := '123.4, 6.6, 67';
  s.ToWords(' ,').PrintLines
end.
```

Кроме того, определена константа AllDelimiters, содержащая все разделители слов в текстах
```pascal
begin
  ...
  s.ToWords(AllDelimiters).Println
end.
```

## Graph3D

### Сериализация и десериализация компонент Object3D 

Сериализация
```pascal
uses Graph3D;

begin
  var s := Sphere(0,0,0,1);
  s.AddChild(Cube(0,0,1,0.5));
  s.AddChild(Cube(1,0,0,0.5));
  s.AddChild(Cube(-1,0,0,0.5));
  s.Serialize('c.dat');
end.
```

Десериализация
```pascal
uses Graph3D;

begin
  var s := Object3D.DeSerialize('c.dat') as SphereT;
  var c1 := s[0] as CubeT;
  var c2 := s[1] as CubeT;
  var c3 := s[2] as CubeT;
end.
```




{% include links.html %}

