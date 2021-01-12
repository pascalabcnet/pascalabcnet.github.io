---
title: Версия 3.7.2 (будущая версия)
keywords: release notes, what's new, announcements, new features
last_updated: 24.08.20
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_7_2.html
toс: false
folder: mydoc
---

## Новые конструкции в языке

### Литералы для BigInteger

Литералы для BigInteger имеют окончание bi: `1bi`, `874658734265762345bi`

Использование 1:

```pascal
begin
  var n := ReadInteger;
  var p := 1bi;
  for var i:=2 to n do
    p *= i;
  Print(p);  
end.
```

Использование 2:

```pascal
##
Print(25bi ** 25 + 17bi ** 17)
```


## Стандартная библиотека

### Sum Average Product для последовательностей BigInteger

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
  var c2 := s[1 as CubeT;
  var c3 := s[2] as CubeT;
end.
```




{% include links.html %}

