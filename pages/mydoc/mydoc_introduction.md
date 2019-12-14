---
title: Введение
sidebar: mydoc_sidebar
permalink: mydoc_introduction.html
folder: mydoc
---

## Обзор

PascalABC.NET – это система программирования и язык Pascal нового поколения для платформы Microsoft .NET. Язык PascalABC.NET содержит все основные элементы современных языков программирования: модули, классы, перегрузку операций, интерфейсы, исключения, обобщенные классы, сборку мусора, лямбда-выражения. 

Система PascalABC.NET включает в себя также простую интегрированную среду, ориентированную на эффективное обучение современному программированию

## Примеры программ

```pascal
begin
  var (a,b) := ReadInteger2;
  var min := a;
  if b < min then
    min := b;
  Print(min);    
end.
```

{% include links.html %}
