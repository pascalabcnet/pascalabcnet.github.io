---
title: Версия 3.6.0
keywords: release notes, announcements, what's new, new features
last_updated: 05.01.20
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_6.html
tok: false
folder: mydoc
---

## Версия 3.6.0. Предстоящие изменения

### Новая синтаксическая конструкция для диапазона: `a..b`. 

Взята из языка Kotlin. Значенния a,b должны быть целыми или символьными.

В зависимости от контекста переводится в различные конструкции. 

Операция `..` менее приоритетна чем сложение, но более приоритетна чем операция `in`.
То есть, следующие выражения можно писать без скобок:
```pascal 
x in 1..5
1+2 .. 3+4
```

В штатной ситуации `a..b` эквивалентно `Range(a,b)`:
```pascal  
(1..10).Select(x -> x*x).Println
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

### Оптимизация скорости срезов a[f:t]

Подобные срезы будут оптимизированы заменой оптимальный функций. Например, для строк `s[f:t]` будет заменена на `s.Substring`

### Оптимизация a.Indices

Для массивов, строк и списков появится единый цикл по индексам:
```pascal
foreach var i in a.Indices do
```
который будет переводиться в 
```pascal
for var i := 0 to a.Length-1 do

for var i := 0 to a.Count-1 do

for var i := 1 to a.Length do
```
в зависимости от типа контейнера

### Условная операция if

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


### Программы без begin-end (предложение)

Программы, не требующие подключения внешних модулей и описаний вне блока, можно писать без begin-end:
```
(1..10).Select(i -> 1..i).Println;
```

{% include links.html %}
