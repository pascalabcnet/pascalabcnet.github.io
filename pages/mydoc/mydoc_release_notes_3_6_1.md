---
title: Версия 3.6.0
keywords: release notes, announcements, what's new, new features
last_updated: 05.01.20
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_6_1.html
toс: false
folder: mydoc
---

## Pred и Succ с двумя параметрами 

Конструкция `a..b` задаёт диапазон элементов.

Значения a,b должны быть целыми или символьными.

В зависимости от контекста `a..b` переводится в различные конструкции. 

Операция `..` менее приоритетна чем сложение, но более приоритетна чем операция `in`.
То есть, следующие выражения можно писать без скобок:
```pascal 
x in 1..5
1+2 .. 3+4
```



{% include links.html %}
