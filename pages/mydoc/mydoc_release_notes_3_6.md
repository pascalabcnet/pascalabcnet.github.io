---
title: Версия 3.6.0
keywords: release notes, announcements, what's new, new features
last_updated: 05.01.20
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_6.html
tok: false
folder: mydoc
---

## Версия 3.6.0

### Предстоящие изменения

Новая синтаксическая конструкция для диапазона a..b. Взята из языка Kotlin.
Значенния a,b должны быть целыми или символьными.

В зависимости от контекста переводися в различные конструкции. 

Операция `..` менее приоритетна чем сложение, но более приоритетна чем операция `in`.
То есть, следующие выражения можно писать без скобок:
```
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
(x >= 1) and (x<=10)
```

Конструкция 
```pascal  
foreach var x in 1..10 do
```
переводится в 
```pascal
for var x := 1 to 10 do
```

{% include links.html %}
