---
title: Версия 3.6.0
keywords: release notes, announcements, what's new, new features
last_updated: 05.01.20
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_6.html
toс: false
folder: mydoc
---

## Новая синтаксическая конструкция для диапазона: `a..b`. 

Конструкция `a..b` задаёт диапазон элементов.

Значения a,b должны быть целыми или символьными.

В зависимости от контекста `a..b` переводится в различные конструкции. 

Операция `..` менее приоритетна чем сложение, но более приоритетна чем операция `in`.
То есть, следующие выражения можно писать без скобок:
```pascal 
x in 1..5
1+2 .. 3+4
```

В штатной ситуации `a..b` эквивалентно `Range(a,b)`:
```pascal  
(1..10).Select(x -> x*x).Println;
var a := Arr(1..10);
var h := HSet(1..10);
(1..5).Cartesian('a'..'z').Println;
```

Конструкция 
```pascal  
x in 1..10
```
переводится в 
```pascal
(x >= 1) and (x <= 10)
```

Конструкция 
```pascal  
foreach var x in 1..10 do
```
переводится в 
```pascal
for var x := 1 to 10 do
```

## Оптимизация скорости срезов строк a[f:t]

Подобные срезы оптимизированы заменой на вызов функции `s.Substring`

## Условная операция if

Реализована по аналогии с Алголом и Котлином вместо ? :
```pascal
var min := if a<b then a else b;
```
Это позволяет писать более понятный код со вложенными конструкциями:
```pascal
begin
  var (x,y) := (5,-3);
  var q := 
    if x>0 then
      if y>0 then
        1
      else 4
    else
      if y>0 then
        2
      else 3;
        
  Print(q)
end.
```
и
```pascal
begin
  var x := ReadReal;
  var sign := if x>0 then 1
    else if x=0 then 0
    else -1;
  Print(sign)
end.
```

## Стандартная функция ReadInt64

```pascal
var x := ReadInt64;
```


{% include links.html %}
