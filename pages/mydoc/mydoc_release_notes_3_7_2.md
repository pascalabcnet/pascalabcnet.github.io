---
title: Версия 3.7.2 (будущая версия)
keywords: release notes, what's new, announcements, new features
last_updated: 24.08.20
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_7_2.html
toс: false
folder: mydoc
---

## Стандартная библиотека

Появились методы расширения строк s.IsInteger и s.IsReal:

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





{% include links.html %}

